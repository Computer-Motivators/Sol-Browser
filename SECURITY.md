# Security Policy

## Sol Browser Security and Privacy

Sol Browser is built with privacy and security as fundamental principles. This document outlines the security measures implemented, privacy guarantees, and our commitment to protecting user data.

## Table of Contents

1. [Privacy Guarantees](#privacy-guarantees)
2. [Tracking and Analytics](#tracking-and-analytics)
3. [Data Collection](#data-collection)
4. [Security Features](#security-features)
5. [Chromium Security](#chromium-security)
6. [Reporting Security Vulnerabilities](#reporting-security-vulnerabilities)
7. [Security Audits](#security-audits)

## Privacy Guarantees

### Core Principles

Sol Browser is designed with the following privacy principles:

1. **Local-First**: All browsing data stays on your device by default
2. **No Tracking**: No usage analytics or telemetry sent to Computer Motivators
3. **User Control**: You decide what data, if any, is shared
4. **Transparent**: Open-source codebase allows full inspection

### What We DON'T Collect

- Browsing history
- Search queries
- Form data or autofill information
- Extension usage
- Tab data
- Bookmarks
- Download history
- Cookies or site data
- User behavior analytics
- Crash reports (unless explicitly enabled by user)
- Diagnostic data
- Location data

### What We DO Collect

**By default: NOTHING**

Optional data collection (must be explicitly enabled by user):
- **Crash Reports**: If crash reporting is enabled, stack traces may be sent to help debug issues
- **Update Checks**: When checking for updates, your version number and OS are sent to verify latest version

## Tracking and Analytics

### Removed Tracking

The following tracking and analytics systems have been REMOVED from Sol Browser:

#### From Chromium:
- ✅ Google Analytics
- ✅ Google Safe Browsing telemetry
- ✅ Spelling correction API calls
- ✅ Navigation suggestions
- ✅ RLZ tracking
- ✅ Google API calls for:
  - Omnibox suggestions (replaced with local-only)
  - Translation services (disabled)
  - Speech recognition (disabled)
  - Cloud print (removed)
- ✅ Default search engine reporting
- ✅ Usage statistics reporting
- ✅ WebRTC leak prevention (enhanced)

#### From BrowserOS (upstream):
- ✅ BrowserOS-specific analytics
- ✅ Feature usage tracking
- ✅ Extension telemetry
- ✅ Feedback extension (removed entirely)

### Network Connections

Sol Browser makes network connections ONLY when:

1. **You navigate to a website** - Standard HTTP/HTTPS requests
2. **You use Sol AI** - Connections to https://computermotivators.com/app/sol
3. **You check for updates** - Optional, can be disabled
4. **You use extensions** - Per extension permissions

### No Home Phone

Sol Browser does NOT "phone home" to:
- Report usage statistics
- Send crash data (unless opted-in)
- Sync browsing data (no sync feature)
- Check spelling (local dictionary only)
- Validate certificates (uses system trust store)

## Data Collection

### Local Storage

All user data is stored locally on your device:

- **Profile Data**: `~/.config/solbrowser/` (Linux), `~/Library/Application Support/Sol Browser/` (macOS), `%APPDATA%\Sol Browser\` (Windows)
- **Cache**: Temporary files stored locally
- **Cookies**: Managed per-site, stored locally
- **Extensions**: Installed locally

### Sol AI Integration

When using Sol AI features:
- **New Tab Page**: Loads https://computermotivators.com/app/sol
- **Search**: Queries sent to https://computermotivators.com/app/sol?q=<query>
- **Data**: Subject to Computer Motivators privacy policy
- **Control**: Can be disabled in settings

### Search Providers

Default search provider:
- **Sol Search**: https://computermotivators.com/app/sol

Optional search providers available:
- **SearXNG**: https://search.computermotivators.com (privacy-focused metasearch)

All other search providers have been removed to prevent data leakage.

## Security Features

### Sandboxing

- **Process Isolation**: Each tab runs in a separate sandboxed process
- **Site Isolation**: Different origins run in different processes
- **Sandbox Strength**: Chromium's robust sandbox (SUID on Linux for privilege separation)

### HTTPS Enforcement

- **HTTPS-First**: Automatically upgrade to HTTPS where available
- **Certificate Validation**: Strict certificate checking
- **HSTS**: HTTP Strict Transport Security support
- **Certificate Pinning**: For critical domains

### Content Security

- **XSS Protection**: Cross-site scripting prevention
- **CSP**: Content Security Policy enforcement
- **Same-Origin Policy**: Strict origin isolation
- **CORS**: Proper cross-origin resource sharing

### Extension Security

- **Manifest V3**: Modern extension API with better security
- **Permission Model**: Granular extension permissions
- **Component Extensions**: Built-in extensions are code-signed
- **Extension Review**: Manual review of extension code

### URL Schema Protection

- **sol://** - Internal pages use `sol://` instead of `chrome://`
- **Isolated Context**: Internal pages run in privileged context
- **No External Access**: External sites cannot access `sol://` pages

## Chromium Security

Sol Browser inherits Chromium's strong security model:

### Upstream Security

- **Regular Updates**: Based on stable Chromium releases
- **Security Patches**: Applied promptly from Chromium security team
- **V8 Security**: JavaScript engine with JIT hardening
- **Memory Safety**: Use-after-free protections, ASLR, DEP

### Enhanced Security

Sol Browser adds additional security measures:
- **Reduced Attack Surface**: Removed unnecessary features
- **No Cloud Services**: No cloud sync reduces remote attack vectors
- **Local-Only**: Reduced network exposure

### Security Update Policy

- **Critical**: Applied within 48 hours of Chromium release
- **High**: Applied within 1 week
- **Medium/Low**: Applied with next minor release

## Reporting Security Vulnerabilities

### Responsible Disclosure

We take security seriously. If you discover a security vulnerability:

**DO:**
- ✅ Email security@computermotivators.com with details
- ✅ Provide step-by-step reproduction instructions
- ✅ Allow us reasonable time to fix (90 days recommended)
- ✅ Disclose responsibly

**DON'T:**
- ❌ Publicly disclose before we've had time to fix
- ❌ Access user data without permission
- ❌ Perform destructive testing

### What to Include

When reporting vulnerabilities, please provide:

1. **Description**: Clear description of the issue
2. **Impact**: Potential security impact
3. **Reproduction**: Step-by-step instructions to reproduce
4. **Proof of Concept**: Code, screenshots, or video
5. **Environment**: OS, Sol Browser version, architecture
6. **Suggested Fix**: If you have ideas (optional)

### Response Timeline

- **Acknowledgment**: Within 48 hours
- **Triage**: Within 1 week
- **Fix**: Depends on severity (critical: immediate, high: 1-2 weeks, medium: 2-4 weeks)
- **Disclosure**: After fix is released and users have time to update (typically 30 days)

### Hall of Fame

Security researchers who responsibly disclose vulnerabilities will be credited (with permission) in:
- Release notes
- This SECURITY.md file
- Our website

## Security Audits

### Current Status

**Last Full Audit**: Pending (Sol Browser is new)
**Chromium Base**: Regularly audited by Google and security community
**Next Planned Audit**: Q2 2025

### Areas Audited

Future audits will cover:
- Build system security
- Patch security
- Extension security
- Privacy features
- Network isolation
- Data storage security

### Audit Results

Audit results will be published here after completion.

## Security Best Practices for Users

### Recommended Settings

1. **Keep Updated**: Install updates promptly
2. **Extensions**: Only install necessary extensions
3. **HTTPS**: Enable HTTPS-First mode (on by default)
4. **Passwords**: Use strong, unique passwords
5. **Privacy**: Review privacy settings regularly

### Advanced Security

For advanced users:
- **DNS over HTTPS**: Enable DoH in settings
- **Disable JavaScript**: For high-security browsing (may break sites)
- **Clear Data**: Regularly clear cache and cookies
- **Incognito Mode**: Use for sensitive browsing

## Compliance

### Standards

Sol Browser aims to comply with:
- **GDPR**: General Data Protection Regulation (EU)
- **CCPA**: California Consumer Privacy Act (US)
- **PIPEDA**: Personal Information Protection Act (Canada)

### Certifications

- Chromium base is certified for various security standards
- Sol Browser inherits these certifications
- Additional certifications planned for 2025

## Changes to This Policy

This security policy may be updated periodically. Material changes will be announced via:
- GitHub release notes
- Sol Browser update notifications

**Last Updated**: October 29, 2025
**Version**: 1.0

## Contact

- **Security Issues**: security@computermotivators.com
- **General Support**: support@computermotivators.com
- **Website**: https://computermotivators.com

---

**Security is a journey, not a destination. We're committed to continually improving Sol Browser's security and privacy.**

Built with privacy in mind by Computer Motivators
