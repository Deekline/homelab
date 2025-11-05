# Homelab Infrastructure

Self-hosted services and applications for personal use, built with Docker and modern DevOps practices.

## Architecture Overview

| Network Server | Main Server       | NAS              | External DNS     |
| -------------- | ----------------- | ---------------- | ---------------- |
| **(24/7 Ops)** | **(Development)** | **(TrueNAS CE)** | **(Cloudflare)** |
| OPNsense       | Traefik           | Samba            | DNS Records      |
| DNS Server     | Authentik         | ZFS              | SSL Certs        |
| Monitoring     | Portainer         |                  | API Access       |
| Core Services  | Media Stack       |                  |                  |
| Zenarmor       | Other             |                  |                  |

## Service Stack

### Core Infrastructure
| Service | Purpose | Status | Network |
|---------|---------|---------|---------|
| **Traefik** | Reverse proxy & SSL termination | ✅ Active | Main Server |
| **CrowdSec/ Traefik Bouncer** | IPS & IDS | ✅ Active | Main Server |
| **Authentik** | Identity provider & SSO | ✅ Active | Main Server |
| **Portainer** | Docker management interface | ✅ Active | Main Server |

### Monitoring & Management
| Service | Purpose | Status | Network |
|---------|---------|---------|---------|
| **Uptime Kuma** | Service monitoring | ✅ Active | Network Server |
| **Homepage** | Service dashboard | ✅ Active | Network Server |
| **Prometheus** | Metrics collection | ✅ Active | Main Server |
| **Telegraf** | Metrics collection | ✅ Active | Main Server |
| **Influx DB** | Metrics storing | ✅ Active | Main Server |
| **Grafana K6** | Metrics collection | ✅ Active | Main Server |
| **Grafana** | Metrics visualization | ✅ Active | Main Server |
| **Graylog** | Logs collection | 🔄 Planned | Main Server |


### Media & Content
| Service | Purpose | Status | Network |
|---------|---------|---------|---------|
| **RSS Reader** | News aggregation | ✅ Active | Main Server |
| **Linkwarden** | Links aggregation | ✅ Active | Main Server |
| **Plex/Jellyfin** | Media streaming | ✅ Active | Main Server |
| **Sonarr/Radarr** | Media management | ✅ Active | Main Server |
| **qBittorrent** | Torrent client | ✅ Active | Main Server |
| **Gluetun** | VPN container for torrenting | 🔄 Planned | Main Server |

### Productivity & Storage
| Service | Purpose | Status | Network |
|---------|---------|---------|---------|
| **Nextcloud** | File storage & collaboration | 🔄 Planned | Main Server |
| **Immich** | Photo management | 🔄 Planned | Main Server |
| **TrueNAS Scale** | Storage Management | ✅ Active | NAS Server |
| **Home Assistant** | IoT | 🔄 Planned | Main Server |
| **Vikunja** | Task Management | ✅ Active | Main Server |
| **n8n** | Automatization | ✅ Active | Main Server |


### Network Infrastructure
| Service | Purpose | Status | Network |
|---------|---------|---------|---------|
| **OPNsense** | Router & firewall | ✅ Active | Network Server |
| **Unbound** | DNS resolver | ✅ Active | Network Server |
| **Suricata** | Intrusion detection | ✅ Active | Network Server |
| **CrowdSec** | Behavioral analysis & IP blocking | ✅ Active | Network Server |
| **WireGuard** | VPN server & client | ✅ Active | Network Server |
| **Tailscale** | VPN remote access | ✅ Active | Network Server |
| **Zenarmor** | Zero Trust threat protecion| ✅ Active | Network Server |

## Technology Stack

### Container Platform
- **Docker** - Container runtime
- **Docker Compose** - Service orchestration
- **Portainer** - Container management UI

### Networking
- **Traefik v3** - Modern reverse proxy
- **Cloudflare** - DNS and SSL certificate management
- **OPNsense** - Network routing and security
- **Tailscale** - Remote access

### Authentication & Security
- **Authentik** - OIDC/SAML identity provider
- **Let's Encrypt** - Automatic SSL certificates
- **IP Whitelisting** - Network access control

### Monitoring & Observability
- **Prometheus** - Metrics collection
- **Grafana** - Dashboards and visualization
- **Graylog** - Logs collection
- **Uptime Kuma** - Service availability monitoring

## Network Design

### Segmentation
- **VLAN 10** - Internal services
- **VLAN 20** - DMZ services
- **VLAN 30** - IoT
- **VLAN 40** - Media  
- **VLAN 50** - WiFi

### Security
- **Local-only access** - Services restricted to private networks
- **Centralized authentication** - Single sign-on via Authentik
- **SSL everywhere** - Automatic HTTPS for all services
- **Network isolation** - VLAN separation and firewall rules

## Service Organization

### Repository Structure
```
homelab/
├── traefik/          # Reverse proxy configuration
├── authentik/        # Identity provider
├── portainer/        # Container management
├── grafana/          #  Grafana Stack
├── telegraf/         #  Metrics collection agent
├── media/           # Plex, Sonarr, Radarr
└── */                # Nextcloud, RSS reader
```

### Configuration Management
- **Environment files** - Service-specific configuration
- **Docker Compose** - Service definitions
- **Dynamic configuration** - Runtime service discovery
- **Template processing** - Environment variable substitution

## Development Workflow

### Local Development
- Services run on development workstation
- Accessible via local network and domain names
- Hot-reload and rapid iteration

### Production Deployment
- Services migrate to dedicated server hardware
- Persistent storage and high availability
- Automated backups and monitoring

## Access Patterns

### Internal Access
- **Direct network access** - Services available on local network
- **DNS resolution** - Custom domain names via local DNS
- **SSO integration** - Single login for all services

### External Access
- **VPN access** - Secure remote connectivity

### External Access (Future)
- **Port forwarding** - Selective service exposure
- **Public services** - Selected services available from internet

## Planned Expansion
1. **Proxmox Backup Server** - Backup servier
2. **Nextcloud** - Self-hosted productivity suite
3. **Immich** - Photo and video management
