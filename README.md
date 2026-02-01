# Browser & Network Diagnostic Tool

A simple, plain-text browser diagnostic tool for troubleshooting web access issues.

## Features

- **Browser Information**: User agent, browser version, platform, OS, language settings
- **Screen & Display**: Resolution, viewport size, color depth, pixel ratio, orientation
- **Browser Capabilities**: JavaScript, cookies, storage, WebGL, service workers, and more
- **Network Information**: Online status, connection type, protocol, hostname, public IP
- **DNS Resolver Connectivity**: Tests connectivity to Cloudflare DNS resolvers (1.1.1.1, IPv4/IPv6)
- **DNS Protocol & Encryption**: Detects DNS-over-HTTPS (DoH), DNS-over-WARP status
- **DNS Network Path**: Data center location, TLS version, HTTP version, WARP/Gateway status
- **DNS Performance**: DoH query timing and response validation
- **Performance Metrics**: Page load time, DNS lookup, TCP connection, TTFB, DOM load
- **Security Context**: Protocol, secure context status, referrer policy
- **Additional Details**: Current URL, referrer, timezone, encoding

## Usage

Simply open `index.html` in any web browser. The diagnostic information will be displayed automatically in plain text format.

### Buttons

- **Copy All Data**: Copies all diagnostic information to clipboard in plain text format
- **Refresh**: Reloads all diagnostic information with current values

## Format

The tool displays all information in a simple, plain-text format:

```
BROWSER & NETWORK DIAGNOSTIC REPORT
============================================================
Generated: [timestamp]

Browser Information
-------------------
User Agent: [value]
Browser: [value]
Platform: [value]
...
```

## Use Cases

- Troubleshooting browser compatibility issues
- Gathering system information for tech support
- Checking browser capabilities and features
- Network diagnostics for connectivity issues
- Performance analysis

## Privacy

All diagnostic checks are performed locally in your browser. External requests are made to:
- `api.ipify.org` - To retrieve your public IP address
- `1.1.1.1` and `1.0.0.1` (Cloudflare DNS) - To test DNS resolver connectivity
- `cloudflare-dns.com` - To test DNS-over-HTTPS (DoH) performance

All tests are read-only connectivity checks. No personal data or browsing history is sent to any servers.

## License

This tool is provided as-is for diagnostic and troubleshooting purposes.
