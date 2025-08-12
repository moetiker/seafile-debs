# Seafile DEB Package Builder

This directory contains a comprehensive tool for building Seafile DEB packages for Ubuntu systems, along with pre-built packages for Seafile v9.0.14.

## 📦 Pre-built Packages (v9.0.14 for Ubuntu 24.04)

Ready-to-install packages are included:

| Package | Size | Description |
|---------|------|-------------|
| `seafile-daemon_9.0.14-1_amd64.deb` | 170 KB | Core Seafile daemon with `seaf-daemon` |
| `seafile-cli_9.0.14-1_all.deb` | 16 KB | Command-line interface with `seaf-cli` |
| `python3-seafile_9.0.14-1_all.deb` | 11 KB | Python 3 bindings for Seafile |
| `libseafile0t64_9.0.14-1_amd64.deb` | 26 KB | Shared libraries |
| `libseafile-dev_9.0.14-1_amd64.deb` | 33 KB | Development headers |
| `seafile-gui_9.0.14-1_amd64.deb` | 20 MB | GUI client with `seafile-applet` |

### Quick Installation

```bash
# Install all packages
sudo dpkg -i *9.0.14*.deb

# Fix any dependency issues
sudo apt-get install -f

# Verify installation
seaf-daemon --version
seaf-cli --version
seafile-applet --version
```

## 🛠 Build Script Usage

The `create-seafile-dpkg` script allows building packages for different Ubuntu versions and Seafile versions.

### Basic Usage

```bash
# Build for Ubuntu 24.04 and Seafile 9.0.14 (default)
./create-seafile-dpkg

# Build for different versions
./create-seafile-dpkg --os-version 22.04 --seafile-version 9.0.15

# Build only daemon packages
./create-seafile-dpkg --daemon-only

# Build only GUI package (requires existing daemon packages)
./create-seafile-dpkg --gui-only
```

### Advanced Options

```bash
# Full option list
./create-seafile-dpkg --help

# Examples:
./create-seafile-dpkg --os-version 24.04 --seafile-version 9.0.14 --clean
./create-seafile-dpkg --container-runtime docker --no-tests
./create-seafile-dpkg --os-version 22.04 --daemon-only --clean
```

### Script Options

| Option | Description | Default |
|--------|-------------|---------|
| `--os-version` | Ubuntu version (20.04, 22.04, 24.04) | 24.04 |
| `--seafile-version` | Seafile version (e.g., 9.0.14) | 9.0.14 |
| `--container-runtime` | Container runtime (podman, docker) | podman |
| `--daemon-only` | Build only daemon packages | false |
| `--gui-only` | Build only GUI package | false |
| `--no-tests` | Skip package installation tests | false |
| `--clean` | Clean build directories before building | false |
| `--help, -h` | Show help message | - |

## 🔧 Requirements

- **Container Runtime**: Podman or Docker
- **Git**: For cloning source repositories
- **Internet Connection**: For downloading dependencies

### Installing Requirements (Ubuntu)

```bash
# For Podman (recommended)
sudo apt update
sudo apt install -y podman git

# For Docker (alternative)
sudo apt update
sudo apt install -y docker.io git
sudo usermod -aG docker $USER  # Logout and login again
```

## 📋 Supported Configurations

### Ubuntu Versions
- Ubuntu 20.04 LTS (Focal)
- Ubuntu 22.04 LTS (Jammy)  
- Ubuntu 24.04 LTS (Noble)

### Seafile Versions
- Any released Seafile version (e.g., 9.0.14, 9.0.15, 10.0.0)
- Script automatically handles version-specific requirements

### Package Architecture
- amd64 (64-bit Intel/AMD)
- Packages are compatible with standard Ubuntu installations

## 🏗 Build Process

The script performs the following steps:

1. **Source Acquisition**: Clones Seafile repositories from GitHub
2. **Environment Setup**: Configures build environment in container
3. **Dependency Installation**: Installs required build dependencies
4. **Package Building**: Compiles and packages Seafile components
5. **Testing**: Verifies package installation and functionality
6. **Output**: Saves packages to build directory

### Build Output

Packages are saved in: `build-{SEAFILE_VERSION}-ubuntu{OS_VERSION}/`

Example: `build-9.0.14-ubuntu24.04/`

## ⚠️ Known Issues & Solutions

### GUI Package Building

The GUI package may require additional steps on some systems:

- **Qt5WebEngine**: Not available on Ubuntu 24.04 by default
- **Solution**: Script uses Qt5WebKit as fallback
- **Alternative**: Use Ubuntu 22.04 for full Qt5WebEngine support

### Common Errors

1. **Container runtime not found**
   ```bash
   sudo apt install podman  # or docker.io
   ```

2. **Permission denied**
   ```bash
   chmod +x create-seafile-dpkg
   ```

3. **Network issues**
   ```bash
   # Check internet connection and try again
   ./create-seafile-dpkg --clean
   ```

## 🧪 Testing

The script includes comprehensive testing:

- Package installation verification
- Binary execution tests
- Python module import tests
- Dependency resolution checks

## 📁 Directory Structure

```
seafile/
├── create-seafile-dpkg           # Main build script
├── debian/                       # Debian packaging files
├── README.md                     # This file
├── LICENSE.txt                   # License information
└── *.deb                         # Pre-built packages (v9.0.14)
```

## 🔄 Upgrade Instructions

To upgrade from older Seafile versions:

```bash
# Stop Seafile services
sudo systemctl stop seafile
sudo systemctl stop seahub

# Install new packages
sudo dpkg -i *9.0.14*.deb
sudo apt-get install -f

# Start services
sudo systemctl start seafile
sudo systemctl start seahub
```

## 🐛 Troubleshooting

### Package Installation Issues

```bash
# Check package integrity
dpkg -I package.deb

# Force installation (if needed)
sudo dpkg -i --force-depends package.deb

# Fix broken packages
sudo apt-get install -f
```

### Build Issues

```bash
# Clean build
./create-seafile-dpkg --clean

# Check container runtime
podman version  # or docker version

# Verbose build (check logs)
./create-seafile-dpkg --no-tests  # Skip tests to see build output
```

## 📝 Contributing

To contribute improvements:

1. Test the script with different configurations
2. Report issues with specific error messages
3. Submit pull requests for enhancements

## 📄 License

This project includes components under various licenses:
- Build script: MIT License
- Seafile: MIT License (see LICENSE.txt)

## 🔗 Resources

- [Seafile Documentation](https://manual.seafile.com/)
- [Ubuntu Packaging Guide](https://packaging.ubuntu.com/)
- [Podman Documentation](https://podman.io/getting-started/)

---

**Last Updated**: August 2025  
**Tested On**: Ubuntu 24.04 LTS, Ubuntu 22.04 LTS  
**Seafile Version**: v9.0.14# seafile-debs
