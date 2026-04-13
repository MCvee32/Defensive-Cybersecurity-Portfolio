# Part 2 - B20: Enhance the security of a GitHub project

### Project: github.com/amitt001/pygmy
An open-source , extensible and easy-to-use URL shortener.

### Security Issue
SSRF (Server-Side Request Forgery) risk  
Impact: internal network access, cloud metadata theft, open redirect abuse  
**Affected File:** utilities/urls.py  
**Vulnerable Code:** regex allows any IP incuding private ranges  
```
def validate_url(url): regex = re.compile( r'...' r'\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}|' # any IPv4 — no range check r'localhost|' # loopback allowed ... ) # No check whether the IP is private or reserved
```
**Attack:** shorten http://169.254.169.254/latest/meta-data/iam/security-credentials/  
- Pygmy redirects any visitor to the AWS metadata endpoint, leaking IAM credentials from the host EC2 instance.  

### Security Enhancement
Adds _is_ssrf_safe() helper and blocked network list into utilities/urls.py  
**Code:**  
```
import ipaddress
_BLOCKED_NETWORKS = [
ipaddress.ip_network('10.0.0.0/8'), # RFC 1918
ipaddress.ip_network('172.16.0.0/12'), # RFC 1918
ipaddress.ip_network('192.168.0.0/16'), # RFC 1918
ipaddress.ip_network('127.0.0.0/8'), # loopback
ipaddress.ip_network('169.254.0.0/16'), # link-local / AWS metadata ipaddress.ip_network('::1/128'), # IPv6 loopback
ipaddress.ip_network('fc00::/7'), # IPv6 unique-local
ipaddress.ip_network('fe80::/10'), # IPv6 link-local
]

_BLOCKED_HOSTNAMES = {'localhost', 'localhost.localdomain'}

def _is_ssrf_safe(hostname):
    """Return False if the hostname resolves to a private/reserved address.
 
    Blocks:
    - 'localhost' and common loopback aliases
    - Any bare IP literal that falls within a reserved network range
    Public domain names (e.g. 'example.com') pass through; DNS resolution is
    intentionally *not* performed here to avoid latency — that is a separate
    runtime concern.
    """
    if hostname.lower() in _BLOCKED_HOSTNAMES:
        return False
    # Strip IPv6 brackets: [::1] -> ::1
    bare = hostname.strip('[]')
    try:
        addr = ipaddress.ip_address(bare)
        for network in _BLOCKED_NETWORKS:
            if addr in network:
                return False
    except ValueError:
        # Not a bare IP literal — treat as a public hostname (safe to allow).
        pass
    return True
```
### Pull Request
<img width="628" height="811" alt="Screenshot 2026-04-13 at 4 13 14 pm" src="https://github.com/user-attachments/assets/3181397f-3707-4943-abcb-0f293535828b" />
