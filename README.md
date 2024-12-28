# Pi-hole Setup and Configuration

## Overview

This repository provides a detailed guide for setting up, configuring, and maintaining a Pi-hole server. Pi-hole is a network-level ad blocker that protects your entire home network by acting as a DNS sinkhole, blocking unwanted ads and trackers.

## Features

- **Network-wide ad blocking**
- **Improved browsing speed**
- **Privacy protection by blocking trackers**
- **Customizable blocklists**
- **Web interface for monitoring and management**

## Why Pi-hole?

Pi-hole is a lightweight solution to improve your browsing experience and enhance your network's privacy. By blocking unwanted ads and trackers at the DNS level, it eliminates distractions and speeds up web browsing without the need for browser extensions.

---

## Requirements

### Hardware
- Raspberry Pi, Linux server, or Docker-compatible system
- Minimum 512MB RAM
- Ethernet connection for optimal performance

### Software
- Linux-based operating system (e.g., Raspbian, Ubuntu, Debian)
- Docker (optional, for containerized installation)

---

## Installation

### 1. Update System

```bash
sudo apt update && sudo apt upgrade -y
```

### 2. Install Pi-hole

#### Option 1: Standard Installation
Run the official installation script:
```bash
curl -sSL https://install.pi-hole.net | bash
```

#### Option 2: Docker Installation
Pull the official Pi-hole Docker image:
```bash
docker pull pihole/pihole
```
Run the container:
```bash
docker run -d \
  --name pihole \
  -e TZ="America/New_York" \
  -v "/etc/pihole/:/etc/pihole/" \
  -v "/etc/dnsmasq.d/:/etc/dnsmasq.d/" \
  --net=host \
  pihole/pihole
```

### 3. Configure Network Settings

Set Pi-hole as your DNS server:
1. Access your router's configuration page.
2. Locate the DNS settings.
3. Enter your Pi-hole server's IP address as the primary DNS.

---

## Configuration

### Access Pi-hole Dashboard
1. Open a browser and navigate to `http://<pi-hole-ip-address>/admin`.
2. Log in using the password displayed during installation (or set a new password using `pihole -a -p`).

### Add Custom Blocklists
1. Go to **Group Management > Adlists**.
2. Add new blocklists by entering URLs and clicking **Add**.
3. Update the gravity database:
   ```bash
   pihole -g
   ```

### Whitelisting and Blacklisting
- To whitelist domains: Use the web interface under **Whitelist** or run:
  ```bash
  pihole -w example.com
  ```
- To blacklist domains: Use the web interface under **Blacklist** or run:
  ```bash
  pihole -b example.com
  ```

---

## Maintenance

### Update Pi-hole
Update Pi-hole and its dependencies with:
```bash
pihole -up
```

### Backup and Restore
- Backup Pi-hole configuration:
  ```bash
  sudo cp -r /etc/pihole/ ~/pihole-backup/
  sudo cp -r /etc/dnsmasq.d/ ~/pihole-backup/
  ```
- Restore configuration:
  ```bash
  sudo cp -r ~/pihole-backup/pihole/ /etc/pihole/
  sudo cp -r ~/pihole-backup/dnsmasq.d/ /etc/dnsmasq.d/
  ```

---

## Troubleshooting

### Common Issues

- **DNS Resolution Failure**: Ensure the Pi-hole server is reachable and DNS settings are correct.
- **Admin Interface Not Loading**: Check that the web server is running:
  ```bash
  sudo systemctl restart lighttpd
  ```

### Logs
- Query logs: `/var/log/pihole.log`
- Web interface logs: `/var/log/lighttpd/error.log`

---

## Contributing

Contributions are welcome! Feel free to submit issues or pull requests to enhance this guide.

