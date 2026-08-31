# 01 — Introduction to Docker

## The problem Docker solves

"It works on my machine" is the classic developer complaint. An app might depend on a specific language version, specific system libraries, specific config files, in a specific OS — and getting all of that identical across your laptop, your teammate's laptop, and the production server is genuinely hard.

Docker's answer: package the application **together with everything it needs to run** (code, runtime, libraries, system tools, settings) into one unit called a **container**. That unit runs the same way everywhere — your machine, a colleague's machine, or a cloud server — because it's not relying on whatever happens to already be installed there.

## Containers vs. Virtual Machines

This is the single most important mental model to get right early.

**A Virtual Machine (VM)** virtualizes an entire computer, including its own operating system kernel. A hypervisor sits on the host and runs several complete guest OSes side by side. Each VM is heavy (gigabytes), boots slowly (minutes), and duplicates a lot of overhead.

**A container** does not include its own OS kernel. It shares the host machine's kernel and is just an isolated *process* with its own filesystem, network, and view of the system — enforced by kernel features (namespaces and cgroups), not full hardware virtualization. Containers are lightweight (megabytes), start almost instantly (sub-second to a few seconds), and you can run dozens of them where you might run only a few VMs.

```
VIRTUAL MACHINES                     CONTAINERS
┌─────────┐ ┌─────────┐              ┌─────────┐ ┌─────────┐
│  App A  │ │  App B  │              │  App A  │ │  App B  │
├─────────┤ ├─────────┤              ├─────────┤ ├─────────┤
│ Guest OS│ │ Guest OS│              │  (libs, │ │  (libs, │
│ (full)  │ │ (full)  │              │  bins)  │ │  bins)  │
├─────────┴─┴─────────┤              ├─────────┴─┴─────────┤
│     Hypervisor       │              │    Docker Engine     │
├───────────────────────┤              ├───────────────────────┤
│     Host OS kernel    │              │     Host OS kernel    │
├───────────────────────┤              ├───────────────────────┤
│       Hardware         │              │       Hardware         │
└───────────────────────┘              └───────────────────────┘
```

Neither is strictly "better" — VMs give you stronger isolation (separate kernels) and can run a different OS entirely (e.g. Windows VM on a Linux host); containers give you speed, density, and consistency for the very common case of "run this app the same way everywhere on the same kernel family (Linux)."

## The two core building blocks

- **Image** — a read-only template: your app's code plus everything it needs, packaged in layers. Think of it like a class in programming, or a recipe.
- **Container** — a running instance created from an image. Think of it like an object/instance, or the dish made from the recipe. You can create many containers from one image.

You will use this analogy constantly: **image = blueprint, container = the thing built from the blueprint.**

## Why Docker specifically (a little history/context)

Linux has had the underlying isolation technology (namespaces, cgroups) for a long time. Docker (launched 2013) didn't invent containers — it made them dramatically easier to use, by providing a simple CLI, the Dockerfile format for repeatable builds, and Docker Hub for easily sharing images. That combination is why "Docker" became almost synonymous with "containers" even though other tools (Podman, containerd directly, etc.) also implement the same underlying ideas.

## Key terms introduced in this module

| Term | Meaning |
|---|---|
| **Image** | Read-only template used to create containers |
| **Container** | A running instance of an image |
| **Docker Engine** | The software that builds/runs containers |
| **Kernel** | The core of the OS that containers share with the host |
| **Namespace** | Kernel feature giving a container its own isolated view (processes, network, etc.) |
| **cgroups (control groups)** | Kernel feature that limits/measures how much CPU, memory, etc. a container can use |
| **Hypervisor** | Software that creates/runs full virtual machines (not used by containers) |

## Hands-on lab

You don't need Docker installed yet for this — it's a thinking exercise, best done right before Module 02.

1. Think of an app you've built or used (even something small — a to-do list, a script). List every piece of "environment" it silently depends on: language version, OS packages, environment variables, a database, config files.
2. Now imagine handing that app to a friend with a totally different laptop. Which of those dependencies would likely break for them?
3. Keep that list — you'll come back to it in Module 06 when you write your first Dockerfile and see exactly how each of those dependencies gets explicitly declared instead of silently assumed.

## Common pitfalls

- **"Docker = a VM."** No — no guest kernel, much lighter, faster startup. Don't reach for VM mental models when debugging container issues.
- **Confusing image and container.** If you're not sure which word applies, ask: "is this the blueprint, or the running thing?"
- **Thinking Docker only runs Linux apps.** Docker Desktop on Windows/Mac runs a lightweight Linux VM behind the scenes specifically so Linux containers can run — but Windows containers (running actual Windows) also exist as a separate, less common track.

## Checkpoint questions

1. In your own words, what's the difference between an image and a container?
2. Why is a container typically much faster to start than a virtual machine?
3. What two kernel features let a container behave as if it's isolated from other processes on the same machine?

---

⬅ [Back to main README](../README.md) | ➡ [Next: 02 - Installation & Setup](../02-installation/README.md)
