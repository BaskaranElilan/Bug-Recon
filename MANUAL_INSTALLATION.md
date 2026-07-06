# Manual Tool Installation

This guide installs Bug Recon dependencies manually on Kali Linux or another Debian-based system. Run commands as root or with `sudo` where required.

## 1. System Packages

Install the base packages used by the installer and scan phases.

```bash
sudo apt update
sudo apt install -y \
  nmap curl wget git python3 python3-pip pipx golang-go build-essential \
  dnsutils whois jq unzip masscan nikto wafw00f whatweb dirb gobuster \
  chromium chromium-driver seclists
```

Package references:

- Nmap: https://nmap.org/
- Masscan: https://github.com/robertdavidgraham/masscan
- Nikto: https://github.com/sullo/nikto
- wafw00f: https://github.com/EnableSecurity/wafw00f
- WhatWeb: https://github.com/urbanadventurer/WhatWeb
- Gobuster: https://github.com/OJ/gobuster
- SecLists: https://github.com/danielmiessler/SecLists

## 2. Go Path

Make sure Go binaries are available in your shell.

```bash
mkdir -p "$HOME/go/bin"
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> ~/.bashrc
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
```

Check Go:

```bash
go version
```

If your distro Go package is too old, install Go manually from:

- Go downloads: https://go.dev/dl/

Example for amd64:

```bash
wget -O /tmp/go.tar.gz https://go.dev/dl/go1.22.3.linux-amd64.tar.gz
gzip -t /tmp/go.tar.gz
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf /tmp/go.tar.gz
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
go version
```

## 3. Go-Based Tools

```bash
go install github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
go install github.com/projectdiscovery/httpx/cmd/httpx@latest
go install github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
go install github.com/projectdiscovery/dnsx/cmd/dnsx@latest
go install github.com/projectdiscovery/katana/cmd/katana@latest
go install github.com/projectdiscovery/naabu/cmd/naabu@latest
go install github.com/ffuf/ffuf/v2@latest
go install github.com/lc/gau/v2/cmd/gau@latest
go install github.com/tomnomnom/assetfinder@latest
go install github.com/tomnomnom/waybackurls@latest
go install github.com/tomnomnom/anew@latest
go install github.com/tomnomnom/qsreplace@latest
go install github.com/projectdiscovery/notify/cmd/notify@latest
go install github.com/projectdiscovery/alterx/cmd/alterx@latest
go install github.com/projectdiscovery/tlsx/cmd/tlsx@latest
go install github.com/hakluke/hakrawler@latest
go install github.com/d3mondev/puredns/v2@latest
go install github.com/trufflesecurity/trufflehog/v3@latest
```

GitHub links:

- Subfinder: https://github.com/projectdiscovery/subfinder
- httpx: https://github.com/projectdiscovery/httpx
- Nuclei: https://github.com/projectdiscovery/nuclei
- dnsx: https://github.com/projectdiscovery/dnsx
- Katana: https://github.com/projectdiscovery/katana
- Naabu: https://github.com/projectdiscovery/naabu
- ffuf: https://github.com/ffuf/ffuf
- gau: https://github.com/lc/gau
- assetfinder: https://github.com/tomnomnom/assetfinder
- waybackurls: https://github.com/tomnomnom/waybackurls
- anew: https://github.com/tomnomnom/anew
- qsreplace: https://github.com/tomnomnom/qsreplace
- notify: https://github.com/projectdiscovery/notify
- alterx: https://github.com/projectdiscovery/alterx
- tlsx: https://github.com/projectdiscovery/tlsx
- hakrawler: https://github.com/hakluke/hakrawler
- puredns: https://github.com/d3mondev/puredns
- TruffleHog: https://github.com/trufflesecurity/trufflehog

Update Nuclei templates:

```bash
nuclei -update-templates
```

## 4. Python-Based Tools

Try normal pip first:

```bash
python3 -m pip install arjun dirsearch uro dnsrecon
```

On Kali/Debian, if system Python blocks global installs, use one of these options:

```bash
python3 -m pip install --break-system-packages arjun dirsearch uro dnsrecon
```

or:

```bash
pipx install arjun
pipx install dirsearch
pipx install uro
pipx install dnsrecon
pipx ensurepath
```

GitHub links:

- Arjun: https://github.com/s0md3v/Arjun
- dirsearch: https://github.com/maurosoria/dirsearch
- uro: https://github.com/s0md3v/uro
- dnsrecon: https://github.com/darkoperator/dnsrecon

## 5. Findomain

Bug Recon expects the `findomain` command.

GitHub:

- Findomain: https://github.com/Findomain/Findomain

Install from release zip:

```bash
wget -O /tmp/findomain.zip https://github.com/Findomain/Findomain/releases/latest/download/findomain-linux-i386.zip
unzip -o /tmp/findomain.zip -d /tmp/
sudo mv /tmp/findomain /usr/local/bin/findomain
sudo chmod +x /usr/local/bin/findomain
findomain --help
```

If the i386 build does not work on your system, download the correct Linux binary from the release page.

## 6. massdns

`massdns` is used by `puredns` for fast DNS bruteforce.

GitHub:

- massdns: https://github.com/blechschmidt/massdns

Install:

```bash
rm -rf /tmp/massdns
git clone https://github.com/blechschmidt/massdns.git /tmp/massdns
make -C /tmp/massdns
sudo cp /tmp/massdns/bin/massdns /usr/local/bin/massdns
sudo chmod +x /usr/local/bin/massdns
massdns --help
```

## 7. LinkFinder

GitHub:

- LinkFinder: https://github.com/GerbenJavado/LinkFinder

Install:

```bash
sudo git clone https://github.com/GerbenJavado/LinkFinder.git /opt/LinkFinder
python3 -m pip install -r /opt/LinkFinder/requirements.txt --break-system-packages
```

## 8. SecretFinder

GitHub:

- SecretFinder: https://github.com/m4ll0k/SecretFinder

Install:

```bash
sudo git clone https://github.com/m4ll0k/SecretFinder.git /opt/SecretFinder
python3 -m pip install -r /opt/SecretFinder/requirements.txt --break-system-packages
```

## 9. ParamSpider

GitHub:

- ParamSpider: https://github.com/devanshbatham/ParamSpider

Install:

```bash
sudo git clone https://github.com/devanshbatham/ParamSpider.git /opt/ParamSpider
python3 -m pip install -r /opt/ParamSpider/requirements.txt --break-system-packages
```

## 10. Feroxbuster

GitHub:

- Feroxbuster: https://github.com/epi052/feroxbuster

Install using the upstream installer:

```bash
curl -sL https://raw.githubusercontent.com/epi052/feroxbuster/main/install-nix.sh | sudo bash -s -- /usr/local/bin
feroxbuster --version
```

Alternative with Cargo:

```bash
cargo install feroxbuster
```

## 11. Verify Required Commands

After installation, verify the commands Bug Recon uses most often:

```bash
for tool in \
  subfinder httpx nuclei dnsx katana naabu ffuf gau assetfinder waybackurls \
  anew qsreplace notify alterx tlsx hakrawler puredns trufflehog arjun \
  dirsearch uro dnsrecon findomain massdns nmap masscan nikto wafw00f \
  whatweb dirb gobuster jq chromium feroxbuster; do
  if command -v "$tool" >/dev/null 2>&1; then
    echo "[OK] $tool"
  else
    echo "[MISSING] $tool"
  fi
done
```

## 12. Run Bug Recon

```bash
sudo bash bug_recon.sh -d target.com
```

Use only against targets where you have explicit authorization.