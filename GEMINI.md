# GEMINI.md

This file provides guidance to Gemini when working with code in this repository.

## Project Overview

This is a single-file browser diagnostic tool (`index.html`) that displays browser, network, and system information in plain text format. The tool is designed for tech support and troubleshooting web access issues.

## Architecture

### Single-File Structure

The entire application is contained in `index.html` with three main sections:

1. **Minimal CSS Styling** (lines 8-25)
   - Monospace font for all content
   - Basic button styling
   - Print media query to hide buttons

2. **Plain Text HTML Structure** (lines 27-41)
   - `<pre id="header">` - Displays report title and timestamp
   - Two buttons: Copy All Data and Refresh
   - `<pre id="diagnosticContent">` - Main diagnostic output area

3. **JavaScript Diagnostic Engine** (lines 43-520)
   - Data collection functions (7 base + 4 DNS categories)
   - Plain text rendering system
   - Async IP address and DNS diagnostics fetching
   - Clipboard copy functionality

### Data Collection Functions

Each function returns an object with key-value pairs of diagnostic information:

**Base Diagnostics (Synchronous):**
- `getBrowserInfo()` - User agent, browser detection, platform, languages, cookies
- `getScreenInfo()` - Screen resolution, viewport, color depth, pixel ratio, orientation
- `getCapabilities()` - Feature detection: storage APIs, WebGL, service workers, WebAssembly
- `getNetworkInfo()` - Online status, connection type, protocol, hostname
- `getPerformanceInfo()` - Page load metrics, DNS, TCP, TLS, TTFB, DOM load times
- `getSecurityInfo()` - Protocol, secure context, cross-origin isolation, referrer policy
- `getAdditionalDetails()` - URL, referrer, document mode, timezone, API support

**Async Diagnostics:**
- `getIPAddress()` - Fetches public IP from api.ipify.org
- `getDNSResolverInfo()` - Tests connectivity to Cloudflare DNS resolvers (1.1.1.1, 1.0.0.1, IPv6)
- `testDNSPerformance()` - Measures DoH query timing and validates functionality

**DNS Helper Functions:**
- `parseCloudflareTrace()` - Extracts data center, TLS, HTTP version from trace data
- `detectDNSEncryption()` - Infers DoH/DoT/WARP status from connectivity and trace data

### Plain Text Rendering System

**Key Functions:**

- `formatValue(value)` - Converts values to plain text ("Enabled", "Disabled", "Not Available")
- `createInfoSection(title, data)` - Generates plain text section with dashes under title
- `updateTimestamp()` - Creates header with title, separator line (60 equals signs), and timestamp
- `renderAllInfo()` - Orchestrates data collection, rendering, and async IP update

**Format Convention:**
```
Section Title
-------------
Key: Value
Key: Value
```

## Making Changes

### Preserving Plain Text Format

When modifying the tool, maintain the plain text output format:

- Use `<pre>` tags to preserve whitespace and newlines
- Use `.textContent` (not `.innerHTML`) when updating diagnostic content
- Keep `formatValue()` returning plain strings (no HTML)
- Maintain section formatting with dashes matching title length

### Adding New Diagnostic Sections

1. Create a new data collection function returning an object
2. Add to `diagnosticData` object in `renderAllInfo()` (lines 435-443 for sync, lines 459-466 for async)
3. The rendering system will automatically format it as plain text

### Modifying Display Format

- Header format: `updateTimestamp()` (lines 412-419)
- Section format: `createInfoSection()` (lines 421-430)
- Value formatting: `formatValue()` (lines 47-58)

### Async Data Updates

Multiple async diagnostics run in parallel using `Promise.all()`:
1. Render initial synchronous data without async values
2. Fetch all async data in parallel (IP + DNS tests) using Promise.all()
3. Update `diagnosticData` object with async results
4. Add new DNS diagnostic sections to `diagnosticData`
5. Re-render all sections with complete data

The pattern uses 5-second timeouts with AbortController to prevent long waits on blocked connections.

## Testing

Open `index.html` directly in a browser (no build step required).

**Test checklist:**
- All 11 diagnostic sections display correctly (7 base + 4 DNS)
- Plain text formatting preserved (no HTML artifacts)
- Copy button copies same format as displayed
- Refresh button updates all data including timestamp
- IP address and DNS diagnostics load asynchronously and update display
- DNS sections show connectivity status, encryption detection, network path, and performance
- Print preview hides buttons but shows all diagnostic text
- Timeouts work correctly on slow/blocked connections

## External Dependencies

- **api.ipify.org** - Free IP address lookup API (read-only, no authentication)
- **1.1.1.1/cdn-cgi/trace** - Cloudflare trace API for DNS resolver connectivity testing
- **1.0.0.1/cdn-cgi/trace** - Cloudflare secondary resolver trace endpoint
- **[2606:4700:4700::1111]/cdn-cgi/trace** - Cloudflare IPv6 resolver (primary)
- **[2606:4700:4700::1001]/cdn-cgi/trace** - Cloudflare IPv6 resolver (secondary)
- **cloudflare-dns.com/dns-query** - Cloudflare DoH JSON API for performance testing
- No build tools, frameworks, or npm packages required

## Privacy & Security

- All diagnostics run client-side in the browser
- External requests:
  - GET to api.ipify.org for public IP address
  - GET to 1.1.1.1/1.0.0.1/IPv6 endpoints for DNS resolver connectivity tests
  - GET to cloudflare-dns.com for DoH performance test (queries cloudflare.com)
- No personal data or browsing history is transmitted
- No cookies or tracking
- Safe to share output (contains no secrets, only browser/network metadata)
- All DNS tests use 5-second timeouts to prevent long waits
