# OWASP ZAP + Tor: Anonymizing Security Scanning on macOS

This guide documents how to route all OWASP ZAP traffic through the Tor network on macOS, ensuring that web application security scans originate from a Tor exit node rather than your real IP address. The setup prevents target servers from identifying your actual location and helps bypass IP‑based blocking.

## How It Works

The browser or automated scanner sends traffic to ZAP’s local proxy (`127.0.0.1:8080`). ZAP is configured to forward every request through Tor’s SOCKS5 proxy (default `127.0.0.1:9050` for system Tor or `9150` for Tor Browser). The target server sees only the IP address of a Tor exit node.


## macOS Setup (Homebrew)

Install the required tools:

```bash
brew install tor
brew install proxychains-ng
brew install --cask zaproxy
```
## Start Tor as a background service:

```bash
brew services start tor
```
## After installing `proxychains-ng` and starting the Tor service, open OWASP ZAP and navigate to: Tools → Options → Network → Local Network
<img width="740" height="561" alt="36g353f" src="https://github.com/user-attachments/assets/c6fa79ba-b26d-4ad0-aaad-3124a75d362c" />

# Enable the following settings:
```Text
Enable SOCKS Proxy
Host: 127.0.0.1
Port: 9050
Version: SOCKS5
Also enable: (This is configured in the following menu:Proxy Properties)
Use SOCKS DNS
After applying the changes, restart ZAP
```
## Verifying the Connection
In the Quick Start tab, enter the following URL:
https://api.ipify.org
Send the request and review the Response tab.
If the configuration is working correctly, the returned IP address should match the Tor exit node rather than your local public IP address, indicating that ZAP traffic is being routed through the Tor network.

