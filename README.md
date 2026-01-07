# 🐳 Mini-Docker

**A Fully Functional Container Runtime Built from Scratch in Python**

Learn how containers really work by building one from scratch.

---

## What is Mini-Docker?

Mini-Docker is an **educational container runtime** that implements the core technologies powering Docker, Podman, and other container runtimes. It demonstrates how containers work at the Linux kernel level.

**Why Mini-Docker?**

| Challenge | Solution |
|-----------|----------|
| Docker source is complex (~100k lines) | Clean Python implementation (~3k lines) |
| Hard to understand internals | Every function documented |
| Intimidating codebase | Simple, readable modules |

---

## 🎥 Demo

[Watch Demo Run](examples/demorun.mp4)

---


## ✨ Features

### Core Features

| Feature | Description |
|---------|-------------|
| **Namespace Isolation** | All 7 Linux namespaces (PID, UTS, Mount, IPC, Network, User, Cgroup) |
| **Cgroups v2** | Resource limiting (CPU, memory, process count) |
| **Seccomp-BPF** | System call filtering with strict whitelist |
| **Capabilities** | Linux capability dropping for reduced privileges |
| **OverlayFS** | Copy-on-write filesystem for efficient storage |
| **Virtual Networking** | Veth pairs with bridge and NAT |
| **OCI Compatibility** | OCI runtime specification support |
| **Pod Support** | Kubernetes-style pod grouping |
| **Rootless Mode** | Run containers without root via user namespaces |
| **Image Builder** | Build container images from Imagefiles |

### Security Layers

```
Layer 1: Namespaces      → Process isolation (7 namespace types)
Layer 2: Cgroups v2      → Resource limits (CPU, memory, PIDs)
Layer 3: Seccomp-BPF     → Syscall filtering (~50 whitelisted calls)
Layer 4: Capabilities    → Privilege reduction (drop dangerous caps)
Layer 5: NO_NEW_PRIVS    → Escalation prevention (prctl flag)
```

---

## 🚀 Quick Start

### Prerequisites

| Requirement | Minimum | Check Command |
|-------------|---------|---------------|
| Linux Kernel | 4.18+ | `uname -r` |
| Python | 3.7+ | `python3 --version` |
| Root access | Yes (or rootless) | `whoami` |

### Installation

```bash
# Clone the repository
git clone https://github.com/Tarunpathak001/Mini-Docker.git
cd Mini-Docker

# Run setup script (creates rootfs)
sudo ./scripts/setup.sh

# Verify installation
python3 -m mini_docker --help
```

### Your First Container

```bash
# Run an interactive shell in a container
sudo python3 -m mini_docker run ./rootfs /bin/sh
```

Inside the container:
```bash
echo $$              # Shows PID 1 (you're init!)
hostname container   # Change hostname (isolated)
ps aux               # Only see container processes
exit                 # Exit container
```

---

## 💻 Usage

### Basic Commands

```bash
# Run a simple command
sudo python3 -m mini_docker run ./rootfs /bin/echo "Hello World"

# Run with a custom hostname
sudo python3 -m mini_docker run --hostname mycontainer ./rootfs /bin/sh

# Run with a name
sudo python3 -m mini_docker run --name web ./rootfs /bin/sh
```

### Resource Limits

```bash
# Limit memory to 100MB
sudo python3 -m mini_docker run --memory 100M ./rootfs /bin/sh

# Limit CPU to 50%
sudo python3 -m mini_docker run --cpu 50 ./rootfs /bin/sh

# Limit number of processes (fork bomb protection)
sudo python3 -m mini_docker run --pids 10 ./rootfs /bin/sh

# Combine all limits
sudo python3 -m mini_docker run \
    --memory 100M \
    --cpu 50 \
    --pids 10 \
    ./rootfs /bin/sh
```

### Container Management

```bash
# List running containers
sudo python3 -m mini_docker ps

# List all containers (including stopped)
sudo python3 -m mini_docker ps -a

# Stop a container
sudo python3 -m mini_docker stop mycontainer

# Remove a container
sudo python3 -m mini_docker rm mycontainer

# Execute command in running container
sudo python3 -m mini_docker exec mycontainer /bin/ls
```

### Pod Support (Kubernetes-style)

```bash
# Create a pod
sudo python3 -m mini_docker pod create mypod

# Add containers to pod (shared namespaces)
sudo python3 -m mini_docker pod add mypod --name app ./rootfs /bin/sh

# List pod containers
sudo python3 -m mini_docker pod ps mypod

# Remove pod
sudo python3 -m mini_docker pod rm mypod
```

### Rootless Mode

```bash
# Run without root (uses user namespaces)
python3 -m mini_docker run --rootless ./rootfs /bin/sh
```

---

## 📐 Project Structure

```
Mini-Docker/
├── mini_docker/           # Core runtime code (~3,000 lines)
│   ├── cli.py            # Command-line interface
│   ├── container.py      # Container lifecycle
│   ├── namespaces.py     # Linux namespaces
│   ├── cgroups.py        # Cgroups v2
│   ├── seccomp.py        # Seccomp-BPF filtering
│   ├── capabilities.py   # Linux capabilities
│   ├── filesystem.py     # OverlayFS
│   ├── network.py        # Virtual networking
│   ├── oci.py            # OCI runtime spec
│   ├── pod.py            # Pod management
│   └── image_builder.py  # Image building
├── docs/                 # Documentation
├── examples/             # Working examples
├── tests/                # Unit tests
└── rootfs/               # Minimal root filesystem
```

---

## 📚 Documentation

| Document | Description |
|----------|-------------|
| [Quick Start](docs/QUICKSTART.md) | Get running in 5 minutes |
| [CLI Commands](docs/CLI-COMMANDS.md) | Complete command reference |
| [Architecture](docs/ARCHITECTURE.md) | System design deep dive |
| [Security Model](docs/SECURITY-MODEL.md) | Security layers explained |
| [Examples](docs/EXAMPLES.md) | Use cases and code samples |
| [Troubleshooting](docs/TROUBLESHOOTING.md) | Common issues and fixes |
| [Benchmarks](docs/BENCHMARKS.md) | Performance measurements |

---

## 🔒 Security Notice

> ⚠️ **Mini-Docker is an educational tool, NOT a production-ready container runtime.**

For production workloads, use:
- [runc](https://github.com/opencontainers/runc) - OCI reference implementation
- [crun](https://github.com/containers/crun) - Fast C implementation
- [Docker](https://www.docker.com/) - Full container platform
- [Podman](https://podman.io/) - Daemonless containers

See [SECURITY.md](SECURITY.md) for vulnerability reporting.

---

## 🛠️ Development

```bash
# Run tests
pytest tests/ -v

# Run linter
flake8 mini_docker/
black --check mini_docker/

# Type checking
mypy mini_docker/
```

---

## 🤝 Contributing

Found an issue or want to improve something? Feel free to open a pull request!

---

## 📄 License

MIT License - see [LICENSE](LICENSE) for details.

---

<div align="center">

**Mini-Docker** - Understanding containers from the ground up.

Made with ❤️ for learning

</div>
