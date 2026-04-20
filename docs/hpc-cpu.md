# High Performance Compute - CPU cluster

This page contains details on the CPU cluster.

## Hardware configuration

The compute system currently consists of 20 identical nodes, with the following hardware specifications:

- **CPU:** AMD EPYC 7713 (2 sockets, each with 64 cores, for 128 physical cores; due to hyperthreading, we have 256 hardware threads)

- **Memory:** 2 TiB of RAM

- **Storage**: the root filesystem is installed on 3 NVMe drives in RAID 5 configuration (so we only have 3.5 TiB available space in total). Each node is also connected to the shared network storage (available under `/mnt`), which has a capacity of 16+ TiB.

We have two extra nodes (`ctrl01` and `db01`), with identical hardware specs, which are used for additional tasks (Slurm controller, Slurm accounting database, NFS v4.2 server etc), so parts of their cores and memory are reserved for system tasks.

## Running jobs on the HPC-CPU cluster

The HPC-CPU cluster is managed using the [Slurm](https://slurm.schedmd.com/overview.html) job scheduler. Please consult the [Slurm quickstart guide](https://slurm.schedmd.com/quickstart.html) for an overview of the system.

To launch a job spanning multiple compute nodes, connect to the **controller node** and either use the `srun` command (for interactive jobs) or the `sbatch` command (for batch jobs / scripts).

If your program/tool is using the [Message-Passing Interface](https://en.wikipedia.org/wiki/Message_Passing_Interface) for parallelization, you can run the `mpirun` command under Slurm to automatically distribute the work across all of the nodes requested by your job.

Most programs with support for parallelization will also provide instructions for how to run the software on a cluster with Slurm.

### Slurm web interface

While your job runs, you can check its status and resource utilisation by running the `squeue` command. To allow for friendlier and more interactive monitoring of a job's status, we have also installed and configured the [Slurm web](https://slurm-web.com/) dashboard.

To access the web UI, navigate to <https://hpc-ctrl01.acc-ub.local> (your browser will likely warn you about untrusted certificates; accept them or skip the warning), then log in with your ACC user credentials. You'll be able to check the system's status, load, job queue and job details.

### Using `mpi4py` (MPI for Python)

If you use [the Python bindings for MPI](https://mpi4py.readthedocs.io/en/stable/), you should use them together with an MPI implementation available as a module on the HPC system (i.e. OpenMPI or MPICH). To use this installation, activate the corresponding modules: `module add mpi/openmpi` or `module add mpi/mpich`.

If you use `conda` to manage your packages, you might find it automatically installs a generic MPI implementation, which will lead to errors, such as:

```
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      hpc-ctrl01.acc-ub.local
Framework: psec
Component: munge
```

You should follow [these instructions from `conda-forge`](https://conda-forge.org/docs/user/tipsandtricks/#using-external-message-passing-interface-mpi-libraries) on how to use an external MPI library. You will have to run a command such as `conda install "openmpi=X.Y.*=external_*"` or `conda install "mpich=X.Y.*=external_*`, where `X.Y` should match the major and minor version numbers for your selected MPI implementation module. This will also require you to have the corresponding module active whenever you run a Python program which uses `mpi4py`.
