##Laptop Web Server: Docker & Zero Trust
Overview
This project repurposes an old ASUS F550C laptop (i3, 4GB RAM, SSD) into a production-ready web server.

The goal was to host a publicly accessible website without exposing the home network or opening router ports.

#Tech Stack
OS: Ubuntu Server 24.04 LTS (Headless).

Containerization: Docker & Docker Compose.

Networking: Cloudflare Tunnel (Zero Trust).

#Architecture
The setup uses a single docker-compose.yml to run two services:

web: The NGINX container serving the site.

tunnel: The Cloudflared connector.

#Networking Logic:

Public Traffic -> Cloudflare Edge -> Secure Tunnel -> NGINX.

Important: The tunnel allows ingress to http://web:80 (using Docker's internal DNS), not localhost.

#Security Features
Zero Inbound Ports: No Port Forwarding required on the router.

Isolation: The host OS is not exposed to the public web.

Remote Management: SSH access is restricted via Cloudflare Access Policies (requires team email authentication).

#Setup Log
OS Installation: Clean install of Ubuntu Server 24.04 on SSD. Enabled OpenSSH.

Network: Manually configured Wi-Fi using Netplan (/etc/netplan/01-wifi.yaml).

Docker: Installed Docker Engine and added user to the docker group (usermod -aG docker).

Deployment: Retrieved Token from Cloudflare Dashboard and ran docker compose up -d.

Storage Fix: Ran lvextend and resize2fs to force Ubuntu to use the entire SSD capacity.

#Command Cheat Sheet
SSH (Local): ssh user@192.168.x.x

Update: sudo apt update && sudo apt upgrade -y

Monitor: htop

Logs: docker compose logs -f

Shutdown: sudo shutdown now
