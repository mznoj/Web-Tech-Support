# Changelog

All notable changes to the Browser & Network Diagnostic Tool will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.1] - 2026-02-01

### Added
- **Content Security Policy (CSP)**: Added comprehensive CSP headers to restrict resource loading and enhance security
- **Configuration Constants**: Centralized configuration in CONFIG object for easier maintenance
  - TOOL_VERSION, IP_TIMEOUT, DNS_TIMEOUT, DNS_LEAK_TIMEOUT, RESOLVER_TIMEOUT, WEBSOCKET_TIMEOUT
- **WebSocket Connectivity Test**: Tests WebSocket support and connection to public echo server with timeout handling

### Enhanced
- **Improved Error Messages**: All error messages now provide actionable guidance
  - DNS timeouts suggest firewall or network issues
  - Failed connections provide specific troubleshooting hints
  - IP address errors suggest checking firewall or refreshing
- **Parallelized DNS Leak Detection**: Changed from sequential to parallel execution using Promise.all()
  - Reduced DNS leak testing time from ~15 seconds to ~5 seconds
  - All three resolver tests now run simultaneously

### Fixed
- Error messages now reference dynamic CONFIG timeout values instead of hardcoded strings
- All timeout values now use CONFIG constants for consistency

### Technical
- 13 diagnostic sections (7 base + 5 DNS + 1 connectivity test)
- ~870 lines of code (single HTML file)
- Faster DNS leak detection (3x speedup)
- Enhanced security with CSP headers
- Removed client-side unsupported checks (proxy, CDN, certificate details)

## [1.0.0] - 2026-02-02

### Added
- **Version System**: Added `TOOL_VERSION` constant for version tracking
- **Version Display**: Version shown in page title, report header, and diagnostic data
- **Export Features**: JSON and CSV export functionality with timestamped filenames
- **DNS Diagnostics Suite**:
  - DNS resolver connectivity testing (Cloudflare IPv4/IPv6)
  - DNS encryption detection (DoH/DoT/WARP)
  - DNS performance benchmarking (Cloudflare, Google, AdGuard)
  - DNS leak detection with multi-resolver testing
  - Cloudflare trace parsing for network path information
- **IP Address Fallback**: Dual-source IP detection (ipify.org with Cloudflare trace fallback)
- **Modern API Support**:
  - User-Agent Client Hints API for browser detection
  - Navigation Timing Level 2 API for performance metrics
- **GitHub Actions CI/CD**: Automatic deployment to Cloudflare Pages on push to main
- **Custom Domain**: web-tech-support.mattz.cc with automatic SSL

### Enhanced
- Browser detection now uses modern User-Agent Client Hints when available
- Performance metrics use Navigation Timing Level 2 API with Level 1 fallback
- IP address detection now has 3-second timeout with automatic fallback
- All DNS tests use 5-second timeouts to prevent hanging on blocked connections
- Security context detection includes mixed content checking

### Fixed
- IP address detection reliability improved with Cloudflare trace fallback
- Browser detection more accurate with User-Agent Client Hints
- Performance timing more precise with modern Navigation Timing API

### Technical
- 12 diagnostic sections (7 base + 5 DNS-related)
- ~810 lines of code (single HTML file)
- No build process required
- Pure client-side JavaScript
- Automatic deployment via GitHub Actions

## [0.x] - Initial Development

### Initial Features (Pre-1.0)
- Basic browser information detection
- Screen and display information
- Browser capabilities testing
- Network information
- Performance metrics
- Security context information
- Plain text formatting system
- Copy to clipboard functionality
- Refresh functionality
- Print-friendly styling

---

## Version Numbering

This project uses [Semantic Versioning](https://semver.org/):
- **MAJOR** version: Incompatible API changes or major feature overhauls
- **MINOR** version: New features in a backwards-compatible manner
- **PATCH** version: Backwards-compatible bug fixes

### Updating Version

To update the version:
1. Change `TOOL_VERSION` constant in `index.html` (line 46)
2. Update page title in `index.html` (line 7)
3. Update this CHANGELOG.md with changes
4. Commit and push - GitHub Actions will auto-deploy

---

## Links

- **Live Site**: https://web-tech-support.mattz.cc
- **GitHub Repository**: https://github.com/mznoj/Web-Tech-Support
- **Cloudflare Pages**: https://web-tech-support.pages.dev
