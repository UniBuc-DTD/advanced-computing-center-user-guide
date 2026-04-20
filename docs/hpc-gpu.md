# High Performance Compute - GPU node

This page contains details on the GPU node.

## Hardware configuration

The sole GPU node we have is equipped with the following:

- **CPU**: Intel Xeon Platinum 8480+ (2 sockets, each with 56 cores; due to hyperthreading, we have 224 hardware threads)
- **Memory**: 2 TiB of RAM
- **Storage**: the root filesystem is installed on 2 NVMe drives in RAID 1 configuration (so we only have 1.7 TiB available space in total). There are 8 more NVMe drives used by various faculties and research groups, each with a capacity of 3.5 TiB. The node is also connected to the shared network storage (available under `/mnt`), which has a capacity of 16+ TiB.
- **GPUs**: 8 × NVIDIA H100, 80 GiB of VRAM each

Since the cores and memory are shared with all other users on the node, please be mindful of your usage.

## Using the GPUs on the HPC-GPU node

Be aware that some of the GPUs are dedicated/reserved for certain faculties or research groups, so not all of them might be available at all times for interactive or batch use. If unsure, please contact prof. Ciprian Păduraru, who administers the GPU node.

To see the current utilisation of the GPUs, use the `nvidia-smi` command.
