# Traefik Reverse Proxy

Modern reverse proxy and load balancer with automatic SSL certificate management for homelab services.

## Overview

Traefik serves as the central entry point for all homelab services, providing:
- Automatic HTTPS with Let's Encrypt certificates via Cloudflare DNS challenge
- Local network access control and security headers
- Integration with Authentik for centralized authentication
- Dynamic service discovery via Docker labels

## Architecture

```
Internet/Local Network → Traefik (80/443) → Docker Services
                            ↓
                      Cloudflare DNS
                      SSL Certificates
```

## Configuration Structure

```
traefik/
├── docker-compose.yml          # Container definition
├── .env                        # Environment variables (not committed)
├── config/
│   ├── traefik.yml            # Main Traefik configuration
│   └── acme.json              # SSL certificates storage
├── dynamic/
│   ├── config.yml             # Middlewares and shared configuration
│   └── services.yml           # Local network services (not committed)
└── logs/                      # Application logs
```

## Key Components

### Entry Points
- **web** (`:80`) - HTTP traffic with automatic HTTPS redirect
- **websecure** (`:443`) - HTTPS traffic with SSL termination

### Certificate Resolver
- **cloudflare** - DNS challenge for automatic SSL certificate generation
- Supports wildcard certificates for `*.domain.com`
- Automatic renewal and storage in `acme.json`

### Middlewares
- **local-only** - Restricts access to private IP ranges
- **security-headers** - Adds security headers (HSTS, XSS protection, etc.)
- **authentik** - Forward authentication to Authentik SSO provider
- **traefik-auth** - Basic authentication for Traefik dashboard

### Providers
- **docker** - Automatic service discovery from Docker labels
- **file** - Static configuration from `dynamic/` directory

## Network Integration

### Docker Network
- Uses external `homelab` network for service communication
- All services connect to this shared network

### External Services
- Proxies to services running on network server (different VLANs)
- Supports both HTTP and HTTPS backends
- SSL certificate verification bypass for self-signed certificates

## Security Features

- **IP Whitelisting** - Local network access only
- **SSL/TLS Enforcement** - Modern TLS configuration
- **Security Headers** - OWASP recommended headers
- **Forward Authentication** - Integration with Authentik SSO
- **Container Security** - No privilege escalation

## Service Discovery

Services are automatically discovered through Docker labels:
```yaml
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.service.rule=Host(`service.domain.com`)"
  - "traefik.http.routers.service.tls.certresolver=cloudflare"
```

## Monitoring

- **Dashboard** - Web interface at `traefik.domain.com`
- **API** - RESTful API for service management
- **Metrics** - Prometheus metrics endpoint
- **Logs** - Structured logging with access logs

## Environment Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `DOMAIN_NAME` | Primary domain | `example.com` |
| `CF_API_EMAIL` | Cloudflare account email | `user@example.com` |
| `CF_DNS_API_TOKEN` | Cloudflare DNS API token | `abc123...` |

## Dependencies

- **Docker & Docker Compose** - Container runtime
- **Cloudflare Account** - DNS management and API access
- **External Network** - `homelab` Docker network
