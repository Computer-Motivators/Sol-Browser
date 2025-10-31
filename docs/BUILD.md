# Building Sol Browser

This guide provides detailed instructions for building Sol Browser from source, including all dependencies, environment setup, and troubleshooting.

## Table of Contents

1. [Overview](#overview)
2. [System Requirements](#system-requirements)
3. [Environment Setup](#environment-setup)
4. [Fetching Chromium Source](#fetching-chromium-source)
5. [Building Sol Browser](#building-sol-browser)
6. [Creating Distribution Packages](#creating-distribution-packages)
7. [Troubleshooting](#troubleshooting)
8. [Advanced Build Options](#advanced-build-options)

## Overview

Building Sol Browser involves:

1. **Environment Setup**: Installing required tools and dependencies
2. **Fetching Chromium**: Downloading ~100GB of Chromium source code
3. **Building**: Compiling Chromium with Sol Browser patches
4. **Packaging**: Creating platform-specific installers

**Time Estimate:**
- First build: 3-6 hours (mostly compilation)
- Incremental builds: 10-30 minutes

**Disk Space:**
- Chromium source: ~100GB
- Build output: ~50GB
- Total recommended: 200GB+ free space

## System Requirements

### Linux

**Minimum:**
- Ubuntu 20.04 LTS or equivalent
- 16GB RAM
- 150GB free disk space
- 4+ CPU cores

**Recommended:**
- Ubuntu 22.04 LTS
- 32GB RAM
- 250GB SSD
- 8+ CPU cores
- Fast internet connection

**Required Packages:**
```bash
sudo apt-get update
sudo apt-get install -y \
  build-essential \
  git \
  python3 \
  python3-pip \
  curl \
  wget \
  pkg-config \
  libglib2.0-dev \
  libgtk-3-dev \
  libnspr4-dev \
  libnss3-dev \
  libxtst-dev \
  libxss-dev \
  libasound2-dev \
  libcups2-dev \
  libdbus-1-dev \
  libgconf-2-4 \
  libpci-dev \
  libpulse-dev \
  libudev-dev \
  libdrm-dev \
  libgbm-dev
```

### macOS

**Minimum:**
- macOS 11 (Big Sur) or later
- 16GB RAM
- 150GB free disk space
- Xcode 13+

**Recommended:**
- macOS 13 (Ventura) or later
- 32GB RAM
- 250GB SSD
- Xcode 15+
- M-series Mac (ARM64) or Intel Mac

**Required Tools:**
```bash
# Install Xcode Command Line Tools
xcode-select --install

# Install Homebrew (if not already installed)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install required packages
brew install python@3.11 git
```

### Windows

**Minimum:**
- Windows 10 (64-bit)
- 16GB RAM
- 150GB free disk space
- Visual Studio 2022

**Recommended:**
- Windows 11 (64-bit)
- 32GB RAM
- 250GB SSD
- Visual Studio 2022 Community or Pro

**Required Components:**
- Visual Studio 2022 with:
  - Desktop development with C++
  - Windows 10 SDK (10.0.20348.0 or later)
  - Debugging Tools for Windows
- Python 3.11+
- Git for Windows

## Environment Setup

### 1. Install depot_tools

Chromium uses Google's `depot_tools` for building:

**Linux/macOS:**
```bash
# Clone depot_tools
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
cd depot_tools

# Add to PATH (add to ~/.bashrc or ~/.zshrc for persistence)
export PATH="$PATH:$(pwd)"
```

**Windows:**
```cmd
# Clone depot_tools
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
cd depot_tools

# Add to PATH permanently via System Properties > Environment Variables
# Or temporarily:
set PATH=%PATH%;%CD%
```

### 2. Configure Git

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global core.autocrlf false
git config --global core.filemode false
```

### 3. Clone Sol Browser Repository

```bash
git clone https://github.com/Computer-Motivators/Sol-Browser.git
cd Sol-Browser
```

## Fetching Chromium Source

This is a one-time setup that downloads ~100GB:

### Create Chromium Directory

```bash
# Create a directory OUTSIDE the Sol Browser repo
mkdir ~/chromium
cd ~/chromium
```

### Fetch Chromium

**Method 1: Automatic (Recommended)**

```bash
# This fetches the Chromium version matching Sol Browser
fetch --nohooks chromium

# Sync to the correct version (check Sol-Browser/packages/browseros/CHROMIUM_VERSION)
cd src
git checkout <version-from-CHROMIUM_VERSION-file>
gclient sync
```

**Method 2: Manual**

Follow the official guide for your platform:
- [Linux](https://chromium.googlesource.com/chromium/src/+/main/docs/linux/build_instructions.md)
- [macOS](https://chromium.googlesource.com/chromium/src/+/main/docs/mac_build_instructions.md)
- [Windows](https://chromium.googlesource.com/chromium/src/+/main/docs/windows_build_instructions.md)

### Install Build Dependencies

**Linux:**
```bash
cd ~/chromium/src
./build/install-build-deps.sh
```

**macOS:**
```bash
# Dependencies installed via Xcode and Homebrew
# No additional steps needed
```

**Windows:**
```powershell
# Visual Studio 2022 should have installed everything
# If not, run:
python3 build/install-build-deps.py
```

## Building Sol Browser

### 1. Navigate to Build Directory

```bash
cd /path/to/Sol-Browser/packages/browseros
```

### 2. Choose Build Configuration

Available configurations:
- `build/config/debug.yaml` - Development build (faster, larger, includes debug symbols)
- `build/config/release.linux.yaml` - Linux production build
- `build/config/release.macos.yaml` - macOS production build
- `build/config/release.windows.yaml` - Windows production build

### 3. Run Build

**Linux (Release):**
```bash
python build/build.py \
  --config build/config/release.linux.yaml \
  --chromium-src ~/chromium/src \
  --build
```

**macOS (Release):**
```bash
python build/build.py \
  --config build/config/release.macos.yaml \
  --chromium-src ~/chromium/src \
  --build
```

**Windows (Release):**
```cmd
python build/build.py ^
  --config build/config/release.windows.yaml ^
  --chromium-src C:\chromium\src ^
  --build
```

**Debug Build (Any Platform):**
```bash
python build/build.py \
  --config build/config/debug.yaml \
  --chromium-src /path/to/chromium/src \
  --build
```

### 4. Build Process

The build process will:

1. **Clean** (if first build): Remove previous build artifacts
2. **Apply Patches**: Apply Sol Browser patches to Chromium
3. **Copy Resources**: Copy icons, extensions, and assets
4. **Configure**: Generate build configuration (GN)
5. **Compile**: Build Chromium (1-3 hours on first build)
6. **Post-build**: Apply final branding and configuration

### 5. Locate Built Browser

After successful build:

**Linux:**
```
~/chromium/src/out/Default_x64/solbrowser
```

**macOS:**
```
~/chromium/src/out/Default_arm64/Sol Browser.app
# or
~/chromium/src/out/Default_x64/Sol Browser.app
```

**Windows:**
```
C:\chromium\src\out\Default_x64\SolBrowser.exe
```

### 6. Run the Browser

**Linux:**
```bash
~/chromium/src/out/Default_x64/solbrowser
```

**macOS:**
```bash
open ~/chromium/src/out/Default_arm64/Sol\ Browser.app
```

**Windows:**
```cmd
C:\chromium\src\out\Default_x64\SolBrowser.exe
```

## Creating Distribution Packages

### Linux (AppImage and .deb)

```bash
python build/build.py \
  --config build/config/release.linux.yaml \
  --chromium-src ~/chromium/src \
  --build --package
```

Output:
- `dist/<version>/SolBrowser.AppImage`
- `dist/<version>/solbrowser_<version>_amd64.deb`

### macOS (DMG)

```bash
python build/build.py \
  --config build/config/release.macos.yaml \
  --chromium-src ~/chromium/src \
  --build --package
```

Output:
- `dist/<version>/Sol Browser_<version>_arm64.dmg` (M-series Mac)
- `dist/<version>/Sol Browser_<version>_x64.dmg` (Intel Mac)

**Universal Binary (Intel + ARM64):**
```bash
python build/build.py \
  --config build/config/release.macos.yaml \
  --chromium-src ~/chromium/src \
  --build --package --universal
```

### Windows (Installer)

```cmd
python build/build.py ^
  --config build/config/release.windows.yaml ^
  --chromium-src C:\chromium\src ^
  --build --package
```

Output:
- `dist\<version>\SolBrowser_installer.exe`

## Troubleshooting

### Common Issues

#### 1. Out of Disk Space

**Symptom:** Build fails with "No space left on device"

**Solution:**
```bash
# Check disk space
df -h

# Clean build artifacts
cd ~/chromium/src
gn clean out/Default_x64

# Remove cache (if needed)
rm -rf ~/.cache/chromium
```

#### 2. Depot Tools Not in PATH

**Symptom:** `gclient` or `gn` command not found

**Solution:**
```bash
# Add depot_tools to PATH
export PATH="$PATH:/path/to/depot_tools"

# Make permanent (Linux/macOS)
echo 'export PATH="$PATH:/path/to/depot_tools"' >> ~/.bashrc
source ~/.bashrc
```

#### 3. Python Version Mismatch

**Symptom:** Build fails with Python errors

**Solution:**
```bash
# Ensure Python 3.8+ is installed
python3 --version

# Linux: Update Python
sudo apt-get install python3.11

# macOS: Use Homebrew
brew install python@3.11

# Update depot_tools to use correct Python
export VPYTHON_BYPASS=manually_managed_python_not_supported_on_swarming
```

#### 4. Chromium Sync Fails

**Symptom:** `gclient sync` fails

**Solution:**
```bash
# Try syncing with force
gclient sync --force

# If still fails, clean and retry
cd ~/chromium
rm -rf src
fetch --nohooks chromium
cd src
gclient sync
```

#### 5. Build Fails During Compilation

**Symptom:** Ninja build fails

**Solution:**
```bash
# Check available RAM
free -h  # Linux
vm_stat  # macOS

# Reduce parallel jobs if low on RAM
gn args out/Default_x64
# Add or modify: use_jumbo_build = true
# Add: concurrent_links = 1  # Reduces RAM usage

# Retry build
ninja -C out/Default_x64 chrome
```

#### 6. macOS: Code Signing Errors

**Symptom:** "Code signing failed"

**Solution:**
```bash
# For development, disable code signing
gn args out/Default_arm64
# Add: is_component_build = true

# Or create a self-signed certificate in Keychain Access
```

#### 7. Windows: Visual Studio Not Found

**Symptom:** "Visual Studio installation not found"

**Solution:**
```cmd
# Set environment variable
set vs2022_install="C:\Program Files\Microsoft Visual Studio\2022\Community"
set GYP_MSVS_VERSION=2022

# Or reinstall Visual Studio with C++ workload
```

### Build Performance

#### Speed Up Builds

1. **Use ccache (Linux):**
   ```bash
   sudo apt-get install ccache
   export PATH="/usr/lib/ccache:$PATH"
   ```

2. **Increase Parallel Jobs:**
   ```bash
   # In GN args
   gn args out/Default_x64
   # Add: use_remoteexec = false
   # ninja will auto-detect CPU cores
   ```

3. **Use Component Build (Debug only):**
   ```bash
   gn args out/Default_x64
   # Add: is_component_build = true
   # Faster linking, but larger binary
   ```

4. **Disable Unused Features:**
   ```bash
   gn args out/Default_x64
   # Add these to speed up builds:
   # enable_nacl = false
   # enable_swiftshader = false
   ```

## Advanced Build Options

### Custom GN Arguments

Edit GN arguments:
```bash
cd ~/chromium/src
gn args out/Default_x64
```

Common arguments:
```gn
# Release optimizations
is_official_build = true
is_debug = false

# Enable all codecs
proprietary_codecs = true
ffmpeg_branding = "Chrome"

# Component build (faster debug builds)
is_component_build = true

# Disable features
enable_nacl = false
enable_widevine = false

# Custom branding
chrome_pgo_phase = 0  # Disable PGO for faster builds
```

### Incremental Builds

After making changes:
```bash
# Only rebuild changed files
ninja -C ~/chromium/src/out/Default_x64 chrome
```

### Clean Builds

```bash
# Clean specific target
ninja -C ~/chromium/src/out/Default_x64 -t clean chrome

# Clean everything
rm -rf ~/chromium/src/out/Default_x64
```

## Build System Architecture

Sol Browser's build system consists of:

1. **build.py**: Main orchestrator
2. **context.py**: Build configuration
3. **modules/**: Modular build steps
   - `patches.py`: Apply Chromium patches
   - `resources.py`: Copy assets
   - `compile.py`: Run Chromium build
   - `package.py`: Create installers
   - `package_linux.py`: Linux packaging
   - `package_windows.py`: Windows packaging

## Environment Variables

Useful environment variables:

```bash
# Chromium
export CHROMIUM_BUILDTOOLS_PATH=/path/to/buildtools
export DEPOT_TOOLS_WIN_TOOLCHAIN=0  # Windows: Use local Visual Studio

# Sol Browser
export SOL_BROWSER_BUILD_TYPE=release
export SOL_BROWSER_ARCH=x64

# Python
export VPYTHON_BYPASS=manually_managed_python_not_supported_on_swarming
```

## Getting Help

If you encounter issues:

1. **Check Logs**: Build logs are in `build/logs/`
2. **Chromium Docs**: https://www.chromium.org/developers/
3. **GitHub Issues**: https://github.com/Computer-Motivators/Sol-Browser/issues
4. **Email Support**: support@computermotivators.com

## Next Steps

After building:

1. **Test the Browser**: Run locally and verify functionality
2. **Create Packages**: Use `--package` flag for distribution
3. **Contribute**: See [CONTRIBUTING.md](../CONTRIBUTING.md)

---

**Happy Building!**

Built by Computer Motivators
