Transform AdGuard Home from a simple ad-blocker into a network security appliance by selecting targeted blocklists.



### 1. The Strategy: Layered Defense
A common mistake is just enabling one massive list and hoping for the best. My configuration uses a **layered approach**. Instead of relying on one source, I use specialized lists that target specific types of threats.

The lists selected in this configuration fall into four categories:

1.  **The Baseline:** General ads and trackers.

2.  **The Heavy Lifter:** A curated, high-quality "pro" list that does most of the work.

3.  **Security & Malware:** Targeted lists specifically for known malicious infrastructure.

4.  **Privacy & Targeted:** Specialized lists for telemetry, link shorteners, and regional content.



### 2. Breakdown of Enabled Lists
Below is the explanation of every list currently enabled in the setup and why it was chosen.

#### A. The General Foundation
* **AdGuard DNS filter**
    * *Role:* The Baseline.
    * *Why:* This is AdGuard's official, well-maintained list. It catches the majority of common web advertisements and basic trackers. It is safe and rarely breaks legitimate websites.

#### B. The "Heavy Lifter" (Crucial)
* **HaGeZi's Pro Blocklist**
    * *Role:* The Aggressive Generalist.
    * *Why:* This is arguably the most important list in the setup. HaGeZi is a highly respected maintainer who curates multiple sources into one "Pro" list. It is aggressive against ads, trackers, and bad actors but is carefully managed to minimize false positives (breaking real sites). **If I could only choose one list, it would be this one.**

#### C. Security Hardening (Malware & Phishing)
These lists are what turn AdGuard into a security tool. They don't care about banner ads; they care about stopping devices from connecting to criminal servers.

* **The Big List of Hacked Malware Web Sites**
    * *Why:* Blocks legitimate websites that have been compromised by hackers and are currently serving malware.
* **uBlock filters – Badware risks**
    * *Why:* Targets sites involved in deceptive practices, scareware ("Your computer is infected!"), and undesirable software distribution.
* **Malicious URL Blocklist (URLHaus)**
    * *Why:* A vital source of intel on currently active malware distribution sites. If a device on the network tries to download a virus, this list should stop the connection before it starts.

#### D. Privacy & Targeted Tools
* **HaGeZi's URL Shortener Blocklist**
    * *Why:* Attackers often hide malicious links behind URL shorteners (like bit.ly clones) to trick users into clicking. This blocks known shady shortener services.
* **HaGeZi's Windows/Office Telemetry**
    * *Why:* Privacy. Windows and Office constantly send usage data "home" to Microsoft. This list blocks those specific tracking domains without breaking Windows Updates.
* **IDN: ABPindo**
    * *Why:* Regional specificity. Global lists often miss ads and scams targeted at specific countries. This list targets content relevant to Indonesia.



### 3. How This Hardens the Home Network
By moving beyond basic ad-blocking, this configuration provides significant security benefits for every device connected to the LAN (phones, PCs, smart TVs, IoT devices, etc.).

**1. Preventing "Drive-By" Infections**
Malware is often delivered through malicious ads ("malvertising"). By blocking the ad domains at the DNS level, the malicious code never even gets a chance to load in the browser.

**2. Stopping Command & Control (C2) Communication**
If a device *does* get infected (e.g., a smart bulb with weak security), the malware usually tries to "phone home" to a C2 server to receive instructions. The security blocklists (like URLHaus) are designed to blackhole these C2 domains, effectively neutralizing the malware's ability to operate.

**3. Reducing Attack Surface**
Every connection a computer makes to a third-party tracker or telemetry server is a potential pathway for data leakage or attack. By blocking thousands of these unnecessary connections daily, the overall "noise" on the network decreases, and privacy increases.

**4. Protecting "Dumb" Devices**
I cannot install antivirus on my Smart TV or IoT devices. DNS filtering is the *only* way to protect these devices from communicating with malicious actors.



### 4. Maintenance
* **Updates:** AdGuard Home automatically checks for updates to these lists based on the schedule defined in settings (usually every 12-24 hours).
* **False Positives:** If a legitimate website is blocked, I must check the AdGuard "Query Log" to see which domain was blocked and by which list, then add it to the "Custom filtering rules" (Allowlist).