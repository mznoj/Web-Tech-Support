# Browser & Network Diagnostic Tool

**Version:** 1.0.1
**Live Site:** https://web-tech-support.mattz.cc

A comprehensive browser and network diagnostic tool for troubleshooting web access issues and gathering system information. All diagnostics run client-side in the browser with no server-side processing.

## Features

### Browser & System Information
- Browser detection using modern User-Agent Client Hints API
- Screen and display information (resolution, viewport, color depth, pixel ratio)
- Hardware capabilities (CPU cores, device memory, touch support)
- Platform and OS detection
- Language and timezone information

### Network Diagnostics
- Public IP address detection with automatic fallback
- Connection type and online status monitoring
- Network Information API data
- Protocol and hostname information

### DNS Diagnostics Suite
- **DNS Resolver Connectivity**: Test connectivity to Cloudflare resolvers (IPv4/IPv6)
- **DNS Encryption Detection**: Detect DoH, DoT, and DNS-over-WARP
- **DNS Performance Benchmarking**: Compare Cloudflare, Google, and AdGuard DNS performance
- **DNS Leak Detection**: Multi-resolver testing to identify potential DNS leaks
- **Network Path Analysis**: Cloudflare trace parsing for data center location and TLS info

### Performance Metrics
- Page load timing using Navigation Timing Level 2 API
- DNS lookup, TCP connection, TLS negotiation times
- Time to First Byte (TTFB)
- DOM load and interactive timing

### Security Context
- HTTPS/TLS detection
- Secure context verification
- Cross-origin isolation status
- Mixed content detection
- Referrer policy

### Export Options
- **Copy All Data**: Copy complete report to clipboard
- **Export JSON**: Download data as timestamped JSON file
- **Export CSV**: Download data as CSV (Section, Key, Value format)
- **Print-Friendly**: Clean layout for printing

## Usage

Access the tool online or run locally - no installation required!

### Online
- **Primary**: https://web-tech-support.mattz.cc
- **Alternative**: https://web-tech-support.pages.dev

### Local
```bash
# No build process needed - just open in browser
open index.html

# Or use a local HTTP server
python3 -m http.server 8000
# Then visit http://localhost:8000
```

## Technology

- **Pure HTML/CSS/JavaScript** - Single file (~810 lines)
- **Zero dependencies** - No frameworks or libraries
- **Client-side only** - All processing in browser
- **Modern APIs**:
  - User-Agent Client Hints
  - Navigation Timing Level 2
  - Network Information API
- **Privacy-focused** - No tracking, cookies, or server-side data collection

## Deployment

Automatically deployed to Cloudflare Pages via GitHub Actions on every push to `main` branch.

**Current Status:** Production deployment active with automatic updates

## Documentation

- **[CLAUDE.md](CLAUDE.md)** - Complete technical documentation and code architecture
- **[DEPLOYMENT.md](DEPLOYMENT.md)** - Deployment guide and CI/CD configuration
- **[CHANGELOG.md](CHANGELOG.md)** - Version history and release notes
- **[SETUP-COMPLETE.md](SETUP-COMPLETE.md)** - Deployment setup completion status

## Privacy & Security

- All diagnostics run client-side in your browser
- External API calls are read-only:
  - `api.ipify.org` - IP address lookup (with Cloudflare trace fallback)
  - Cloudflare DNS resolvers - Connectivity and performance testing
  - Google DNS, AdGuard DNS - Performance benchmarking
  - Akamai/Google - DNS leak detection
- No personal data or browsing history transmitted
- No cookies, tracking, or analytics
- Safe to share diagnostic output (contains only browser/network metadata)

## Development

### Local Testing
No build process required! Simply open `index.html` in any modern browser.

### Updating Version

1. Update `TOOL_VERSION` constant in `index.html` (line 46)
2. Update page title in `index.html` (line 7)
3. Document changes in `CHANGELOG.md`
4. Commit and push to `main` - automatic deployment via GitHub Actions

### Project Structure

```
Web-Tech-Support/
├── index.html                    # Main application (single file)
├── .github/workflows/
│   └── deploy.yml                # GitHub Actions CI/CD
├── CLAUDE.md                     # Technical documentation
├── DEPLOYMENT.md                 # Deployment guide
├── CHANGELOG.md                  # Version history
├── README.md                     # This file
├── SETUP-COMPLETE.md             # Setup status
└── .cloudflare-pages.json        # Cloudflare config
```

## Use Cases

- Troubleshooting browser compatibility issues
- Gathering system information for tech support tickets
- Testing browser capabilities and feature support
- Network diagnostics for connectivity issues
- DNS configuration troubleshooting
- Performance analysis and optimization
- Security context verification

## Browser Support

Works in all modern browsers with JavaScript enabled:
- Chrome/Edge 90+
- Firefox 88+
- Safari 14+
- Opera 76+

Graceful degradation for older browsers (uses API fallbacks where needed).

## Contributing

This is a personal project, but suggestions and feedback are welcome via GitHub issues.

## Links

- **Live Tool**: https://web-tech-support.mattz.cc
- **GitHub Repository**: https://github.com/mznoj/Web-Tech-Support
- **Cloudflare Pages**: https://web-tech-support.pages.dev
- **GitHub Actions**: [View Deployments](https://github.com/mznoj/Web-Tech-Support/actions)

## License

This tool is provided as-is for diagnostic and troubleshooting purposes.

---

**Version 1.0.0** | Built with ❤️ | Deployed on Cloudflare Pages
