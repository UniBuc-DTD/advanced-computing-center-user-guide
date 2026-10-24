# Network

This page provides an overview of the network infrastructure of the ACC.

## Network equipment

The ACC's internal network backbone consists of:

- 2x core [Juniper QFX5120-48Y-8C](https://www.juniper.net/documentation/us/en/hardware/qfx5120/topics/topic-map/qfx5120-system-overview.html) switches (in an active/backup virtual chassis setup for redundancy), providing up to **4 Tbps** bidirectional switching bandwidth. The core switch is connected to the firewall and to the HPC and HCI switches.

- 2x [Juniper EX4650-48Y-8C](https://www.hpe.com/us/en/collaterals/collateral.a00150780enw.html) switches (virtual chassis setup for redundancy) for the HPC nodes, delivering up to **4 Tbps** of bandwidth. The nodes are connected to the switches using redundant SFP connections.

- 2x [Juniper EX4650-48Y-8C](https://www.hpe.com/us/en/collaterals/collateral.a00150780enw.html) switches (virtual chassis setup for redundancy) for the HCI nodes, delivering up to **4 Tbps** of bandwidth. The nodes are connected to the switches using redundant SFP connections.

- 1x [NVIDIA Mellanox QM8700](https://www.nvidia.com/en-au/networking/infiniband/qm8700/) InfiniBand switch, providing up to **16 Tbps** of non-blocking low-latency bandwidth. Each node is connected to the switch over a **100 Gbps** link.

- 2x [FortiGate 400F](https://www.fortinet.com/resources/data-sheets/fortigate-400f-series) next-generation firewall (in an active/backup redundant configuration), connected through a **1 Gbps uplink** to the public internet. We plan to also connect an additional 10 Gbps uplink to [RoEduNet](https://www.roedu.net/) in the future.
