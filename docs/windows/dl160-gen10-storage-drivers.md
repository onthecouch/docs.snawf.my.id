## Backstory
We are rolling out the new Finance System next quarter, and the vendor explicitly lists Windows Server 2025 as a requirement for the database host. That meant I had to wipe our existing DL160 Gen10 (which was running 2022) and do a fresh install.

I thought this would be a quick 30-minute re-image. I grabbed the ISO, booted up, and hit the classic HP wall: **"Where do you want to install Windows?"**

The drive list was empty. It turns out Server 2025 doesn't ship with the native drivers for the embedded HP storage controller yet. To make it more annoying, Intelligent Provisioning hung on the "Network Detection" screen, so I had to bypass the fancy tools and force the drivers in manually.

## Problem: Missing Storage Drivers
The DL160 Gen10 uses the embedded **HPE Smart Array S100i SR Gen10** controller (Software RAID). The generic Windows installer has no idea what this hardware is, so it sees zero disks.

## Fix: Manually Injecting the 2022 Driver
Since Server 2025 is brand new, HP hasn't listed specific drivers for it in their support portal. The workaround is using the **Windows Server 2022** drivers, which work perfectly.

### Requirements
* A spare USB stick (or just use the installer stick if you have space).
* A laptop to prep the files.

### Step 1: Download the Correct Driver
!!! note "Note:"
    Don't make the mistake I did and download the "P-series" or "E-series" drivers. Those are for the physical RAID cards.

For the onboard RAID, we need the **S100i** driver:

1. **Download Link:** [HPE Smart Array S100i SW RAID Driver (Server 2022)](https://support.hpe.com/connect/s/softwaredetails?language=en_US&collectionId=MTX-09fdb681b4924b89&tab=releaseNotes)
2. **File Name:** `cp052316.exe` (1.6 MB)

### Step 2: How - to inject
!!! danger "Crucial:"
    You cannot just dump the `.exe` on the USB. The Windows installer ignores executables.

1.  Run the `cp052316.exe(1.6 MB)` on your laptop.
2.  **Do not click Install.** Click the **Extract** button.
3.  Dump the files into a folder named something obvious like `S100i_Drivers`.
4.  Copy that folder to your USB stick.

### Step 3: Loading the Driver During Setup
1.  Boot the server into the Windows Server 2025 installer.
2.  At the empty drive selection screen, click **Load Driver**.
3.  Browse to the folder on the USB.
4.  Select the **HPE Smart Array S100i SR Gen10 SW RAID** driver.
5.  Click **Next**.

The list refreshed immediately after this, showing the RAID array. Proceed the installation as usual.

## Troubleshooting / Sanity Checks
* **BIOS Mode:** If the driver loads but the list is still empty, check the BIOS (F9). Go to **System Options > Storage** and ensure the controller is set to **"Smart Array SW RAID Support"**. If it was somehow set to "AHCI," the driver won't work.
* **Intelligent Provisioning:** If you try to use F10 to automate this and it hangs, don't wait for it. Just hard reboot and use the manual method above. It's faster than troubleshooting the IP network stack.
* **Never use old server in your company.**