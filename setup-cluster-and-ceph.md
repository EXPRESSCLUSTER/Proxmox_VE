# How to setup a cluster and Ceph for Proxmox VE

## Environment

- Hardware
  - CPU: Intel(R) Xeon(R) CPU E3-1220 V2 @ 3.10GHz
  - RAM: 16GB
  - Disk: 1TB

- Software
  - OS: Ubuntu Server 22.04
  - Virtualization: KVM + Qemu
  - Nested Virtualization: On (check the flag in `/sys/module/kvm_intel/parameters/nested`)
  - Proxmox version: 8.2 (proxmox-ve_8.2-1.iso)

## Procedures

1. Prepare three nodes (or VMs) for running Proxmox VE.
   - In this document, each node is referred to as pve01, pve02, pve03.

2. Create a cluster.
   1. Create a cluster on pve01.
   2. Copy the join information.
   3. Join pve02 and pve03 to the created cluster.
      - Paste the join information.
      - Enter the root password of pve01.

3. Setup Ceph.
   1. Install Ceph into all nodes (i.e. pve01, pve02 and pve03) via Proxmox VE console.
      - (Note: if the package installation process doesn't work properly, confirm a proxy setting by checking `/etc/apt/apt.conf.d/76pveproxy`)
   2. Create an OSD in each node. Its means that you will have three OSDs in the cluster.
      - It's recommended to prepare a new blank disk for the OSD.
   3. Create a Pool.
      - In this doc, the pool is referred to as "cluster-storage".

4. Add Monitors.
   - Maybe a monitor for pve01 is already created. So create monitors for pve02 and pve03.
  
5. Create a new VM.
   - Make sure that **it is necessary to specify "cluster-storage" as VM's storage** in Disks tab.

6. Add a created VM as an HA resource.
   - Requested State should be "started"
