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

## What is inside of Proxmox VE?

OS version

```
# cat /etc/os-release 
PRETTY_NAME="Debian GNU/Linux 12 (bookworm)"
NAME="Debian GNU/Linux"
VERSION_ID="12"
VERSION="12 (bookworm)"
VERSION_CODENAME=bookworm
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
```

Kernel version

```
# uname -r
6.8.4-2-pve
```

Installed packages

```
# dpkg -l | grep proxmox
ii  libproxmox-acme-perl                 1.5.0                               all          Proxmox ACME integration perl library
ii  libproxmox-acme-plugins              1.5.0                               all          Proxmox acme.sh wrapper for DNS API plugins
ii  libproxmox-backup-qemu0              1.4.1                               amd64        Proxmox Backup Server client library for QEMU
ii  libproxmox-rs-perl                   0.3.3                               amd64        PVE/PMG common perl parts for Rust perlmod bindings
ii  proxmox-archive-keyring              3.0                                 all          Proxmox APT archive keyring
ii  proxmox-backup-client                3.2.0-1                             amd64        Proxmox Backup Client tools
ii  proxmox-backup-file-restore          3.2.0-1                             amd64        Proxmox Backup single file restore tools for pxar and block device backups
ii  proxmox-backup-restore-image         0.6.1                               amd64        Kernel/initramfs images for Proxmox Backup single-file restore.
ii  proxmox-default-kernel               1.1.0                               all          Default Proxmox Kernel Image
ii  proxmox-firewall                     0.3.0                               amd64        Proxmox's nftables-based firewall written in rust
ii  proxmox-kernel-6.8                   6.8.4-2                             all          Latest Proxmox Kernel Image
ii  proxmox-kernel-6.8.4-2-pve-signed    6.8.4-2                             amd64        Proxmox Kernel Image (signed)
ii  proxmox-kernel-helper                8.1.0                               all          Function for various kernel maintenance tasks.
ii  proxmox-mail-forward                 0.2.3                               amd64        Proxmox mail forward helper
ii  proxmox-mini-journalreader           1.4.0                               amd64        Minimal systemd Journal Reader
ii  proxmox-offline-mirror-docs          0.6.6                               all          Proxmox offline repository mirror and subscription key manager
ii  proxmox-offline-mirror-helper        0.6.6                               amd64        Proxmox offline repository mirror and subscription key manager helper
ii  proxmox-termproxy                    1.0.1                               amd64        Wrapper proxy for executing programs in the system terminal
ii  proxmox-ve                           8.2.0                               all          Proxmox Virtual Environment
ii  proxmox-websocket-tunnel             0.2.0-1                             amd64        Proxmox websocket tunneling helper
ii  proxmox-widget-toolkit               4.2.1                               all          Core Widgets and ExtJS Helper Classes for Proxmox Web UIs

# dpkg -l | grep pve-
ii  libpve-access-control                8.1.4                               all          Proxmox VE access control library
ii  libpve-apiclient-perl                3.3.2                               all          Proxmox VE API client library
ii  libpve-cluster-api-perl              8.0.6                               all          Proxmox Virtual Environment cluster Perl API modules.
ii  libpve-cluster-perl                  8.0.6                               all          Proxmox Virtual Environment cluster Perl modules.
ii  libpve-common-perl                   8.2.1                               all          Proxmox VE base library
ii  libpve-guest-common-perl             5.1.1                               all          Proxmox VE common guest-related modules
ii  libpve-http-server-perl              5.1.0                               all          Proxmox Asynchrounous HTTP Server Implementation
ii  libpve-network-perl                  0.9.8                               all          Proxmox VE's SDN (Software Defined Network) stack
ii  libpve-notify-perl                   8.0.6                               all          Notify helper module.
ii  libpve-rs-perl                       0.8.8                               amd64        PVE parts which have been ported to Rust - Rust source code
ii  libpve-storage-perl                  8.2.1                               all          Proxmox VE storage management library
ii  libpve-u2f-server-perl               1.2.0                               amd64        Perl bindings for libu2f-server
ii  proxmox-kernel-6.8.4-2-pve-signed    6.8.4-2                             amd64        Proxmox Kernel Image (signed)
ii  pve-cluster                          8.0.6                               amd64        "pmxcfs" distributed cluster filesystem for Proxmox Virtual Environment.
ii  pve-container                        5.0.10                              all          Proxmox VE Container management tool
ii  pve-docs                             8.2.1                               all          Proxmox VE Documentation
ii  pve-edk2-firmware                    4.2023.08-4                         all          edk2 based UEFI firmware modules for virtual machines
ii  pve-edk2-firmware-legacy             4.2023.08-4                         all          edk2 based legacy 2MB UEFI firmware modules for virtual machines
ii  pve-edk2-firmware-ovmf               4.2023.08-4                         all          edk2 based UEFI firmware modules for virtual machines
ii  pve-esxi-import-tools                0.7.0                               amd64        Tools to allow importing VMs from ESXi hosts
ii  pve-firewall                         5.0.5                               amd64        Proxmox VE Firewall
ii  pve-firmware                         3.11-1                              all          Binary firmware code for the pve-kernel
ii  pve-ha-manager                       4.0.4                               amd64        Proxmox VE HA Manager
ii  pve-i18n                             3.2.2                               all          Internationalization support for Proxmox VE
ii  pve-lxc-syscalld                     1.3.0                               amd64        PVE LXC syscall daemon
ii  pve-manager                          8.2.2                               amd64        Proxmox Virtual Environment Management Tools
ii  pve-qemu-kvm                         8.1.5-5                             amd64        Full virtualization on x86 hardware
ii  pve-xtermjs                          5.3.0-3                             all          HTML/TypeScript based fully-featured terminal for Proxmox projects
```

Disk configuration

```
# lsblk
NAME                         MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sr0                           11:0    1    2K  0 rom  
vda                          253:0    0  100G  0 disk 
├─vda1                       253:1    0 1007K  0 part 
├─vda2                       253:2    0  512M  0 part 
└─vda3                       253:3    0 99.5G  0 part 
  ├─pve-swap                 252:0    0  7.7G  0 lvm  [SWAP]
  ├─pve-root                 252:1    0   35G  0 lvm  /
  ├─pve-data_tmeta           252:2    0    1G  0 lvm  
  │ └─pve-data-tpool         252:4    0 42.5G  0 lvm  
  │   ├─pve-data             252:5    0 42.5G  1 lvm  
  │   ├─pve-vm--100--disk--0 252:6    0   16G  0 lvm  
  │   ├─pve-vm--101--disk--0 252:7    0   16G  0 lvm  
  │   ├─pve-vm--100--disk--1 252:8    0    2G  0 lvm  
  │   └─pve-vm--101--disk--1 252:9    0    2G  0 lvm  
  └─pve-data_tdata           252:3    0 42.5G  0 lvm  
    └─pve-data-tpool         252:4    0 42.5G  0 lvm  
      ├─pve-data             252:5    0 42.5G  1 lvm  
      ├─pve-vm--100--disk--0 252:6    0   16G  0 lvm  
      ├─pve-vm--101--disk--0 252:7    0   16G  0 lvm  
      ├─pve-vm--100--disk--1 252:8    0    2G  0 lvm  
      └─pve-vm--101--disk--1 252:9    0    2G  0 lvm

# df -h
Filesystem            Size  Used Avail Use% Mounted on
udev                  3.9G     0  3.9G   0% /dev
tmpfs                 785M  896K  785M   1% /run
/dev/mapper/pve-root   35G   13G   20G  40% /
tmpfs                 3.9G   43M  3.8G   2% /dev/shm
tmpfs                 5.0M     0  5.0M   0% /run/lock
/dev/fuse             128M   16K  128M   1% /etc/pve
tmpfs                 785M     0  785M   0% /run/user/0
```

