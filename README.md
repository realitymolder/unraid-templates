# Unraid CA Templates

A collection of Unraid Community Applications (CA) templates for Docker containers.

## Templates

| Template | Description |
|---|---|---|
| `netbird-server.xml` | NetBird combined server (management, signal, relay, STUN — v0.65+ architecture) |
| `netbird-dashboard.xml` | NetBird web dashboard (requires NetBird-Server) |
| `odysseus.xml` | Main Odysseus AI workspace (standalone install, requires companion containers) |
| `odysseus-aio.xml` | Odysseus AIO master container (all-in-one, auto-deploys full stack) |
| `chromadb.xml` | ChromaDB vector database (companion for standalone Odysseus) |

## Adding to Unraid

1. Go to **Community Applications → Settings → Repositories**
2. Add this repository URL:
   ```
   https://github.com/realitymolder/unraid-templates
   ```
3. Templates will appear under the **NetBird** and **Odysseus** categories

### NetBird Setup Notes

NetBird v0.65.0+ consolidated the old 5-container architecture into a single `netbird-server` container. These templates use the new combined architecture.

Before installing the templates, you need:
1. A **public domain** pointed at your Unraid server (e.g., `netbird.yourdomain.com`)
2. A **reverse proxy** (Nginx Proxy Manager, SWAG, Traefik) handling TLS termination
3. A **`config.yaml`** — run NetBird's `getting-started.sh` on any Linux machine to generate one, then copy it to `/mnt/user/appdata/netbird-server/config.yaml`
4. TCP ports **80/443** and UDP port **3478** open on your firewall

**Installation order:** Start `NetBird-Server` first, then `NetBird-Dashboard`. Configure your reverse proxy to route `netbird.yourdomain.com` → `NetBird-Server:8081` and expose port 3478/UDP for STUN.

## Adding New Templates

1. Create an XML file following the [Unraid Docker Template XML Schema](https://forums.unraid.net/topic/38619-docker-template-xml-schema/)
2. Add a 192x192 PNG icon to `templates/img/`
3. Submit a pull request

### Template guidelines

- Use lowercase, hyphenated filenames (e.g. `my-app.xml`)
- Include `<Registry>` pointing to the Docker Hub or GHCR page
- Include `<Support>` and `<Project>` links
- Icon should be 192x192 PNG or SVG

## Links

- [Unraid Community Applications](https://forums.unraid.net/topic/128970-community-applications/)
- [Unraid Docker Template XML Schema](https://forums.unraid.net/topic/38619-docker-template-xml-schema/)
- [Writing a template compatible for Unraid](https://selfhosters.net/docker/templating/templating/)
