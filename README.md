# Sol Browser

**A privacy-focused Chromium-based browser by Computer Motivators**

Sol Browser is an open-source web browser built on Chromium, designed with privacy and integration with Sol AI as core principles. This repository contains both the Chromium build system and browser-specific features.

## Repository Structure

This is a monorepo with two main components:

### 1. Browser (`packages/browseros`)
The Chromium build system containing:
- **Build Scripts** - Python-based build automation (`build/`)
- **Chromium Patches** - Modifications to Chromium source (`chromium_patches/`)
- **Resources** - Icons, extensions, and assets (`resources/`)
- **Configuration** - Build configurations for different platforms (`build/config/`)

### 2. Agent Extension (Coming Soon)
AI-powered browser extension with Sol integration (planned for `packages/solbrowser-agent/`)

## Technical Overview

### Core Architecture

Sol Browser is built by:
1. Fetching Chromium source code (see [Chromium Version](packages/browseros/CHROMIUM_VERSION))
2. Applying custom patches for branding and features
3. Building with platform-specific configurations
4. Packaging for distribution (AppImage, .deb, .dmg, .exe)

### Key Technologies

- **Language**: C++ (Chromium core), Python (build system), TypeScript (extensions)
- **Build System**: GN (Generate Ninja) + Ninja
- **Platforms**: Linux (x64, ARM64), macOS (x64, ARM64, Universal), Windows (x64)
- **Package Formats**: AppImage, Debian (.deb), DMG, Windows Installer

## Getting Started

### Prerequisites

**For Browser Development:**
- ~100GB disk space (Chromium source)
- 16GB+ RAM recommended
- Platform-specific tools:
  - Linux: `build-essential`, Python 3.8+
  - macOS: Xcode Command Line Tools
  - Windows: Visual Studio Build Tools

### Quick Start - Building the Browser

1. **Clone the repository**
   ```bash
   git clone https://github.com/Computer-Motivators/Sol-Browser.git
   cd Sol-Browser
   ```

2. **Fetch Chromium source** (one-time setup)
   
   Follow [Chromium's official guide](https://www.chromium.org/developers/how-tos/get-the-code/) for your platform. This downloads ~100GB and takes 2-3 hours.

3. **Build Sol Browser**
   ```bash
   cd packages/browseros
   
   # Linux
   python build/build.py \
     --config build/config/release.linux.yaml \
     --chromium-src /path/to/chromium/src \
     --build
   
   # macOS
   python build/build.py \
     --config build/config/release.macos.yaml \
     --chromium-src /path/to/chromium/src \
     --build
   
   # Windows
   python build/build.py \
     --config build/config/release.windows.yaml \
     --chromium-src /path/to/chromium/src \
     --build
   ```

4. **Run the browser**
   
   The built browser will be in `chromium/src/out/Default_<arch>/`

For detailed build instructions, see [docs/BUILD.md](docs/BUILD.md).

## Key Features

### Privacy-First Design
- No Google tracking or analytics
- Local-first data storage
- User-controlled AI integration

### Sol AI Integration
- Default search: https://computermotivators.com/app/sol
- New tab page: https://computermotivators.com/app/sol
- Sol-powered browsing assistance

### URL Schema
Uses `sol://` instead of `chrome://` for internal pages:
- `sol://settings`
- `sol://extensions`
- `sol://version`

## Building Packages

### Linux
```bash
python build/build.py \
  --config build/config/release.linux.yaml \
  --chromium-src /path/to/chromium/src \
  --build --package
```

Creates:
- `SolBrowser.AppImage` - Portable AppImage
- `solbrowser_<version>_amd64.deb` - Debian package

### macOS
```bash
python build/build.py \
  --config build/config/release.macos.yaml \
  --chromium-src /path/to/chromium/src \
  --build --package
```

Creates:
- `Sol Browser_<version>_<arch>.dmg` - DMG installer

### Windows
```bash
python build/build.py \
  --config build/config/release.windows.yaml \
  --chromium-src /path/to/chromium/src \
  --build --package
```

Creates:
- `SolBrowser_installer.exe` - Windows installer

## Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for:
- Development setup
- Code standards
- Contribution workflow
- Signing the CLA

### Quick Contribution Guide

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a Pull Request

## Repository Information

- **Owner**: Computer Motivators
- **Repository**: https://github.com/Computer-Motivators/Sol-Browser
- **License**: AGPL-3.0 (see [LICENSE](LICENSE))
- **Issues**: https://github.com/Computer-Motivators/Sol-Browser/issues

## Version Information

- **Chromium Version**: See [CHROMIUM_VERSION](packages/browseros/CHROMIUM_VERSION)
- **Sol Browser Version**: See [build/config/NXTSCAPE_VERSION](packages/browseros/build/config/NXTSCAPE_VERSION)

## System Requirements

### Minimum
- **OS**: Linux (Ubuntu 20.04+), macOS 11+, Windows 10+
- **RAM**: 4GB
- **Disk**: 500MB

### Recommended
- **RAM**: 8GB+
- **Disk**: 1GB+

## Security

For security-related information, see [SECURITY.md](SECURITY.md).

To report security vulnerabilities, email: security@computermotivators.com

## Support

- **Documentation**: https://github.com/Computer-Motivators/Sol-Browser/tree/main/docs
- **Issues**: https://github.com/Computer-Motivators/Sol-Browser/issues
- **Email**: support@computermotivators.com

## Build Status

Builds are tested on:
- Ubuntu 22.04 LTS (x64, ARM64)
- macOS 13+ (x64, ARM64)  
- Windows 11 (x64)

## Acknowledgments

Sol Browser is built on top of:
- **Chromium** - The open-source browser project
- **depot_tools** - Google's build tools

Special thanks to the open-source community for making this possible.

---

**Built with privacy in mind by Computer Motivators**
