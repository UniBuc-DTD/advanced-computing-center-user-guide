# Advanced Computing Center User Guide

## Description

This repository contains documentation for the users of the [**Advanced Computing Center** (ACC)](https://edis.unibuc.ro/data-center-si-laboratoare-de-acces/) of the [University of Bucharest](https://unibuc.ro/?lang=en).

## Available resources

The ACC benefits from several kinds of hardware, suited for different workloads:

- The parallel _High-Performance Computing_ cluster (**HPC-CPU**), offering nodes with lots of cores and RAM; excels at scientific computing and distributed CPU-intensive jobs;

- The GPU computing node (**HPC-GPU**), designed for Machine Learning (ML) and Artificial Intelligence (AI) workloads;

- The hyperconverged infrastructure (**HCI**) cluster, a virtualized environment for deploying educational / research applications.

We also offer a few services, hosted or backed by the ACC:

- A [**data repository**](https://data.unibuc.ro/) ([CKAN](https://ckan.org/) instance), intended to be used as a data archive for the UB community's research output.

- A [**JupyterHub** instance](https://jupyter.unibuc.ro) for running Jupyter notebooks on the ACC infrastructure (runs on the HCI, therefore it lacks GPU support). Authentication is done through the institutional Microsoft 365 account.

- [Application and system performance monitoring](https://github.com/UniBuc-DTD/advanced-computing-center-monitoring) system, based on open-source technologies such as Prometheus, OpenTelemetry, Grafana etc.

- Consulting, analysis, installation, configuration, monitoring, optimization and technical support for members of the UB academic community who are interested in deploying **educational** or **research** software. Contact us for more information.

This list will be expanded as we identify further needs of UB's students, teachers and researchers and develop corresponding solutions.

## Requesting resources

The internal UB procedure for requesting access to the cluster or virtual machines is currently _work-in-progress_. Please contact the University's management or the ACC-UB administrators directly to obtain access.

When requesting access, please specify **which kind** of resource(s) you need access to (**HPC-CPU**, **HPC-GPU** and/or **HCI**), for **how long** you want to have access (e.g. a year), **how many** resources you will need to use and the **motivation** for requesting them (e.g. access to the GPU cluster for training some ML models; a virtual machine for a data analysis / research app; access to the CPU cluster to run some simulations etc). The decision for granting you access to this shared infrastructure is subject to the decision of the University's upper management.

## Connecting to the resources

The computational resources are available **exclusively from the internal UB network** (e.g. from the internet or Wi-Fi at the faculty/office/rectorate).

In exceptional situations, you may request a [Virtual Private Network (VPN)](https://en.wikipedia.org/wiki/Virtual_private_network) account to be able to access them remotely over a secured connection.

The VPN solution will be based either on Fortinet (for accessing the internal ACC network only) or Cisco (for accessing the whole UB intranet):
- For Fortinet, follow the received instructions to install and configure the latest stable version of [FortiClient](https://www.fortinet.com/support/product-downloads).
- For Cisco, follow the instructions received by e-mail to download and install the latest version of the Cisco AnyConnect Secure Mobility Client.

After ensuring you are connected to the internal ACC/UB network (you can verify this by running a command such as [`ping hpc-ctrl01.acc-ub.local`](https://www.lifewire.com/ping-command-2618099)), there are differnet ways for accessing the internal resources:

- For the HPC-CPU cluster, connect to the **controller node** (`hpc-ctrl01.acc-ub.local`) using a [SSH client](https://en.wikipedia.org/wiki/Secure_Shell), with the (initial) credentials you were provided for your account:

  `ssh example@hpc-ctrl01.acc-ub.local`

- For the HPC-GPU cluster, connect to the **GPU node** (`hpc-nodegpu01.acc-ub.local`) using a [SSH client](https://en.wikipedia.org/wiki/Secure_Shell), with the credentials you were provided for your account:

  `ssh example@hpc-nodegpu01.acc-ub.local`

- For a virtual machine on the HCI cluster, connect either using [SSH](https://en.wikipedia.org/wiki/Secure_Shell) to the domain or IP address of the VM you requested or, if the application you requested has a web UI, connect directly to it at `http://10.23.21.<example>'`.

If you encounter issues with connecting to the infrastructure (after your access request has been approved and confirmed!), please contact the ACC administrators.

## Module system

Scientific software tools and libraries are globally available to all users through a [modules system](https://hpc-wiki.info/hpc/Modules), based on the [Environment Modules](https://modules.readthedocs.io/en/latest/index.html) package.

To **show** all available software modules, run `module avail`.

The ones listed under `/mnt/modules/modulefiles` are the ones installed/maintained by the ACC team. To activate a module, use the `module enable` command followed by its name. For example: `module enable mpi/openmpi/5.0.9` to enable the OpenMPI implementation, version 5.0.9, compiled by us, with optimizations specific for the HPC system.

Multiple modules can be active at the same time. To see which modules you currently have active, use `module list`. To unload/deactivate all previously enabled modules, run `module purge`.

## Installing tools

On the CPU and GPU nodes, only the system administrators have `sudo` rights / root access. Regular users are encouraged to install all of the tools they need locally, using package managers which do not require root.

- For [**Python**](https://www.python.org/), you can either use the system install (not recommended, since the version might change between system updates) with a [virtual environment](https://docs.python.org/3/library/venv.html), or (recommended) use a package manager such as [`uv`](https://docs.astral.sh/uv/) or [`conda`](https://anaconda.org/) (preferrably [Miniconda](https://www.anaconda.com/docs/getting-started/miniconda/main)).

  For example, to install `uv` run `curl -LsSf https://astral.sh/uv/install.sh | sh` then logout and log back in. You can then install the desired version of Python using `uv python install <version>` and proceed to create projects or install PyPI packages globally.

- For [**Julia**](https://julialang.org/), use [`juliaup`](https://julialang.org/install/) to manage your Julia install and packages.

- For **C/C++/Fortran**, use [Miniconda](https://www.anaconda.com/docs/getting-started/miniconda/main) to install the compilers and the required libraries.

- For [**Node.js**](https://nodejs.org/en), use a Node version manager such as [`fnm`](https://github.com/Schniz/fnm).

If you encounter any issues with installing these tools or would like to install some software which doesn't support non-root installations, please contact us.

## Running jobs on the HPC-CPU cluster

The HPC-CPU cluster is managed using the [slurm](https://slurm.schedmd.com/overview.html) job scheduler. Please consult its [quickstart guide](https://slurm.schedmd.com/quickstart.html) for an overview of the system.

To launch a job spanning multiple compute nodes, connect to the **controller node** and either use the `srun` command (for interactive jobs) or the `sbatch` command (for batch jobs / scripts).

If your program/tool is using the [Message-Passing Interface](https://en.wikipedia.org/wiki/Message_Passing_Interface) for parallelization, you can run the `mpirun` command under Slurm to automatically distribute the work across all of the nodes requested by your job.

Most programs with support for parallelization will also provide instructions for how to run the software on a cluster with Slurm.

### Using `mpi4py` (MPI for Python)

If you use [the Python bindings for MPI](https://mpi4py.readthedocs.io/en/stable/), you should use them together with an MPI implementation available as a module on the HPC system (i.e. OpenMPI or MPICH). If you use `conda` to manage your packages, you might find it automatically installs a generic MPI implementation, which will lead to errors, such as:

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

## Using the GPUs on the HPC-GPU node

The GPU node is equipped with a total of 8 $\times$ NVIDIA H100 cards, each with 80 GiB of VRAM. However, be aware that some of the GPUs are dedicated/reserved for certain faculties or research groups, so not all of them might be available at all times for interactive or batch use. If unsure, please contact prof. Ciprian Păduraru, who administers the GPU node.

To see the current utilisation of the GPUs, use the `nvidia-smi` command.
