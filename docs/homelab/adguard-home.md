**Context:** Running behind a secondary router (Tianyi) with a locked ISP modem.


### 1. Network Topology & Strategy
Since the ISP router is locked, I will configure **AdGuard Home** to work strictly within the Local Network managed by the secondary router.

**The Flow:**
`ISP Router (Locked)` → `Secondary Router (10.10.10.1)` → `Proxmox` → `AdGuard LXC (10.10.10.5)`

* **How it works:** The secondary router's DHCP server will tell all my devices to use the AdGuard LXC as their DNS server. The ISP router is completely bypassed for DNS logic.

### 2. LXC Container Specification
Here is my LXC container optimized specs.

| Feature | Setting |
| :--- | :--- |
| **OS Template** | Debian 12 (Bookworm) |
| **Type** | Unprivileged Container |
| **CPU** | 1 Core |
| **RAM** | 512 MB (1GB if retaining long logs) |
| **Storage** | 8 GB (local-lvm) |
| **Network** | Static IP (See below) |

#### IP Configuration
Make sure this IP is **outside** my router's DHCP range to avoid conflicts.
* **IPv4/CIDR:** `10.10.10.5/24`
* **Gateway:** `10.10.10.1` (my tianyi router)


### 3. Installation Steps

#### A. Prepare the Container
Open the adguard LXC console and run:

```bash
apt update && apt upgrade -y
apt install curl sudo tar -y
```
#### B. Download & Install AdGuard Home

```bash
# Download the latest release
curl -s -S -L [https://static.adguard.com/adguardhome/release/AdGuardHome_linux_amd64.tar.gz](https://static.adguard.com/adguardhome/release/AdGuardHome_linux_amd64.tar.gz) | tar xz

# Move into directory
cd AdGuardHome

# Install the service
./AdGuardHome -s install
```
#### C. Verify Status
```bash
systemctl status AdGuardHome
```


### 4. Web UI Configuration
server up:
**`http://10.10.10.5:3000`**

**Initial Setup Wizard**

1. **Listening Interface:** Select All interfaces.
2. **Port Settings:**
   * **Admin Web Interface:** Port 3000 (Default)
   * **DNS Server:** Port 53 (Critical)
   * **User Setup:** Create your admin username and password.


### 5. Post-Install Configuration

#### **A. Router Configuration (Critical)**

Log into **Secondary Router (Tianyi)** admin panel.

1. Navigate to **DHCP Settings** (or LAN Settings).
2. **Primary DNS:** Set to 10.10.10.5 (AdGuard IP).
3. **Secondary DNS:** Leave EMPTY.
4. Save and **Reboot the Router** to force all devices to get the new settings.

#### **B. Upstream DNS (Inside AdGuard)**

Tell AdGuard where to send non-blocked traffic.

1. Go to **Settings > DNS Settings.**
2. **Upstream DNS Servers:**
   * 1.1.1.1 (Cloudflare)
   * 1.0.0.1
   * (Encrypted) https://dns.cloudflare.com/dns-query
3. **Bootstrap DNS:** 1.1.1.1
4. Click Test Upstreams and then Apply.


### 6. Verification

To ensure it is working, open a command prompt (CMD) on your laptop or PC:

```powershell
nslookup google.com
```
**Success Indicators:**
  * **Server:** Should show 10.10.10.5 (or the hostname of your AdGuard LXC).
  * **AdGuard Dashboard:** The "DNS Queries" counter should start increasing as you browse the web.