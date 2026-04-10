# devcontainer-vscode

A simple VS Code devcontainer for Java development, backed by Docker Compose.

Open the folder in VS Code and choose "Reopen in Container" to get a fully configured Java development environment with a database and safe Docker access.

## Getting Started

1. Copy `.env.template` to `.env` and fill in the values.
2. Open the folder in VS Code.
3. When prompted, select "Reopen in Container" (or run the command via the Command Palette).

## Docker Compose Services

| Service | Description |
|---|---|
| **app** | Main development container. Built from the local Dockerfile. Exposes port 8080. Mounts the workspace, Maven cache, Claude cache, VS Code Server data/extensions, SSH keys, and gitconfig. |
| **database** | PostgreSQL 17. Credentials are read from `.env`. Data is persisted in a named volume. |
| **docker-socket-proxy** | [Tecnativa Docker socket proxy](https://github.com/Tecnativa/docker-socket-proxy). Gives the `app` container scoped, read-only-friendly access to the Docker daemon. Required for TestContainers. |

## Dockerfile

The `app` image is built from **Eclipse Temurin JDK 21**. It installs common development tools (`git`, `curl`, `wget`, `vim`, `zsh`, `docker-cli`, etc.), creates a non-root `vscode` user (UID/GID 501), sets up Oh My Zsh with the `git` and `mvn` plugins, and pre-creates cache directories for VS Code Server, Maven (`.m2`), and Claude Code (`.claude`).
