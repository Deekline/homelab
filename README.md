# Homelab Infrastructure

Self-hosted services and applications for personal use, built with Docker and modern DevOps practices.

## Architecture Overview

| Network Server      | Main Server          | External DNS      |
|---------------------|---------------------|-------------------|
| **(24/7 Ops)**      | **(Development)**   | **(Cloudflare)**  |
| OPNsense            | Traefik             | DNS Records       |
| DNS Server          | Authentik           | SSL Certs         |
| Monitoring          | Portainer           | API Access        |
| Core Services       | Media Stack         |                   |

## Service Stack

### Core Infrastructure
| Service | Purpose | Status | Network |
|---------|---------|---------|---------|
| **Traefik** | Reverse proxy & SSL termination | ✅ Active | Main Server |
| **Authentik** | Identity provider & SSO | ✅ Active | Main Server |
| **Portainer** | Docker management interface | ✅ Active | Main Server |

### Monitoring & Management
| Service | Purpose | Status | Network |
|---------|---------|---------|---------|
| **Uptime Kuma** | Service monitoring | ✅ Active | Network Server |
| **Homepage** | Service dashboard | ✅ Active | Network Server |
| **Prometheus** | Metrics collection | 🔄 Planned | Main Server |
| **Grafana** | Metrics visualization | 🔄 Planned | Main Server |

### Media & Content
| Service | Purpose | Status | Network |
|---------|---------|---------|---------|
| **RSS Reader** | News aggregation | 🔄 Planned | Main Server |
| **Plex/Jellyfin** | Media streaming | 🔄 Planned | Main Server |
| **Sonarr/Radarr** | Media management | 🔄 Planned | Main Server |
| **qBittorrent** | Torrent client | 🔄 Planned | Main Server |
| **Gluetun** | VPN container for torrenting | 🔄 Planned | Main Server |

### Productivity & Storage
| Service | Purpose | Status | Network |
|---------|---------|---------|---------|
| **Nextcloud** | File storage & collaboration | 🔄 Planned | Main Server |
| **Immich** | Photo management | 🔄 Planned | Main Server |

### Network Infrastructure
| Service | Purpose | Status | Network |
|---------|---------|---------|---------|
| **OPNsense** | Router & firewall | ✅ Active | Network Server |
| **Unbound** | DNS resolver | ✅ Active | Network Server |
| **Suricata** | Intrusion detection | ✅ Active | Network Server |
| **CrowdSec** | Behavioral analysis & IP blocking | ✅ Active | Network Server |
| **WireGuard** | VPN server & client | ✅ Active | Network Server |

## Technology Stack

### Container Platform
- **Docker** - Container runtime
- **Docker Compose** - Service orchestration
- **Portainer** - Container management UI

### Networking
- **Traefik v3** - Modern reverse proxy
- **Cloudflare** - DNS and SSL certificate management
- **OPNsense** - Network routing and security

### Authentication & Security
- **Authentik** - OIDC/SAML identity provider
- **Let's Encrypt** - Automatic SSL certificates
- **IP Whitelisting** - Network access control

### Monitoring & Observability
- **Prometheus** - Metrics collection
- **Grafana** - Dashboards and visualization
- **Uptime Kuma** - Service availability monitoring

## Network Design

### Segmentation
- **VLAN 10** - Internal services
- **VLAN 20** - DMZ services  
- **WiFi Network** - Development workstation

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
├── monitoring/       # Prometheus & Grafana
├── media/           # Plex, Sonarr, Radarr
└── productivity/    # Nextcloud, RSS reader
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

### External Access (Future)
- **Port forwarding** - Selective service exposure
- **VPN access** - Secure remote connectivity
- **Public services** - Selected services available from internet

## Planned Expansion

### Short Term
1. **RSS Reader** - News and content aggregation
2. **Prometheus & Grafana** - Comprehensive monitoring
3. **Media Stack** - Automated media management

### Medium Term
1. **Nextcloud** - Self-hosted productivity suite
2. **Immich** - Photo and video management
3. **VPN Server** - Secure remote access

### Long Term
1. **High Availability** - Multi-node setup
2. **Kubernetes** - Container orchestration platform
3. **GitOps** - Automated deployment pipeline
