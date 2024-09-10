---
title: Benchmarking blobfuse
tags:
  - benchmark
  - workload
---
This is the procedure I used to benchmark [[blobfuse]] using [[elbencho]]. The workload emulates multiple three-node jobs (`NODES_PER_GROUP`) running concurrently. Each three-node job reads 900 files in total, and each node within the three reads a non-overlapping subset of those 900 files (300 each). Each of those 900 files is 1024 MiB.

This workload is designed to emulate the process of starting up an inferencing cluster that will serve an LLM that relies on [[LLM training#Workload partitioning|pipeline parallelism]] across three nodes. Each three-node model replica has to load its own copy of the model from an Azure Blob storage account.

```yaml
# See https://github.com/Azure/azure-storage-fuse/blob/main/setup/baseConfig.yaml

allow-other: true
read-only: true

components:
  - libfuse
  - stream
# - file_cache
  - attr_cache
  - azstorage

libfuse:
  default-permission: 0444
  attribute-expiration-sec: 7200
  entry-expiration-sec: 7200
  negative-entry-expiration-sec: 7200

stream:
  block-size-mb: 32
  buffer-size-mb: 256
  max-buffers: 900

file_cache:
  path: /nvme/blobfuse_cache
  high-threshold: 90
  sync-to-flush: true
  refresh-sec: 7200

attr_cache:
  timeout-sec: 7200

azstorage:
  type: block
  account-name: daxdatawus3
  container: modeldata
  endpoint: https://daxdatawus3.blob.core.windows.net
  mode: msi
```

```bash
#!/usr/bin/env bash
#
# Before running this script, generate the hostfile
#
# Then start the elbencho service on all clients IF IT IS NOT ALREADY RUNNING:
#
#   clush --hostfile hostfile /usr/local/bin/elbencho --service

NODES_PER_GROUP=3
BLOCKSIZE=4M
THREADS=${THREADS:-1}
ELBENCHO_BIN="/usr/local/bin/elbencho"
OUTPUT_BASENAME="elb.bfstream.bs${BLOCKSIZE}.threads${THREADS}.direct"
CACHE_PATH="/mnt/modeldata"

# unlike other scripts, use an explicit hostfile instead of @all for
# reproducibility's sake
if [ ! -f "hostfile" ]; then
    echo "Couldn't find hostfile; aborting." >&2
    exit 1
fi

# reset blobfuse state
clush --hostfile hostfile --copy blobfuse-stream.yml --dest /home/$USER/blobfuse-stream.yml
clush --hostfile hostfile "blobfuse2 unmount $CACHE_PATH; sleep 5; blobfuse2 $CACHE_PATH --config-file /home/$USER/blobfuse-stream.yml"

# Manage and record cache state
(
    echo "Dropping page caches on all hosts"
    clush --hostfile hostfile "echo 1 | sudo tee /proc/sys/vm/drop_caches"
) 2>&1 | tee "${OUTPUT_BASENAME}.cachestate.txt"

hosts=()

TOTAL_NODES=$(wc -l hostfile | awk '{print $1}')
for i in $(seq 1 $NODES_PER_GROUP $TOTAL_NODES); do
    this_group=$(sed -n "$i,$((i+2))p" hostfile | paste -sd, -)
    # append this_group to hosts
    hosts+=($this_group)
done

JOBSCRIPT_OUTPUT_FILENAME="$PWD/${OUTPUT_BASENAME}.jobscript.out"
echo "elbencho starts at $(date +%s)" | tee -a "$JOBSCRIPT_OUTPUT_FILENAME"

# for each group of hosts, run a command
group_num=0
for group in "${hosts[@]}"; do
    echo "Running group ${group_num} on hosts ${group}"
    OUTPUT_BASENAME="elb.bfstream.bs${BLOCKSIZE}.threads${THREADS}.direct.groupnum${group_num}"
$ELBENCHO_BIN \
        --hosts "$group" \
    --resfile "$PWD/$OUTPUT_BASENAME.resfile.out" \
    --csvfile "$PWD/$OUTPUT_BASENAME.csvfile.csv" \
    --livecsv "$PWD/$OUTPUT_BASENAME.livecsv.csv" \
    --threads $THREADS \
    --size 1024M \
    --block $BLOCKSIZE \
    --direct \
    --sync \
    --read \
    --liveint 1000 \
    --nolive \
    $CACHE_PATH/shard.{0..899} &
    group_num=$((group_num+1))
done
wait

echo "elbencho ends at $(date +%s)" | tee -a "$JOBSCRIPT_OUTPUT_FILENAME"
```