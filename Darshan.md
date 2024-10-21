[Darshan](https://www.mcs.anl.gov/research/projects/darshan/) is an I/O profiling library that intercepts I/O calls from various HPC-relevant APIs and keeps track of how they are used at configurable levels of detail.

The Darshan experience these days is as follows:

```bash
# on the HPC system, load darshan, then compile and run the app
module load darshan
mpicc -o myapp myapp.c
srun -n 16 ./myapp

# find the log, then generate a report from it
darshan-config --log-path # figure out where your log went
python -m darshan summary /path/to/2024/7/16/glock_myapp_...darshan

# on your local laptop, copy the report and open it in a browser
scp mysupercomputer:/path/to/report.html .
open report.html
```