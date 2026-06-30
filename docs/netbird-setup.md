# NetBird Self-Hosted Setup

These templates use the **new combined architecture** (v0.65.0+) — management, signal, relay, and STUN all run in a single `netbird-server` container. The old multi-container setup (separate management, signal, relay, coturn) is deprecated.

## Prerequisites

1. A **public domain** pointed at your Unraid server (e.g., `netbird.yourdomain.com`)
2. A **reverse proxy** (Nginx Proxy Manager, SWAG, Traefik) handling TLS termination
3. A **`config.yaml`** — run NetBird's `getting-started.sh` on any Linux machine to generate one, then place it at `/mnt/user/appdata/netbird-server/config/config.yaml`
4. TCP ports **80/443** and UDP port **3478** open on your firewall

## Installation Order

1. Start **NetBird-Server** first
2. Then start **NetBird-Dashboard**

## Reverse Proxy Configuration

Configure your reverse proxy to:

| Service | Internal URL |
|---|---|
| NetBird-Server (HTTP/gRPC) | `http://<unraid-ip>:8081` |
| NetBird-Dashboard | `http://<unraid-ip>:8080` |

Expose UDP port **3478** for STUN (required for NAT traversal).

### Path Routing (Nginx Example)

```nginx
server {
    listen 443 ssl;
    server_name netbird.yourdomain.com;

    # Dashboard
    location / {
        proxy_pass http://unraid-ip:8080;
    }

    # gRPC paths (must use HTTP/2)
    location /signalexchange.SignalExchange/ {
        grpc_pass grpc://unraid-ip:8081;
    }
    location /management.ManagementService/ {
        grpc_pass grpc://unraid-ip:8081;
    }
    location /management.ProxyService/ {
        grpc_pass grpc://unraid-ip:8081;
    }

    # Relay, WebSocket, API, OAuth2
    location ~ ^/(relay|ws-proxy/|api|oauth2) {
        proxy_pass http://unraid-ip:8081;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```

## Generating config.yaml

On any Linux machine with Docker:

```bash
curl -fsSL https://github.com/netbirdio/netbird/releases/latest/download/getting-started.sh | bash
```

Select option `[0]` (Traefik) or your preferred reverse proxy. After the script completes, copy `config.yaml` from the output directory to:

```
/mnt/user/appdata/netbird-server/config/config.yaml
```

Then edit `config.yaml` to ensure `listenAddress: ":80"` (since the reverse proxy handles TLS).

## Ports

| Port | Protocol | Container | Purpose |
|---|---|---|---|
| 8081 | TCP | netbird-server | HTTP/gRPC (reverse proxy target) |
| 3478 | UDP | netbird-server | STUN for NAT traversal |
| 8080 | TCP | netbird-dashboard | Web UI (reverse proxy target) |
