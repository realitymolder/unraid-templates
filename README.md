# Unraid CA Templates

A collection of Unraid Community Applications (CA) templates for Docker containers.

## What are CA Templates?

CA templates are XML files that define how Docker containers integrate with Unraid's Community Applications system. They enable one-click installation of Docker containers through the Unraid web interface. Each template specifies container configuration including:

- Docker image and repository
- Port mappings
- Volume mounts
- Environment variables
- Container metadata (name, description, icons)
- Support and project links

## Templates

| Template | Description |
|---|---|
| `odysseus.xml` | Main Odysseus AI workspace (standalone install, requires companion containers) |
| `odysseus-aio.xml` | Odysseus AIO master container (all-in-one, auto-deploys full stack) |
| `chromadb.xml` | ChromaDB vector database (companion for standalone Odysseus) |

## Adding to Unraid

1. Go to **Community Applications → Settings → Repositories**
2. Add this repository URL:
   ```
   https://github.com/realitymolder/odysseus-ca-templates
   ```
3. Templates will appear under the **Odysseus** category

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
