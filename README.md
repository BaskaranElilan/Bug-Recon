# Bug Recon

Bug Recon is an automated bug bounty reconnaissance script for Kali Linux and other Debian-based systems. One command maps a target's attack surface across 13 sequential phases: subdomain enumeration, DNS analysis, port scanning, technology fingerprinting, WAF checks, endpoint discovery, JavaScript analysis, cloud asset discovery, vulnerability scanning, secrets hunting, screenshots, reporting, and directory fuzzing.

Maintained by Elilan Baskaran.


## What It Does

The script runs 13 phases against a target domain in sequence.

Phase 1 - Subdomain Enumeration
Pulls from passive sources including Subfinder, Assetfinder, Findomain, crt.sh, AlienVault, Wayback Machine, URLScan, RapidDNS, HackerTarget, BufferOver, CommonCrawl, and OTX. It merges and deduplicates results, runs AlterX permutations, performs active DNS bruteforce with puredns or dnsx, resolves DNS, and probes HTTP services.

Phase 2 - DNS Deep Recon
Attempts zone transfers, enumerates DNS records, checks SPF/DMARC/DKIM records, gathers ASN/IP range context, and performs reverse DNS lookup.

Phase 3 - Port Scanning
Uses Naabu for fast port discovery, Nmap for service/version detection, common interesting port checks, and Shodan InternetDB lookup.

Phase 4 - Technology Fingerprinting
Runs bulk httpx technology detection, detailed WhatWeb fingerprinting, response header collection, and cookie flag analysis.

Phase 5 - WAF Detection
Runs wafw00f, checks response headers for WAF/CDN fingerprints, and attempts origin-IP discovery.

Phase 6 - Endpoint Discovery
Harvests URLs with GAU, Waybackurls, Katana, and Hakrawler, then categorizes API endpoints, admin panels, sensitive files, GraphQL endpoints, and parameterized URLs.

Phase 7 - JavaScript Analysis
Extracts JavaScript URLs, downloads files, runs LinkFinder and SecretFinder, and applies regex-based secret discovery patterns.

Phase 8 - Parameter Discovery
Extracts parameters from URLs, runs ParamSpider and Arjun, then categorizes SSRF/open redirect, IDOR, XSS, and SQLi-related parameters.

Phase 9 - Cloud Asset Recon
Checks S3, GCS, Azure Blob, and Firebase naming patterns derived from the target company name, and writes SSRF metadata endpoints for manual testing.

Phase 10 - Vulnerability Scanning
Runs Nuclei for critical/high/medium findings, checks exposed panels, misconfigurations, CVEs, sensitive paths, CORS issues, and missing security headers.

Phase 11 - Secrets Hunting
Runs TruffleHog against the target organization on GitHub when available, checks for exposed `.git` directories, and generates Google dorks for manual review.

Phase 12 - Screenshots
Captures headless Chromium screenshots for live hosts.

Phase 13 - Feroxbuster Directory Fuzzing
Runs last because it is time-intensive. Feroxbuster scans live subdomains with `dirb/common.txt`, and results are merged into `all_endpoints_master.txt`.

## Requirements

- Kali Linux or another Debian-based distribution
- Go 1.22 or higher
- Python 3
- Root or sudo access
- Active internet connection

The `--install` mode attempts to install required tools automatically.

## Installation

```bash
git clone https://github.com/BaskaranElilan/Bug-Recon.git
cd Bug-Recon
chmod +x bug_recon.sh
sudo bash bug_recon.sh --install
```


## Install Notes

Run installation with root privileges and an active internet connection:

```bash
sudo bash bug_recon.sh --install
```

Bug Recon uses `apt`, `go install`, `pip3`, and a few GitHub release downloads. If a tool still fails to install, rerun the command after checking connectivity, package-manager locks, and Go/Python paths.
## Usage

```bash
sudo bash bug_recon.sh -d target.com
```

Available options:

```text
-d  <domain>     Target domain - required
-o  <dir>        Output directory - default: ./recon_<domain>
-t  <threads>    Thread count - default: 50
-w  <wordlist>   Custom wordlist path
--deep           Enable deep scan, adds Nikto and extended checks
--passive        Passive recon only, no active scanning
--install        Install all tools and exit
--fresh          Ignore checkpoint, restart from scratch
-h               Show help
```

## Examples

```bash
sudo bash bug_recon.sh -d example.com
sudo bash bug_recon.sh -d example.com --deep -t 100
sudo bash bug_recon.sh -d example.com --passive
sudo bash bug_recon.sh -d example.com -o /root/results -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words.txt
sudo bash bug_recon.sh -d example.com --fresh
```

## Resume and Checkpoints

The script saves progress after each completed phase to `./recon_<domain>/.checkpoint`.

If a scan is interrupted, rerun the same command. Bug Recon detects the previous run and resumes from the last completed phase. Use `--fresh` to force a full restart.

## Output Structure

```text
recon_target.com/
|-- subdomains/
|-- dns/
|-- ports/
|-- tech/
|-- waf/
|-- endpoints/
|   |-- ferox_all/
|   `-- all_endpoints_master.txt
|-- js_analysis/
|-- params/
|-- cloud/
|-- vulnerabilities/
|-- secrets/
|-- screenshots/
|-- reports/
|   `-- RECON_REPORT_<domain>.md
`-- recon.log
```

## Key Files After a Scan

```text
vulnerabilities/nuclei_findings.txt       Automated vulnerability hits
vulnerabilities/sensitive_exposed.txt     Exposed .env, .git, config files
vulnerabilities/cors_issues.txt           CORS misconfigurations
js_analysis/secrets_found.txt             API keys and tokens from JS files
js_analysis/manual_secrets.txt            Regex-based secret matches
endpoints/all_endpoints_master.txt        Full endpoint list for Burp/manual review
params/ssrf_redirect_params.txt           Pre-filtered SSRF and open redirect params
params/idor_params.txt                    Pre-filtered IDOR params
params/xss_sqli_params.txt                Pre-filtered injection params
cloud/s3_buckets.txt                      Public cloud bucket findings
secrets/exposed_git.txt                   Exposed .git repositories
subdomains/live_403_forbidden.txt         403 bypass candidates
subdomains/live_401_auth_required.txt     Auth bypass candidates
```

## Tools Used

Go-based: subfinder, httpx, nuclei, dnsx, naabu, katana, gau, assetfinder, waybackurls, anew, uro, qsreplace, alterx, tlsx, hakrawler, puredns, ffuf, notify.

System packages: nmap, masscan, nikto, wafw00f, whatweb, dirb, gobuster, dnsutils, whois, curl, jq, seclists, chromium.

Python-based: arjun, dirsearch, trufflehog, dnsrecon.

Compiled or git-cloned: LinkFinder, SecretFinder, ParamSpider, massdns, findomain, feroxbuster.

## Legal

This tool is for authorized security testing only. Use it only against targets where you have explicit written permission, or targets explicitly listed in a bug bounty program scope.

Unauthorized scanning is illegal. The author is not responsible for misuse.

## Author

Elilan Baskaran
GitHub: https://github.com/BaskaranElilan
Repository: https://github.com/BaskaranElilan/Bug-Recon

## License

MIT License. See [LICENSE](LICENSE) for details.