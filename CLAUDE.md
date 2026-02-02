# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-file browser diagnostic tool (`index.html`) that displays browser, network, and system information in plain text format. The tool is designed for tech support and troubleshooting web access issues.

**Current Version:** 1.0.0
**Live URLs:**
- Production: https://web-tech-support.pages.dev
- Custom Domain: https://web-tech-support.mattz.cc

**Deployment:** Automatic via GitHub Actions on every push to `main`

## Architecture

### Single-File Structure

The entire application is contained in `index.html` with three main sections:

1. **Minimal CSS Styling** (lines 8-25)
   - Monospace font for all content
   - Basic button styling
   - Print media query to hide buttons

2. **Plain Text HTML Structure** (lines 27-43)
   - `<pre id="header">` - Displays report title and timestamp
   - Four buttons: Copy All Data, Refresh, Export JSON, Export CSV
   - `<pre id="diagnosticContent">` - Main diagnostic output area
   - `<noscript>` warning for JavaScript-disabled browsers

3. **JavaScript Diagnostic Engine** (lines 45-810)
   - Version constant: `TOOL_VERSION` (line 46)
   - Data collection functions (7 base + 5 DNS categories)
   - Plain text rendering system
   - Async IP address and DNS diagnostics fetching
   - Clipboard copy and export functionality (JSON/CSV)

### Data Collection Functions

Each function returns an object with key-value pairs of diagnostic information:

**Base Diagnostics (Synchronous):**
- `getBrowserInfo()` - User agent, browser detection with User-Agent Client Hints API, platform, languages, cookies, hardware info
- `getScreenInfo()` - Screen resolution, viewport, color depth, pixel ratio, orientation
- `getCapabilities()` - Feature detection: storage APIs, WebGL, service workers, WebAssembly, geolocation, notifications
- `getNetworkInfo()` - Online status, connection type, protocol, hostname
- `getPerformanceInfo()` - Page load metrics using Navigation Timing Level 2 API, DNS, TCP, TLS, TTFB, DOM load times
- `getSecurityInfo()` - Protocol, secure context, cross-origin isolation, referrer policy, mixed content detection
- `getAdditionalDetails()` - Tool version, URL, referrer, document mode, timezone, API support

**Async Diagnostics:**
- `getIPAddress()` - Fetches public IP with fallback (primary: api.ipify.org, fallback: Cloudflare trace)
- `getDNSResolverInfo()` - Tests connectivity to Cloudflare DNS resolvers (1.1.1.1, 1.0.0.1, IPv6)
- `testDNSPerformance()` - Measures Cloudflare DoH query timing, validates functionality, checks DNSSEC
- `testGoogleDNS()` - Benchmarks Google DNS (8.8.8.8) performance and DNSSEC validation
- `testAdGuardDNS()` - Tests AdGuard DNS performance with privacy-focused features
- `detectDNSLeaks()` - Multi-resolver testing to detect DNS leaks and identify actual DNS resolver

**DNS Helper Functions:**
- `parseCloudflareTrace()` - Extracts data center, TLS, HTTP version, WARP/Gateway status from trace data
- `detectDNSEncryption()` - Infers DoH/DoT/WARP status from connectivity and trace data

**Export Functions:**
- `exportAsJSON()` - Downloads diagnostic data as timestamped JSON file
- `exportAsCSV()` - Downloads diagnostic data as timestamped CSV file (Section, Key, Value format)

### Plain Text Rendering System

**Key Functions:**

- `formatValue(value)` - Converts values to plain text ("Enabled", "Disabled", "Not Available")
- `createInfoSection(title, data)` - Generates plain text section with dashes under title
- `updateTimestamp()` - Creates header with title, version number, separator line (60 equals signs), and timestamp
- `renderAllInfo()` - Orchestrates data collection, rendering, and all async diagnostics (IP + DNS tests)
- `copyToClipboard()` - Copies full report (header + content) to clipboard with visual feedback
- `refreshData()` - Triggers complete re-collection and re-rendering of all diagnostics

**Format Convention:**
```
BROWSER & NETWORK DIAGNOSTIC REPORT
============================================================
Version: 1.0.0
Generated: [timestamp]

Section Title
-------------
Key: Value
Key: Value
```

### Version Management

**Version Constant:** `TOOL_VERSION` (line 46)
- Current version: `1.0.0`
- Displayed in: page title, report header, Additional Details section
- Included in: JSON/CSV exports

**To Update Version:**
1. Change `TOOL_VERSION` constant (line 46)
2. Update version in page title (line 7)
3. Commit and push - GitHub Actions will auto-deploy

## Making Changes

### Preserving Plain Text Format

When modifying the tool, maintain the plain text output format:

- Use `<pre>` tags to preserve whitespace and newlines
- Use `.textContent` (not `.innerHTML`) when updating diagnostic content
- Keep `formatValue()` returning plain strings (no HTML)
- Maintain section formatting with dashes matching title length

### Adding New Diagnostic Sections

1. Create a new data collection function returning an object
2. Add to `diagnosticData` object in `renderAllInfo()` (lines 667-675 for sync, lines 685-710 for async)
3. The rendering system will automatically format it as plain text

### Modifying Display Format

- Header format: `updateTimestamp()` (lines 645-651)
- Section format: `createInfoSection()` (lines 654-662)
- Value formatting: `formatValue()` (lines 50-60)

### Async Data Updates

Multiple async diagnostics run in parallel using `Promise.all()`:
1. Render initial synchronous data without async values
2. Fetch all async data in parallel using Promise.all():
   - IP address (with fallback)
   - DNS resolver connectivity (4 endpoints)
   - Cloudflare DNS performance
   - Google DNS performance
   - AdGuard DNS performance
   - DNS leak detection
3. Update `diagnosticData` object with async results
4. Add new DNS diagnostic sections to `diagnosticData`
5. Re-render all sections with complete data

The pattern uses timeouts with AbortController:
- 3-second timeout for IP address detection (both primary and fallback)
- 5-second timeout for all DNS tests to prevent long waits on blocked connections

## Testing

Open `index.html` directly in a browser (no build step required).

**Test checklist:**
- Version number displays in page title and report header
- All 12 diagnostic sections display correctly (7 base + 5 DNS)
  1. Browser Information
  2. Screen & Display
  3. Browser Capabilities
  4. Network Information (includes async IP address)
  5. Performance Metrics
  6. Security Context
  7. Additional Details (includes Diagnostic Tool Version)
  8. DNS Resolver Connectivity
  9. DNS Protocol & Encryption
  10. DNS Network Path
  11. DNS Provider Benchmark (Cloudflare, Google, AdGuard)
  12. DNS Leak Detection
- Plain text formatting preserved (no HTML artifacts)
- All buttons work correctly: Copy, Refresh, Export JSON, Export CSV
- Copy button copies same format as displayed with header
- Refresh button updates all data including timestamp
- Export JSON creates valid JSON file with timestamp in filename
- Export CSV creates valid CSV file with Section, Key, Value columns
- IP address detection falls back to Cloudflare trace if ipify.org fails
- DNS diagnostics load asynchronously and update display
- Print preview hides all buttons but shows diagnostic text
- Timeouts work correctly on slow/blocked connections
- Online/offline events update network status dynamically

## External Dependencies

**IP Address Detection:**
- **api.ipify.org** - Primary IP address lookup API (3s timeout)
- **1.1.1.1/cdn-cgi/trace** - Fallback IP detection via Cloudflare trace

**DNS Resolver Connectivity:**
- **1.1.1.1/cdn-cgi/trace** - Cloudflare primary IPv4 resolver
- **1.0.0.1/cdn-cgi/trace** - Cloudflare secondary IPv4 resolver
- **[2606:4700:4700::1111]/cdn-cgi/trace** - Cloudflare primary IPv6 resolver
- **[2606:4700:4700::1001]/cdn-cgi/trace** - Cloudflare secondary IPv6 resolver

**DNS Performance Benchmarking:**
- **cloudflare-dns.com/dns-query** - Cloudflare DoH JSON API (queries cloudflare.com A record)
- **dns.google/resolve** - Google DNS-over-HTTPS API (queries cloudflare.com A record)
- **dns.adguard-dns.com/dns-query** - AdGuard DNS DoH API with privacy features

**DNS Leak Detection:**
- **whoami.ds.akahelp.net** - Akamai resolver identification (TXT record via Google DNS)
- **o-o.myaddr.l.google.com** - Google resolver identification (TXT record)
- **whoami.ipv6.akahelp.net** - IPv6 resolver identification (TXT record)

**Build/Deploy:**
- No build tools, frameworks, or npm packages required
- Pure HTML/CSS/JavaScript - deployable to any static host
- GitHub Actions for CI/CD (`.github/workflows/deploy.yml`)
- Cloudflare Pages for hosting

## Privacy & Security

- All diagnostics run client-side in the browser
- External requests (all read-only, no authentication):
  - GET to api.ipify.org for public IP address (with Cloudflare trace fallback)
  - GET to 1.1.1.1/1.0.0.1/IPv6 endpoints for DNS resolver connectivity tests
  - GET to cloudflare-dns.com, dns.google, dns.adguard-dns.com for DoH performance benchmarks
  - GET to various whoami/resolver identification endpoints for leak detection
- No personal data or browsing history is transmitted
- No cookies or tracking
- No analytics or telemetry
- Safe to share output (contains no secrets, only browser/network metadata)
- All DNS tests use timeouts (3-5 seconds) to prevent long waits on blocked connections
- Exported JSON/CSV files contain same data as displayed (no additional sensitive info)

## Deployment

### Automatic Deployment via GitHub Actions

The project uses GitHub Actions for continuous deployment to Cloudflare Pages.

**Workflow File:** `.github/workflows/deploy.yml`

**Trigger Events:**
- Push to `main` branch → Production deployment
- Pull requests → Preview deployments

**Deployment Process:**
1. GitHub Actions detects push to `main`
2. Checks out repository code
3. Uses `cloudflare/pages-action@v1` to deploy
4. Uploads static files to Cloudflare Pages
5. Deployment completes in ~30 seconds

**Configuration:**
- **Project Name:** `web-tech-support`
- **Account ID:** Stored in workflow file
- **API Token:** Stored in GitHub Secrets (`CLOUDFLARE_API_TOKEN`)
- **Directory:** `.` (root directory)
- **No build command** - static HTML deployment

**Live URLs:**
- Production: https://web-tech-support.pages.dev
- Custom Domain: https://web-tech-support.mattz.cc

**Manual Deployment (if needed):**
```bash
# Set environment variables
export CLOUDFLARE_API_TOKEN="<token>"
export CLOUDFLARE_ACCOUNT_ID="04083e374d8ab0c8b215859b8dea0e8b"

# Deploy using Wrangler CLI
wrangler pages deploy . --project-name=web-tech-support --branch=main
```

**Monitoring Deployments:**
```bash
# View recent deployments
gh run list --limit 5

# Watch current deployment
gh run watch

# View deployment logs
gh run view --log
```

### Deployment Checklist

When making changes that should be deployed:
1. Test locally by opening `index.html` in browser
2. Update `TOOL_VERSION` if making significant changes
3. Commit changes with descriptive message
4. Push to `main` branch
5. GitHub Actions automatically deploys
6. Verify deployment at live URLs
7. Check GitHub Actions tab for deployment status

### Rollback Procedure

If a deployment introduces issues:
1. Go to GitHub Actions tab
2. Find the last working deployment
3. Re-run that workflow, OR
4. Use `git revert` and push to trigger new deployment
