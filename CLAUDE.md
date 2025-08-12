# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains the Seafile client daemon - the sync client component of the Seafile open source cloud storage system. This is part of a larger ecosystem with separate repositories for the server core, web UI, and various client applications.

## Architecture

### Core Components

- **Debian Packaging System**: This repository is configured as a Debian package builder for Seafile client components
- **Build Script**: `create-seafile-dpkg` - comprehensive containerized build system for creating DEB packages
- **Client Daemon**: Core sync functionality (seaf-daemon)
- **CLI Interface**: Command-line tools (seaf-cli) 
- **Python Bindings**: Python 3 RPC client interface for interacting with the daemon
- **Shared Library**: libseafile C library with development headers

### Package Structure

The repository builds multiple Debian packages:
- `seafile-daemon` - Core daemon binary
- `seafile-cli` - Command-line interface
- `python3-seafile` - Python bindings
- `libseafile0t64` - Shared library runtime
- `libseafile-dev` - Development headers
- `seafile-gui` - Desktop GUI client (when built)

## Build System

### Primary Build Command

```bash
# Build all packages with defaults (Ubuntu 24.04, Seafile 9.0.14)
./create-seafile-dpkg

# Build for different Ubuntu version
./create-seafile-dpkg --os-version 22.04

# Build only daemon packages (no GUI)
./create-seafile-dpkg --daemon-only

# Build with cleanup
./create-seafile-dpkg --clean
```

### Build Dependencies

The build system uses containerized builds with Podman/Docker and requires:
- Container runtime (podman or docker)
- Git for source fetching
- Internet connection for dependencies

### Build Process

1. Clones Seafile source from GitHub (specific version tags)
2. Copies debian packaging files from this repository to source
3. Sets up containerized Ubuntu build environment
4. Installs build dependencies via apt
5. Compiles and packages using `dpkg-buildpackage`
6. Runs installation tests in clean container
7. Outputs DEB packages to build directory

**Note**: Only essential debian packaging source files are stored in this repository. Build artifacts are automatically generated during the build process.

## Development Commands

### Package Installation
```bash
# Install all built packages
sudo dpkg -i *9.0.14*.deb
sudo apt-get install -f  # Fix dependencies

# Test installation
seaf-daemon --version
seaf-cli --version
python3 -c "import seafile; print('Python module works')"
```

### Build Options
```bash
# Full option list
./create-seafile-dpkg --help

# Container runtime selection
./create-seafile-dpkg --container-runtime docker

# Skip tests for faster builds
./create-seafile-dpkg --no-tests

# GUI-only build (requires existing daemon packages)
./create-seafile-dpkg --gui-only
```

### Supported Configurations
- Ubuntu 20.04, 22.04, 24.04 LTS
- Any Seafile version with GitHub releases
- amd64 architecture

## Code Organization

### Python RPC Interface
- Located in: `debian/python3-seafile/usr/lib/python3/dist-packages/seafile/`
- `rpcclient.py`: Main RPC client with searpc decorators for daemon communication
- `__init__.py`: Public API exports and task type definitions

### C Headers
- Located in: `debian/tmp/usr/include/seafile/`
- Core API functions for repository management, syncing, configuration
- Uses GObject/GLib types and SearpcClient for communication

### Debian Packaging
- `debian/control`: Package definitions and dependencies
- `debian/rules`: Build rules with debhelper
- `create-seafile-dpkg`: Main build orchestration script

## Common Issues

### GUI Package Building
- Qt5WebEngine unavailable on Ubuntu 24.04, script falls back to Qt5WebKit
- For full GUI support, use Ubuntu 22.04 or earlier

### Container Runtime
- Script defaults to Podman but supports Docker
- Ensure container runtime is installed and accessible

### Build Failures
- Use `--clean` flag to clear build directories
- Check container runtime availability
- Verify internet connectivity for source downloads