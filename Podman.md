I use Podman to do HPC development in MacOS because some tools (like [[Darshan]]) rely on Linux-specific hooks. Here's the process:

## Create the container image

Create a base container image:

```bash
podman pull ubuntu:22.04
```

Then launch an interactive session within the container to set up the dev environment:

```bash
# Launch the container
podman run -it --name hpc-dev ubuntu:22.04 bash

# Once inside the container
apt update && apt install -y gcc g++ git build-essential
```

After setting up the environment, commit the container’s state so it can be reused without reinitializing everything:

```bash
podman commit hpc-dev my-hpc-dev-env
```

The above can be done whenever changes worth saving are made.

## Create a data volume

Create a persistent volume to store data outside the container:

```bash
# Create a persistent volume
podman volume create hpc-dev-vol

# Launch the container with the volume mounted
podman run -it --name hpc-dev --volume hpc-dev-vol:/workspace my-hpc-dev-env bash
```

Or just bind mount a local directory:

```bash
podman run -it --name hpc-dev -v /path/on/macos:/workspace my-hpc-dev-env bash
```

## Re-use the container

Once the container is exited, it can be stopped and started again without losing the environment:

```bash
podman start -ai hpc-dev
```

The container can also be used in other systems:

```bash
# Export the container
podman save -o hpc-dev.tar my-hpc-dev-env

# Import the container
podman load -i hpc-dev.tar
```

## Machine configuration

The default Podman install on MacOS only gave me 4 cores and 2 GB of RAM. You can change this:

```bash
podman machine stop
podman machine set --cpus 8
podman machine set --memory 8192
podman machine start

podman run --rm -it --name hpc-dev --volume hpc-dev-vol:/workspace my-hpc-dev-env bash
```

To see the number of cores on MacOS:

```bash
sysctl -n hw.ncpu
```