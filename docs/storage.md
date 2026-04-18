# Storage

This page provides an overview of the data storage components of the ACC.

## Storage-Area Network

The primary data store for the ACC is an [HPE Alletra MP B10000](https://www.hpe.com/us/en/alletra-storage-mp-b10000.html) node, providing 50 TiB+ of all-flash block storage, with the possibility of expansion. The volumes are exported using NVMe-over-TCP, over redundant 25 Gbps link (meaning up to 3 GiB/s read/write speeds, in the best case scenario).

The HCI cluster also features a vSphere-managed [vSAN](https://www.vmware.com/products/cloud-infrastructure/vsan) storage network, with 200+ TiB of high-speed all-flash storage. This data store is available only for virtualization-based workloads.

## Backup

For data backup and long-term archiving, we have an [HPE StoreOnce 3660](https://buy.hpe.com/in/en/storage/disk-storage-systems/storeonce-systems/hpe-storeonce-3660-80tb-base-system/p/r6u02a) system with 50 TiB storage capacity and advanced deduplication support. We use [Veeam Backup & Replication](https://www.veeam.com/products/veeam-data-platform/backup-recovery.html) to manage and automate the backup & recovery process.

We only back up certain disks and partitions by default (the system's root filesystem, certain critical virtual machines etc). In particular, the users' home directories are _**not**_ backed up as part of our regular jobs (since they often contain a lot of temporary files, job output, possibly sensitive data etc). If you have certain files or folders which you want to be included in the backup, please contact us.
