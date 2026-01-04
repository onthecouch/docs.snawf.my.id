**Public URL:** `https://kb.snawf.my.id`

**Architecture:** "Single Tunnel" Gateway. The tunnel agent runs inside the **Nginx Proxy Manager** container. It grabs traffic from Cloudflare and hands it directly to Nginx, which then routes it to the correct internal IP.

---

### 1. Prerequisites
* A domain name added to Cloudflare (e.g., `snawf.my.id`).
* **Nginx Proxy Manager (NPM)** installed and running.
* **BookStack** installed and running.


---

### 2. Install Cloudflared Agent
We install the tunnel software directly inside the Nginx Proxy Manager container.

* Open the **Proxmox Shell** (Datacenter > Node > Shell).
* Enter container:

```bash
pct enter 123
```
* Download and install the agent:

```bash
curl -L --output cloudflared.deb [https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb](https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb)
```

```bash
dpkg -i cloudflared.deb
```

### 3. Authenticate & Login
Link this container to your Cloudflare account.

* Run the login command:

```bash
cloudflared tunnel login
```

* **Authorize:** It will print a URL starting with `https://dash.cloudflare.com/...`. Copy this URL into a browser on your PC, log in, and select your domain (`snawf.my.id`).
* **Success:** The terminal will confirm it downloaded the certificate file (`cert.pem`).

---

### 4. Create and Configure the Tunnel
We will create a "Catch-All" tunnel that sends *everything* to Nginx. This means you rarely have to touch this config file again.

* **Create the Tunnel:**

```bash
cloudflared tunnel create kb-home
```
*Copy the Tunnel ID (UUID) it displays. You will need it below.*

* **Route the DNS:**
Create a wildcard CNAME record so `*kb.snawf.my.id` points to this tunnel.

```bash
cloudflared tunnel route dns kb-home *kb.snawf.my.id
```

* **Create Configuration File:**

```bash
mkdir -p /etc/cloudflared
nano /etc/cloudflared/config.yml
```

* **Paste the Config:**
Replace `<UUID>` with the ID before

```yaml
tunnel: <UUID>
 credentials-file: /root/.cloudflared/<UUID>.json

  ingress:
    # Rule 1: Bookstack
    - hostname: "*kb.snawf.my.id"
      service: http://10.10.10.12:80
      
    # Rule 2: Fallback (Required)
    - service: http_status:404
```

---

### 5. Start the Tunnel Service
Enable the tunnel to start automatically on boot.

```bash
cloudflared service install
systemctl start cloudflared
systemctl enable cloudflared
systemctl status cloudflared
```

---

### 6. Nginx Proxy Manager Setup

Now that traffic reaches Nginx, tell Nginx where to send it.
  1. Log into your NPM Web UI (`http://10.10.10.6:81`).

  2. Click **Hosts** > **Proxy Hosts** > **Add Proxy Host**.

      * **Domain Names:** `kb.snawf.my.id`
      * **Scheme:** http
      * **Forward Hostname/IP:** 10.10.10.12 (BookStack Container IP).
      * **Forward Port:** 80.
      * **Block Common Exploits:** Enabled.

  3. **SSL Tab:**
      * SSL Certificate: None (Cloudflare handles the SSL encryption at the edge, so "None" or a self-signed cert here is fine for the internal leg).

  4. Click **Save**.

---

### 7. Critical: Update BookStack Config

If you skip this, images will break and links will redirect to the local IP.
* Enter your BookStack container console.
* Edit the environment configuration:

```
nano /var/www/bookstack/.env
```
* Find APP_URL and update it to the public address:

```TOML
APP_URL=https://kb.snawf.my.id
```
* Save and exit.
* Restart BookStack (or the container) to apply changes.

---

### 8. Verification
  1. Disconnect phone from WiFi (use mobile data).
  2. Open a browser and visit https://kb.snawf.my.id.
  3. You should see your BookStack login page with a secure lock icon.