# Velocity

[![Synchronize Velocity](https://github.com/schnellert/papermc-docker/actions/workflows/velocity.yml/badge.svg)](https://github.com/schnellert/papermc-docker/actions/workflows/velocity.yml)

> Docker images for [Velocity](https://papermc.io/software/velocity), built from the official releases provided by the [PaperMC Fill API](https://fill.papermc.io/swagger-ui/index.html#).

## 🏷️ Tags

- `<version>`           - latest build for that version, e.g. `4.1.0-SNAPSHOT`
- `<version>-build.<n>` - a specific, immutable build, e.g. `4.1.0-SNAPSHOT-build.21`

Pin `<version>-build.<n>` for reproducible deployments; use `<version>` to follow the newest build in that version. 

## Platforms

- `linux/amd64`
- `linux/arm64`

## ⚙️ Configuration

| Variable            | Description                                                   | Default           |
|---------------------|---------------------------------------------------------------|-------------------|
| `JAVA_TOOL_OPTIONS` | JVM options.                                                  | `-Xms512M -Xmx1G` |

Persistent data lives in `/data`. The container runs on a non-root user (**UID 1000**), so the mounted host directory must be writable by that user.

For example:
```bash
mkdir -p /data/velocity-1
chown -R 1000:1000 /data/velocity-1
```

## 🚀 Deployment Examples

Paper can be deployed using different container orchestration platforms. The examples below focus on:

* [Docker](#docker)
* [Docker Compose](#docker-compose)

> [!NOTE]
> These images are regular JRE-based container images, not distroless images. This keeps them flexible for custom runtime requirements, such as overriding the Docker entrypoint to run initialization scripts, modify configuration files, or perform other startup tasks before Paper is launched.
> 
> The same approach can also be used with orchestration platforms such as **Docker Swarm** or **Kubernetes**.

## Docker

### a.) Example

```bash
docker run -dit \
   --name velocity-1 \
   -p 25565:25565 \
   -v /data/velocity-1:/data \
   schnellert/velocity:4.1.0-SNAPSHOT
```

### b.) Options

- `-d` - runs the container i **detached mode**.
- `-i` - keeps **STDIN** open for interactive input.
- `-t` - allocates a **pseudo-TTY**, allowing you to attach to the Paper console.

### c.) Manage the instance

Attach to the console:
```bash
docker attach velocity-1
```

Detach from the console without stopping the container:
```text
Ctrl+P, Ctrl+Q
```

View the logs:
```bash
docker logs -f velocity-1
```

## Docker Compose

### a.) Example

```yaml
services:
  velocity-1:
    image: schnellert/velocity:4.1.0-SNAPSHOT
    volumes:
      - /data/velocity-1:/data
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
docker compose attach velocity-1
```

Detach from the console without stopping the container:
```text
Ctrl+P, Ctrl+Q
```

View the logs:
```bash
docker compose -f velocity-1
```
