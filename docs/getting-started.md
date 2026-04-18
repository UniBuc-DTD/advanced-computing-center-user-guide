# Getting started

This page provides an overview of the resources and services the ACC provides, as well as means to access them.

## Available resources

The ACC offers several kinds of hardware, suited for different workloads:

- [The parallel _High-Performance Computing_ cluster (**HPC-CPU**)](hpc-cpu.md), offering nodes with lots of cores and RAM; excels at scientific computing and distributed CPU-intensive jobs;

- [The GPU computing node (**HPC-GPU**)](hpc-gpu.md), designed for Machine Learning (ML) and Artificial Intelligence (AI) workloads;

- [The hyperconverged infrastructure (**HCI**) cluster](hci.md), a virtualized environment for deploying educational / research applications.

We also offer a few services, hosted or backed by the ACC:

- A [**data repository**](https://data.unibuc.ro/) ([CKAN](https://ckan.org/) instance), intended to be used as a data archive for the UB community's research output.

- A [**JupyterHub** instance](https://jupyter.unibuc.ro) for running Jupyter notebooks on the ACC infrastructure (runs on the HCI, therefore it lacks GPU support). Authentication is done through the institutional Microsoft 365 account.

- [Application and system performance monitoring](https://github.com/UniBuc-DTD/advanced-computing-center-monitoring) system, based on open-source technologies such as Prometheus, OpenTelemetry, Grafana etc.

- Consulting, analysis, installation, configuration, monitoring, optimization and technical support for members of the UB academic community who are interested in deploying **educational** or **research** software. Contact us for more information.

This list will be expanded as we identify further needs of UB's students, teachers and researchers and develop corresponding solutions.

## Requesting access

The internal UB procedure for requesting access to the cluster or virtual machines is currently _work-in-progress_. Please contact the University's management or the ACC-UB administrators directly to obtain access.

When requesting access, please specify **which kind** of resource(s) you need access to (**HPC-CPU**, **HPC-GPU** and/or **HCI**), for **how long** you want to have access (e.g. a year), **how many** resources you will need to use and the **motivation** for requesting them (e.g. access to the GPU cluster for training some ML models; a virtual machine for a data analysis / research app; access to the CPU cluster to run some simulations etc).

The decision for granting you access to this shared infrastructure is subject to the discretion of the University's upper management.


## Connecting to the resources

The computational resources of the ACC are available primarily **from the internal UB network** (e.g. from the internet or Wi-Fi at the faculty/office/rectorate).

In well-motivated situations, you may request the provisioning of a [Virtual Private Network (VPN)](https://en.wikipedia.org/wiki/Virtual_private_network) account to be able to access the compute nodes remotely over a secured connection. You can request VPN access from the ACC administrators before or after your user account has been created. The same credentials will be used for the VPN connection as for your main ACC user account. See [the VPN guide](vpn.md) for more information.

**Note:** If you have a VPN account provided by the University's IT department, which uses Cisco's AnyConnect client, then you can use that as well in order to connect to the ACC. The UniBuc VPN is managed by [the IT&C Department](https://unibuc.ro/despre-ub/organizare/administratie/directia-itc/), not by the ACC-UB infrastructure administrators.

## Networked access

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

On the CPU and GPU nodes, only system administrators have `sudo` rights (root access). Regular users are encouraged to:

- Use the tools and libraries available through the [module system](#module-system);

- Request the addition of new modules by asking the ACC administrators to install and configure them;

- Install the tools they need locally, using package managers which do not require root (see below for programming language-specific examples).

### Examples

- For [**Python**](https://www.python.org/), you can activate a specific version of the CPython interpreter by using `module add tools/python/<version>`, use the system install (not recommended, since the version might change between system updates) with a [virtual environment](https://docs.python.org/3/library/venv.html), or (recommended) use a package manager such as [uv](https://docs.astral.sh/uv/) or [Conda](https://anaconda.org/) (or [Miniconda](https://www.anaconda.com/docs/getting-started/miniconda/main)).

    For example, to install `uv` run `curl -LsSf https://astral.sh/uv/install.sh | sh` then logout and log back in. You can then install the desired version of Python using `uv python install <version>` and proceed to create projects or install PyPI packages globally.

- For [**Julia**](https://julialang.org/), use [`juliaup`](https://julialang.org/install/) to manage your Julia install, environments and packages.

- For **C/C++/Fortran**, activate a specific version of the GCC suite by using `module add tools/gcc/<version>`, install and use [Spack](https://spack.io/) or [Miniconda](https://www.anaconda.com/docs/getting-started/miniconda/main) to install the compilers and the required libraries.

- For [**Node.js**](https://nodejs.org/en), use a Node version manager such as [`fnm`](https://github.com/Schniz/fnm).

If you encounter any issues with installing these tools or would like to install some software which doesn't support non-root installations, please contact us.

## System-specific guides

For more detailed usage instructions, consult the guide for the [HPC-CPU cluster](hpc-cpu.md), the [HPC-GPU node](hpc-gpu.md) or the [HCI virtualization platform](hci.md).
