# Virtualization platform

The ACC also has 10 nodes as part of a hyperconverged infrastructure cluster, based on [VMware vSphere](https://www.vmware.com/products/cloud-infrastructure/vsphere). Virtual machines are available for UB academic members who request them for teaching or research purposes.

## Hardware configuration

The 10 nodes of the HCI cluster each have the following specs:

- **CPU**: AMD EPYC 7513 (128 hardware threads)
- **Memory**: 2 TiB of RAM
- **Storage**: over 200 TiB available in vSAN, also connected to [the storage offered by HPE Alletra](storage.md).

The nodes are all connected together in a high-availability cluster.

## Requesting virtual machines

Please contact the ACC system administrators if you require a virtual machine.

We can provide Linux-based VMs (Ubuntu, Alma etc.) or Windows-based VMs (Windows 11 with Remote Desktop support, Windows Server 2025 etc.). Certain proprietary software might require special licenses (e.g. Windows license keys), which you must provide to the ACC administrators for the installation and configuration of the requested applications.

We can either provide you with administrative credentials to the virtual machine (e.g. SSH user with `sudo` rights or Windows admin user) or install and manage a given software suite (e.g. an application server) for you.
