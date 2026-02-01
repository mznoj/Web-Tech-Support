# Browser & Network Diagnostic Tool

A simple, plain-text browser diagnostic tool for troubleshooting web access issues.

## Features

- **Browser Information**: User agent, browser version, platform, OS, language settings
- **Screen & Display**: Resolution, viewport size, color depth, pixel ratio, orientation
- **Browser Capabilities**: JavaScript, cookies, storage, WebGL, service workers, and more
- **Network Information**: Online status, connection type, protocol, hostname, public IP
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

All diagnostic checks are performed locally in your browser. The only external request made is to `api.ipify.org` to retrieve your public IP address. No data is sent to any other servers.

## License

This tool is provided as-is for diagnostic and troubleshooting purposes.
