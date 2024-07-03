# Combination of EXPRESSCLUSTER and the HA function of Proxmox VE

## What effects does it have?

When you build a guest OS cluster using EXPRESSCLUSTER in a virtual environment, you can implement automatic recovery of a downed guest OS by utilizing the HA function of Proxmox VE. For instance, even if the virtual infrastructure machine running the guest OS fails, the guest OS can be moved to another surviving virtual infrastructure machine within the Proxmox VE cluster. This means that even if the EXPRESSCLUSTER cluster, which normally operates on 2 nodes, is reduced to operating on only 1 node, it can automatically recover to operating on 2 nodes.

## Procedures

1. Prepare a cluster and Ceph storage.
   Refer to this doc (link)

2. Create a two VMs.
   - Make sure it is necessary to specify a pool of Ceph as VM's storage in Disks tab.

3. Add the created VMs as HA resources.
   - Requested State should be "started".

4. Setup EXPRESSCLUSTER on the created VMs.

## Demonstoration

TBA
