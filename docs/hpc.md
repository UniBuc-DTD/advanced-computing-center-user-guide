# High-Performance Computing Cluster

## Summary

The [high-performance compute](https://en.wikipedia.org/wiki/High-performance_computing) (HPC) cluster is the "brain" of the ACC-UB infrastructure, featuring powerful hardware for solving the most advanced scientific problems, including: numerical simulations, big data processing, machine learning etc.

The servers run [Linux](https://en.wikipedia.org/wiki/Linux), more specifically [Ubuntu Server](https://ubuntu.com/download/server) 24.04. They feature a shared filesystem ([NFS v4.2](https://en.wikipedia.org/wiki/Network_File_System) and [NVMe over TCP](https://en.wikipedia.org/wiki/NVMe_over_TCP)), [InfiniBand](https://en.wikipedia.org/wiki/InfiniBand) for high-speed inter-node communication and [RDMA](https://en.wikipedia.org/wiki/Remote_direct_memory_access), as well as an [environment modules](https://envmodules.io/) system. Job scheduling is handled by [SLURM](slurm.md).

## CPU nodes

### Hardware configuration

The compute system currently consists of 22 identical nodes, with the following hardware specifications:

- **CPU:** AMD EPYC 7713 (2 sockets, each with 64 cores, for 128 physical cores; due to hyperthreading, we have 256 hardware threads)

- **Memory:** 2 TiB of RAM

- **Storage**: the root filesystem is installed on 3 NVMe drives in RAID 5 configuration (so we only have 3.5 TiB available space in total). Each node is also connected to the shared network storage (available under `/mnt`), which has a capacity of 32+ TiB.

The nodes `ctrl01` and `db01` are used for additional tasks (SLURM controller, SLURM accounting database, NFS v4.2 server etc), so parts of their cores and memory are reserved for system tasks.

### CPU partitions

The HPC-CPU nodes are divided into multiple Slurm **partitions** (queues), with different characteristics, as shown in the figure and table below.

![Slurm queues for CPU nodes](images/cpu-partitions-diagram.png)

| Partition | Max nodes | Max job run time | Default job run time |
| --------- | --- | --- | --- |
| `debug` | 2 | 1 hour | 30 minutes |
| `interactive` | 2 | 4 hours | 1h |
| `cpu-wide` | 11 | 24 hours | 4h |
| `cpu-medium` | 8 | 36 hours | 6h |
| `cpu-standard` | 4 | 3 days | 8h |
| `cpu-narrow` | 2 | 5 days | 24h |
| `cpu-long` | 1 | 12 days | 24h |


We recommend users to start with the most conservative configuration for their job (for example, it's best to start on the `debug` or `interactive` partition to make sure your job script works). Once you have an idea of how your workload scales, you can start running on the `cpu-wide` and `cpu-medium` queues. If your job requires a lot of time to finish and cannot be interrupted easily, use the `cpu-standard` or `cpu-narrow` queues.

**Please note that, while we do our best to ensure 100% uptime of the compute cluster (including through hardware redundancy), unexpected situations are always a possibility.** We encourage users to use [checkpointing](https://hpc.nmsu.edu/discovery/slurm/backfill-and-checkpoints/#_introduction_to_checkpoint) and regularly save their jobs' running state to avoid data loss. Slurm can also be configured to [send a custom interrupt signal](https://docs.gwdg.de/doku.php?id=en:services:application_services:high_performance_computing:running_jobs_slurm:signals) to your job script before the time limit is reached, to allow it to save its progress and stop.

## GPU node

### Hardware configuration

The GPU node we have is equipped with the following:

- **CPU**: Intel Xeon Platinum 8480+ (2 sockets, each with 56 cores; due to hyperthreading, we have 224 hardware threads)
- **Memory**: 2 TiB of RAM
- **Storage**: the root filesystem is installed on 2 NVMe drives in RAID 1 configuration (so we only have 1.7 TiB available space in total). There are 8 more NVMe drives used by various faculties and research groups, each with a capacity of 3.5 TiB. The node is also connected to the shared network storage (available under `/mnt`), which has a capacity of 32+ TiB.
- **GPUs**: 8 × NVIDIA H100, 80 GiB of VRAM each

Since the cores and memory are shared with all other users on the node, please be mindful of your usage.

### Using the GPUs on the HPC-GPU node

To access the GPUs, your job must be submitted to the `gpu` partition and must explicitly request them by using the `--gpus` flag in SLURM.

Be aware that some of the GPUs are currently dedicated/reserved for certain research groups, so not all of them might be available to SLURM.

### Running interactive jobs on the GPU node

If you want to run a shell/interactive job on the GPU node (e.g. a [Jupyter](https://jupyter.org/) notebook), you can request a job allocation for a script which does nothing but wait:

```shell
sbatch --partition=gpu --gpus=1 --time=01:00:00 --account=acc-YOUR-ACCOUNT <<< '#!/bin/bash
sleep 1h
'
```

Change the job's time limit, number of GPUs and the `sleep` time as required.

Once the job starts running, connect to the node using `ssh`, as usual. The [`pam_slurm_adopt`](https://slurm.schedmd.com/pam_slurm_adopt.html) module will ensure your SSH session gets "adopted" into the running job allocation. You can then start the Jupyter server (or any other app) and use [SSH tunelling](https://www.ssh.com/academy/ssh/tunneling-example) to forward the required ports back to your computer.
