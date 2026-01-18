### 1. Prerequisites
**PowerShell 7 (Required)**
Oh My Posh is designed for PowerShell 7+ (`pwsh`), not the legacy Windows PowerShell 5.1 (`powershell`).

* **Check your version:**
```powershell
$PSVersionTable.PSVersion
```

* **Install PowerShell 7 (if needed):**
```powershell
winget install Microsoft.PowerShell
```

**Nerd Font (Required for Icons)**
Without a "Nerd Font," icons and separators will look like broken squares.

* **Install the Font:**
```powershell
winget install NerdFonts.CascadiaCode
```
* **Configure Terminal:**
    * Open Windows Terminal Settings (`Ctrl+,`).
    * Go to **Profiles** > **Defaults** > **Appearance**.
    * Set **Font face** to `CaskaydiaCove Nerd Font`.

---

### 2. Install Oh My Posh
We use `winget` for the easiest installation and updates.
```powershell
winget install JanDeDobbeleer.OhMyPosh
```
**Verify installation:**
```powershell
oh-my-posh --version
```
---
### 3. The PowerShell Profile (Critical Step)

*This is the most common point of failure. PowerShell 7 uses a different profile path than legacy PowerShell, and the folder often does not exist by default.*

* Check where PowerShell looks for the profile:
```powershell
$PROFILE # Expected: C:\Users\<user>\Documents\PowerShell\Microsoft.PowerShell_profile.ps1
```

* Create the profile file: Run this command to force-create the folder and file:
```powershell
New-Item -ItemType File -Path $PROFILE -Force
```

**Why was it missing?**

* `Documents\WindowsPowerShell` = Legacy PowerShell 5.1
* `Documents\PowerShell` = Modern PowerShell 7
* PowerShell 7 does not auto-create this folder until told to.

---
### 4. Downloading Themes
> Note: New versions of Oh My Posh do not install themes to a local folder by default. You must download the config file you want.

* **Create a config directory:**
```powershell
mkdir $HOME\.config\oh-my-posh -Force
```

* **Download emodipt-extend:**
```powershell
Invoke-WebRequest `"[https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/main/themes/emodipt-extend.omp.json](https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/main/themes/emodipt-extend.omp.json)" `-OutFile "$HOME\.config\oh-my-posh\emodipt-extend.omp.json"
```

---
### 5. Activate the Prompt
Now we tell PowerShell to load Oh My Posh on startup.
* **Open your profile:**
```powershell
notepad $PROFILE
```
* **add the url:**
```powershell
oh-my-posh init pwsh --config "$HOME\.config\oh-my-posh\emodipt-extend.omp.json" | Invoke-Expression
```
* **Reload your profile:**
```powershell
. $PROFILE
```