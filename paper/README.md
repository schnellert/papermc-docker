# Paper

[![Synchronize Paper](https://github.com/schnellert/papermc-docker/actions/workflows/paper.yml/badge.svg)](https://github.com/schnellert/papermc-docker/actions/workflows/paper.yml)

> Docker images for [Paper](https://papermc.io/software/paper), built from the official releases provided by the [PaperMC Fill API](https://fill.papermc.io/swagger-ui/index.html#).

**Platforms:** `linux/amd64` and `linux/arm64`.


## 🏷️ Tags

- `<version>`           - latest build for that version, e.g. `1.21.11`
- `<version>-build.<n>` - a specific, immutable build, e.g. `1.21.11-build.42`

Pin `<version>-build.<n>` for reproducible deployments; use `<version>` to follow the newest build in that version. 

## Platforms

- `linux/amd64`
- `linux/arm64`

## ⚙️ Configuration

| Variable            | Description                                                   | Default         |
|---------------------|---------------------------------------------------------------|-----------------|
| `ACCEPT_EULA`       | Minecraft EULA. The instance will not start until is `true`.  | `false`         |
| `JAVA_TOOL_OPTIONS` | JVM options.                                                  | `-Xms2G -Xmx2G` |

Persistent data lives in `/data`. The container runs on a non-root user (**UID 1000**), so the mounted host directory must be writable by that user.

For example:
```bash
mkdir -p /data/paper-1
chown -R 1000:1000 /data/paper-1
```

## 🚀 Deployment Examples

Paper can be deployed using different container orchestration platforms. The examples below focus on:

* [Docker](#docker)
* [Docker Compose](#docker-compose)

> [!NOTE]
> These images are regular JRE-based container images, not distroless images. This keeps them flexible for custom runtime requirements, such as overriding the Docker entrypoint to run initialization scripts, modify configuration files, or perform other startup tasks before Paper is launched.
> 
> The same approach can also be used with orchestration platforms such as **Docker Swarm** or **Kubernetes**.

> [!IMPORTANT]
> Paper is **stateful**. Each instance must use its own persistent volume.
> 
> Do **not** share the same writable `/data` directory between multiple Paper instances/replicas.

## Docker

### a.) Example

```bash
docker run -dit \
   --name paper-1 \
   -e ACCEPT_EULA=true \
   -p 25565:25565 \
   -v /data/paper-1:/data \
   schnellert/paper:1.21.11
```

### b.) Options

- `-d` - runs the container i **detached mode**.
- `-i` - keeps **STDIN** open for interactive input.
- `-t` - allocates a **pseudo-TTY**, allowing you to attach to the Paper console.

### c.) Manage the instance

Attach to the console:
```bash
docker attach paper-1
```

Detach from the console without stopping the container:
```text
Ctrl+P, Ctrl+Q
```

View the logs:
```bash
docker logs -f paper-1
```

## Docker Compose

### a.) Example

```yaml
services:
  paper-1:
    image: schnellert/paper:1.21.11
    volumes:
      - /data/paper-1:/data
    environment:
      ACCEPT_EULA: 'true'
    ports:
      - 25565:25565
    stdin_open: true
    tty: true
```

### b.) Options

`stdin_open` and `tty` correspond to Docker's interactive and TTY options:

- `stdin_open: true`  - keeps **STDIN** open.
- `tty: true`         - allocates a **pseudo-TTY**.

### c.) Manage the instance

Start the instance:
```bash
docker compose up -d
```

Attach to the console:
```bash
docker compose attach paper-1
```

Detach from the console without stopping the container:
```text
Ctrl+P, Ctrl+Q
```

View the logs:
```bash
docker compose -f paper-1
```
