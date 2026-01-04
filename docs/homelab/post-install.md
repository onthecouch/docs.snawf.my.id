### 1. Run the Script
This script automatically detects your Proxmox version and performs the necessary repository changes and updates.

 1. Open the Proxmox Shell (**Datacenter** > **[pve]** > **Shell**).
 2. Paste and run the following command:

```bash
bash -c "$(wget -qLO - https://github.com/community-scripts/ProxmoxVE/raw/main/misc/post-pve-install.sh)"
```

## 2. The Wizard Steps

The script will launch an interactive interface in the shell. It is generally recommended to answer Yes (y) to the prompts to get a standard "Home Use" configuration.

The script will perform the following actions:

- Correct Repositories: Disables the "Enterprise" (Paid) repository and enables the "No-Subscription" (Free) repository.
- Update System: Runs apt update and apt dist-upgrade to get the latest packages.
- Remove Nag: Disables the "No Valid Subscription" login popup.
- Kernel Clean: (Optional) Removes unused kernel versions to save space.
- Reboot: Reboots the server to apply kernel updates.

## 3. Verification

Once the server comes back online:

- Log in to the Web GUI.
- Navigate to [Your Node] > Updates > Repositories.
- Verify that the pve-enterprise repository is Disabled and pve-no-subscription is Enabled.