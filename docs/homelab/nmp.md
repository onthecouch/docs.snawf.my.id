Initial Setup (with docker)

### 1. Container Specs
This container is optimized for low-resource usage while running the Docker daemon.

| Feature | Setting |
| :--- | :--- |
| **OS** | Debian 12 (Bookworm) |
| **Type** | Unprivileged LXC |
| **CPU** | 1 Core |
| **RAM** | 512 MB |
| **Swap** | 512 MB |
| **Disk** | 10 GB |
| **IP Address** | Static (e.g., `10.10.10.6`) |
| **Critical Features** | **Nesting=1**, **Keyctl=1** |

> **Why Nesting?** You *must* enable the "Nesting" feature in the Proxmox LXC Options tab. Without this, Docker will fail to start inside the container.


### 2. Preparation: Install Docker
Since this is a fresh LXC, we need to install the Docker engine first.

* **Update & Install Dependencies:**
```bash
apt update && apt upgrade -y
apt install curl sudo -y
```
* **Install Docker (Official Script):**
    This is the fastest way to get Docker on Debian/Ubuntu.
```bash
curl -fsSL [https://get.docker.com](https://get.docker.com) -o get-docker.sh
sh get-docker.sh
```
* **Verify Docker is Running:**
```bash
systemctl status docker
```
*If it fails, double-check that "Nesting" is enabled in Proxmox options and reboot the LXC.*

---

### 3. Deploy Nginx Proxy Manager
We will use `docker-compose` to manage the service.

* **Create a Directory:**
```bash
mkdir -p /opt/npm
cd /opt/npm
```
* **Create the Compose File:**
```bash
nano docker-compose.yml
```
* **Paste the Configuration:**
```yaml
services:
 app:
  image: 'jc21/nginx-proxy-manager:latest'
  restart: unless-stopped
  ports:
    - "127.0.0.1:80:80"
    - "127.0.0.1:443:443"
    - "127.0.0.1:81:81"
  volumes:
    - ./data:/data
    - ./letsencrypt:/etc/letsencrypt
```
* **Start the Container:**
```bash
docker compose up -d
```

---

### 4. Initial Configuration
Once Docker finishes pulling the images (this might take a minute on 512MB RAM):

* **URL:** `http://10.10.10.6:81`
* **Default Email:** `admin@example.com`
* **Default Password:** `changeme`

---

### 5. The "Internal" Strategy (AdGuard + NPM)

Since my ISP router is locked, I use this exclusively for **Internal** traffic (cleaning up my local network).

**The Workflow:**

1. **AdGuard Home** acts as the "Phonebook". It tells my devices that `jellyfin.home` points to this Docker container (`10.10.10.6`).

2. **Nginx** acts as the "Traffic Cop". It listens on `10.10.10.6`, sees the request for `jellyfin.home`, and forwards it to the actual Jellyfin IP (`10.10.10.20:8096`).

#### Example Setup:

1. **AdGuard:** Add DNS Rewrite `jellyfin.home` -> `10.10.10.6`

2. **Nginx Proxy Manager:** Add Proxy Host `jellyfin.home` -> Forward to `10.10.10.118` Port `8096`.

Now I can access my media server via `https://jellyfin.home` instead of remembering IPs.