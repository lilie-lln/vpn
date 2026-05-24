# ❇️ Dockerize xray proxy 

A containerized deployment solution for setting up an Xray Proxy (VLESS protocol) using Docker and Docker Compose. 
## 🔫 Architecture Overview

```text
Browser / curl 
        ↓
SOCKS5 Proxy (127.0.0.1:1080)
        ↓
Xray Client (Docker Container)
        ↓
VLESS Tunnel (Port 443 / TCP)
        ↓
Xray Server (Docker Container on VPS)
        ↓
Internet (Target Website sees VPS IP)
```
## 🔫 File structure

```text
xray-docker-proxy/
├── .gitignore
├── .env
├── docker-compose.yml
├── server/
│   ├── Dockerfile
│   └── config.template.json
└── client/
    ├── Dockerfile
    └── config.template.json
```

## 🔫 Quick Start

### Prerequisites & VPS Provisioning
To deploy the server instance, we will need a Virtual Private Server (VPS). 
- Spin up an instance (e.g., Ubuntu LTS) using a cloud provider, we use **Vultr** here.
- Ensure Docker and Docker Compose are installed on VPS.
- Open port 443 on firewall
  
```bash
run `ufw allow 443/tcp
```

### Configure Environment Variables (Local)
.env

```ini
XRAY_UUID=
VPS_IP=
```

### Server Deployment

Transfer the project files to VPS, navigate to the root directory, and spin up the server container:

```bash
docker compose up -d xray-server
```

### Client Deployment 

```bash
docker compose up -d xray-client
```

*Note: The client container is configured to listen on `0.0.0.0:1080` internally so that host machine can access the SOCKS5 proxy service via `127.0.0.1:1080`.*

## 🔫 Connection Testing

Verify that traffic is successfully routing through the proxy tunnel using `curl`:

### Basic test
```bash
curl --proxy socks5://127.0.0.1:1080 [https://ipinfo.io](https://ipinfo.io)
```
### Routing DNS resolution through proxy
```bash
curl --proxy socks5h://127.0.0.1:1080 [https://ipinfo.io](https://ipinfo.io)

```
