# How to setup Proxmox VE on KVM/Qemu environment

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

1. Create a new VM for Proxmox VE.  
   (Command line example)
```
$ virt-install --name proxmox-test \
     --vcpus 4 --ram 4096 --network network=default \
     --connect qemu:///system \
     --disk path=/var/lib/libvirt/images/proxmox-via-cli.qcow2,size=100 \
     --os-variant debian12 \
     --cdrom /home/user/proxmox-ve_8.2-1.iso
```

2. Select "Install Proxmox VE (Graphical)".

3. Follow the on-screen instructions of the Proxmox VE Installer.
   1. Read an EULA and click "I agree".
   2. Click "Next".
   3. Specify Country, Time zone and Keyboard Layout. Then click "Next".
   4. Input your password and Email. Then click "Next".
   5. Specify the network configuration.  
      For example,
      - Management Interface: enp1s0 (virtio_net)
      - Hostname (FQDN): proxmox.test.local
      - IP Address (CIDR): 192.168.122.100 / 24
      - Gateway: 192.168.122.1
      - DNS Server: 192.168.122.1
   7. Check a configuration summary and click "Install".
   8. Wait for completion of the installation process.

4. Launch a web browser and access to https://192.168.122.100:8006.  
   Login with the following user account.
   - User name: root
   - Password: <the_password_you_entered>
