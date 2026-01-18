# Homelab Documentation


---
**Host:** `pve`  
**Role:** Primary Hypervisor & Storage Node

### 1\. Hardware Specifications

| Component | Spec | Notes |
| --- | --- | --- |
| **Model** | Dell Inspiron 14 7000 | Lid closed, power settings tweaked |
| **CPU** | Intel Core i7-7500U | 2 Cores / 4 Threads @ 2.70GHz |
| **RAM** | 8 GB DDR3 | Non-ECC |
| **Boot Drive** | 128 SSD | Proxmox OS + ISOs (`local`) |
| **Data Drive** | External HDD (USB) | Mounted as `HDD-Ext` (Media) |

### 2\. Network Interface

- **Physical:** `enp2s0` (Ethernet) - **Priority**
- **Bridge:** `vmbr0` → Mapped to Physical
- **Static IP:** `10.10.10.10`
- **Gateway:** `10.10.10.1`
- **DNS:** `10.10.10.5` (AdGuard)

### 3\. Storage Layout

- **local (pve):** Backups, ISO images, CT Templates.
- **local-lvm:** VM Disks (Block Storage).
- **Media-Mount:** Passed through to VM 111 via VirtIO or Bind Mount.

### 4\. Admin Notes

- **Power Loss:** Laptop battery acts as short-term UPS.
- **Maintenance:** Run `apt update` monthly.
- **Access:** SSH on Port 22 allowed from local subnet only.