# Combination of EXPRESSCLUSTER and the HA function of Proxmox VE

## What effects does it have?

When you build a guest OS cluster using EXPRESSCLUSTER in a virtual environment, you can implement automatic recovery of a downed guest OS by utilizing the HA function of Proxmox VE.
For instance, even if the virtual infrastructure machine running the guest OS fails, the guest OS can be moved to another surviving virtual infrastructure machine within the Proxmox VE cluster.
This means that even if the EXPRESSCLUSTER cluster, which normally operates on 2 nodes, is reduced to operating on only 1 node, it can automatically recover to operating on 2 nodes.

## Procedures

1. Prepare a cluster and Ceph storage.
   Refer to [this document](/setup_cluster_and_ceph.md).

2. Create a two VMs.
   - Make sure it is necessary to specify a pool of Ceph as VM's storage in Disks tab.

3. Add the created VMs as HA resources.
   - Requested State should be "started".

4. Setup EXPRESSCLUSTER on the created VMs.

## Demonstoration

1. Start each node on a separate Proxmox VE host.
   - For instance, configure such that Node 1 is running on Host 1, and Node 2 is running on Host 2.

2. Confirm that all the failover groups and monitor resources are in a normal state with EXPRESSCLUSTER.
   - Assume the failover group is running on the Node 1 side.

3. Log into Host 1 where Node 1 is running and force it to shut down using the command: `poweroff -f -f`

4. Confirm that the failover group has failed over to Node 2.
   - Recognize that the cluster is now operating on just a single node.

5. After waiting, the HA function of Proxmox VE will detect that Node 1 is stopped, and Node 1 will be automatically started on a surviving host.

6. By waiting further, the rebooted Node 1 will return to the EXPRESSCLUSTER cluster, thereby letting the cluster return to operating on two nodes.
