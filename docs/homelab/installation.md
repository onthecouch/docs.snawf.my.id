**Goal:** This page covers the preparation and step-by-step installation of Proxmox VE on bare metal hardware.

### 1. Hardware &amp; Prep Prerequisites

*Before starting, ensure the hardware is ready.*

- **Download ISO:** Latest Proxmox VE ISO from [proxmox.com](https://www.proxmox.com/en/downloads).
- **Create Boot Media:** Flash ISO to USB (using Rufus or Etcher).
- **Network Info:** Have a static IP address picked out (Proxmox *needs* a static IP).
    
    
    - *IP:* `10.10.10.1`
    - *Gateway:* `10.10.10.1`
    - *DNS:* `1.1.1.1` or `8.8.8.8` (or your local router).
- **Hardware Check:** Verify CPU supports Virtualization (Intel VT-x / AMD-V).

### 2. BIOS/UEFI Settings

*Crucial steps to ensure the hypervisor runs correctly.*

- **Virtualization:** Enabled (Look for Intel VT-d / AMD-Vi).
- **Secure Boot:** Disabled (Often causes issues with custom drivers or older versions).
- **Boot Order:** Set USB Drive as Priority #1.

### 3. The Installation Steps

1. **Boot:** Insert USB and boot the server. Select "Install Proxmox VE".
2. **EULA:** Accept the license agreement.
3. **Target Disk:** Select the drive to install OS on.

!!! warning "Warning:"
    **Warning:** This drive will be completely wiped!

1. **Country/Time:** Set your Location and Time Zone.
2. **Credentials:** Set the `root` password and a valid email address (for system alerts).
3. **Network Configuration:**
    
    
    - **Hostname:** e.g., `pve.hd pve`
    - **IP Address:** (Enter the static IP from Step 1)
4. **Summary:** Review all settings and click **Install**.

### 4. First Boot &amp; Verification

Once the installation finishes, the system will reboot. Remove the USB drive.

- Wait for the command line login screen to appear.
- Note the IP address shown on the screen (e.g., `https://10.10.10.1:8006`).
- Open a web browser on another computer and navigate to that URL.
    
    
    - *Note: You will get an SSL Security Warning. This is normal (self-signed certificate). Click "Advanced" -&gt; "Proceed Unsafe".*
- **Login:**
    
    
    - **User:** `root`
    - **Password:** (The one you set in Step 5)
    - **Realm:** Linux PAM standard authentication