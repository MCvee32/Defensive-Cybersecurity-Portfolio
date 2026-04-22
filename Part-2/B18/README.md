# Part 2 - B18: Contribute to an open-source project related to cybersecurity.

### Project: CVE Binary Tool (cve-bin-tool)
**Repository:** https://github.com/ossf/cve-bin-tool  
**What it Does:** Scans binaries, SBOMs, and package lists for known CVEs using data from NVD, RedHat, OSV, GitLab Advisory Database, and Curl  

### Contribution: New Binary Checker - Traefik
Adds a new binary checker for Traefik, a widely open-source reverse proxy and load balancer. Traefik has no existing checker in the project, and has numerous recorded CVEs, meaning cve-bin-tool was blind to Traefik installations on scanned systems. Therefore, there is a genuine gap.  
**CVE Coverage:**  
- With this checker, cve-bin-tool can now detect:
  - CVE-2024-45410
  - CVE-2025-32431
  - CVE-2025-47952
  - CVE-2024-28869
  - CVE-2023-47633

### Files Added
**File Created:** cve_bin_tool/checkers/traefik.py  
- New checker class with version, filename, and contains patterns  
**Code:**  
```
"""
CVE checker for traefik

https://www.cvedetails.com/product/55360/Containous-Traefik.html
"""
from __future__ import annotations
from cve_bin_tool.checkers import Checker

class TraefikChecker(Checker):
    CONTAINS_PATTERNS: list[str] = [
        r"github\.com/traefik/traefik",
        r"traefik\.toml",
    ]
    FILENAME_PATTERNS = [r"traefik"]
    VERSION_PATTERNS = [
        r"traefik/v([0-9]+\.[0-9]+\.[0-9]+)",
        r"Version:\s*v?([0-9]+\.[0-9]+\.[0-9]+)",
    ]
    VENDOR_PRODUCT = [("traefik", "traefik"), ("containous", "traefik")]
```
**Pattern Rationale:**  
- CONTAINS: CONTAINS_PATTERNS:
Go binaries embed the full import path (github.com/traefik/traefik) and config file references (traefik.toml) as literal strings. Both are specific enough to avoid false positives.
- FILENAME: FILENAME_PATTERNS:
The binary is shipped as "traefik" across all platforms (Linux, Windows, macOS, Docker).
- VERSION: VERSION_PATTERNS:
Go binaries encode module version as "traefik/v2.10.4". The secondary pattern captures human-readable "Version: v3.0.0" strings found in help output embedded in the binary.
- VENDOR_PRODUCT: VENDOR_PRODUCT:
Both "traefik" (current vendor) and "containous" (original company, legacy CVEs) are included to ensure full CVE coverage across all recorded vulnerabilities.

**File Created:** test/test_data/traefik.py 
- Mapping tests and package test URLs
**Code:**  
```
mapping_test_data = [
    { "product": "traefik", "version": "2.10.4",
      "version_strings": ["traefik/v2.10.4"] },
    { "product": "traefik", "version": "3.0.0",
      "version_strings": ["Version: v3.0.0"] },
    { "product": "traefik", "version": "2.11.9",
      "version_strings": ["traefik/v2.11.9"] },
]
package_test_data = [
    { "url": "https://github.com/traefik/traefik/releases/download/v2.10.4/",
      "package_name": "traefik_v2.10.4_linux_amd64.tar.gz",
      "product": "traefik", "version": "2.10.4" },
    { "url": "https://github.com/traefik/traefik/releases/download/v3.0.0/",
      "package_name": "traefik_v3.0.0_linux_amd64.tar.gz",
      "product": "traefik", "version": "3.0.0" },
]
```
### Pull Request
<img width="634" height="814" alt="Screenshot 2026-04-13 at 3 11 37 pm" src="https://github.com/user-attachments/assets/08104d16-a1a3-4c30-84d5-5099f0688f0f" />
<img width="617" height="818" alt="Screenshot 2026-04-13 at 3 11 51 pm" src="https://github.com/user-attachments/assets/971e03da-4a7b-4499-9aac-8c6885c0ba5e" />



