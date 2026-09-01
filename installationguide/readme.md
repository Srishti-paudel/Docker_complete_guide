# 02: Installation & Setup

## What you're actually installing

"Installing Docker" installs several pieces at once:

- **Docker Engine**, the daemon (`dockerd`) that does the real work, plus the CLI (`docker`) you type commands into.
- **Docker CLI**, the command-line tool that talks to the daemon.
- **Docker Compose**, the multi-container tool (now built into the `docker` CLI as `docker compose`, no space needed before "compose" as a subcommand).
- **BuildKit**, the modern image-build engine (used automatically by newer Docker versions).

## Docker Desktop vs. Docker Engine (Linux)

- **Docker Desktop** (Windows/macOS/Linux): a GUI app that bundles everything above, plus a dashboard for browsing containers/images/volumes visually. On Windows/macOS it also runs a small lightweight VM under the hood, because Linux containers need a Linux kernel, and Windows/macOS don't have one natively.
- **Docker Engine (Linux, no Desktop)**: on native Linux, you can install just the engine and CLI directly, no VM needed, since the host kernel already is Linux. This is the typical setup on Linux servers.

Beginners on Windows/macOS should use Docker Desktop. Linux users can use either; the Engine-only route is common for servers.

## Installing

**Exact steps vary by OS/version and change over time, follow Docker's current official instructions rather than a fixed guide baked into this repo:**

- Windows/macOS: [docker.com/get-started](https://www.docker.com/products/docker-desktop/)
- Linux: [docs.docker.com/engine/install](https://docs.docker.com/engine/install/) (pick your distro)

After installing, **on Linux**, add your user to the `docker` group so you don't need `sudo` for every command:

```bash
sudo usermod -aG docker $USER
# then log out and back in (or run `newgrp docker`)
```

## Verifying your installation

```bash
docker --version
docker compose version
docker info
```

`docker info` is worth actually reading once. It shows you the number of containers/images, storage driver, and whether the daemon is reachable at all (a common early error is "Cannot connect to the Docker daemon" which just means Docker Desktop / the `dockerd` service isn't running yet).

## Key terms introduced in this module

| Term | Meaning |
|---|---|
| **Docker daemon (`dockerd`)** | Background service that manages images, containers, networks, volumes |
| **Docker CLI** | The `docker` command you type; it sends requests to the daemon |
| **Docker Desktop** | GUI app bundling the engine + CLI + Compose + dashboard |
| **Docker Compose** | Tool/CLI plugin for multi-container apps |
| **BuildKit** | Modern, faster image-build backend used by `docker build` |

## Hands-on lab

1. Install Docker Desktop (or Engine on Linux) following the official docs linked above.
2. Run:
 ```bash
 docker --version
 docker info
 ```
3. Run the traditional "hello world":
 ```bash
 docker run hello-world
 ```
 Read the output carefully. It actually explains, in plain English, the exact sequence of steps Docker just performed (contacting the daemon, pulling the image, creating a container, running it, printing output, exiting). This is worth reading twice; it's a preview of the whole architecture you'll learn in Module 03.
4. List what got downloaded:
 ```bash
 docker images
 docker ps -a
 ```
 You should see the `hello-world` image and a stopped container from step 3.

## Common pitfalls

- **"Cannot connect to the Docker daemon"**, Docker Desktop isn't running, or on Linux the `docker` service isn't started (`sudo systemctl start docker`).
- **Needing `sudo` for every command on Linux**, fixed by adding your user to the `docker` group (see above); don't just prefix every command with `sudo` forever.
- **Low disk space surprises later**, Docker images/containers/volumes accumulate over time; you'll learn cleanup commands in the cheat sheet, but know this is normal and expected, not a bug.

## Checkpoint questions

1. What's the difference between the Docker CLI and the Docker daemon?
2. Why does Docker Desktop on macOS/Windows run a hidden VM, but Docker Engine on native Linux doesn't need one?
3. What command would you run to check whether Docker is installed and the daemon is actually reachable?

---

[Previous: 01 - Introduction](../01-introduction/README.md) | [Main README](../README.md) | [Next: 03 - Docker Architecture](../03-docker-architecture/README.md)
