# Proxmox VE Quick Start Guide (Physical Server Installation)

This guide will walk you through installing **Proxmox Virtual Environment (VE)** on a **physical server**.

---

## Prerequisites

Before you begin, make sure you have the following:

- A physical server with at least **64-bit CPU** (Intel VT-x or AMD-V support required for virtualization).
- A **bootable USB drive** with the latest **Proxmox VE ISO** (downloaded from [Proxmox](https://www.proxmox.com/en/downloads)).
- A stable **network connection**.
- An IP address for your Proxmox VE server.

---

## Steps to Install Proxmox VE

### 1. Boot Your Physical Server from the USB Drive

1. Insert the **bootable USB drive** into your physical server.
2. Power on the server and press the key (e.g., **F2**, **Delete**, or **Esc**) to enter the **BIOS/UEFI settings**.
3. Change the boot order to **boot from the USB drive**.
4. Save the settings and exit the BIOS. The server will boot into the Proxmox VE installer.

### 2. Install Proxmox VE on the Physical Server

1. Follow the on-screen instructions to start the installation.
2. Select the target disk where Proxmox VE should be installed (this is typically the primary disk of the server).
3. Configure **timezone**, **keyboard layout**, and **network settings** as prompted.
4. Set a strong **root password** and provide an **email address** for system notifications.
5. After the installation completes, **remove the USB drive** and **reboot** the server.

### 3. Access the Proxmox VE Web Interface

1. Once the server reboots, open a web browser on a computer connected to the same network.
2. Enter the following URL in the browser:

```
https://<Server-IP>:8006
```

Replace `<Server-IP>` with your server's IP address.

3. Log in with the **root** user and the password you set during installation.

### 4. Configure Network Settings

1. In the Proxmox VE web interface, navigate to **Datacenter > Node > Network**.
2. Ensure that your **network interface** and **bridge (vmbr0)** are correctly configured to allow virtual machines to connect to the physical network.
3. Adjust the settings as needed to match your network infrastructure.

### 5. Create Virtual Machines

1. In the Proxmox dashboard, click **Create VM**.
2. Assign a **VM ID** and a **name** for your virtual machine.
3. Select the **ISO image** of the operating system you want to install (you can upload this via **local storage** or mount it from an external source).
4. Allocate **CPU cores**, **RAM**, and **storage** to your virtual machine.
5. Configure the **network settings**, assigning the VM to the network bridge (e.g., **vmbr0**).
6. Start the VM and proceed with the operating system installation.

### 6. Add Storage for Virtual Machines

1. To add more storage for VMs, go to **Datacenter > Storage**.
2. You can add **local storage**, **NFS shares**, **iSCSI targets**, or external storage (e.g., via **Ceph**).
3. Configure the storage based on your infrastructure and needs.

### 7. Set Up Backups and Snapshots

1. It's crucial to set up regular backups for your VMs. This can be configured under **Datacenter > Backup**.
2. Schedule automatic backups and choose the storage location.
3. Additionally, use **snapshots** to capture the state of your VMs for quick rollback in case of issues.

---

You are now ready to use Proxmox VE on your physical server for managing and deploying virtual machines. Happy virtualization!
