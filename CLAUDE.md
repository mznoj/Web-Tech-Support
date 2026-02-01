# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

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

3. **JavaScript Diagnostic Engine** (lines 43-396)
   - Data collection functions (7 categories)
   - Plain text rendering system
   - Async IP address fetching
   - Clipboard copy functionality

### Data Collection Functions

Each function returns an object with key-value pairs of diagnostic information:

- `getBrowserInfo()` - User agent, browser detection, platform, languages, cookies
- `getScreenInfo()` - Screen resolution, viewport, color depth, pixel ratio, orientation
- `getCapabilities()` - Feature detection: storage APIs, WebGL, service workers, WebAssembly
- `getNetworkInfo()` - Online status, connection type, protocol, hostname
- `getPerformanceInfo()` - Page load metrics, DNS, TCP, TLS, TTFB, DOM load times
- `getSecurityInfo()` - Protocol, secure context, cross-origin isolation, referrer policy
- `getAdditionalDetails()` - URL, referrer, document mode, timezone, API support
- `getIPAddress()` - Async function fetching public IP from api.ipify.org

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
2. Add to `diagnosticData` object in `renderAllInfo()` (lines 308-316)
3. The rendering system will automatically format it as plain text

### Modifying Display Format

- Header format: `updateTimestamp()` (lines 285-292)
- Section format: `createInfoSection()` (lines 294-303)
- Value formatting: `formatValue()` (lines 47-58)

### Async Data Updates

The IP address demonstrates the pattern for async data:
1. Render initial data without async values
2. Fetch async data (lines 327-328)
3. Update `diagnosticData` object
4. Re-render all sections with updated data (lines 330-335)

## Testing

Open `index.html` directly in a browser (no build step required).

**Test checklist:**
- All 7 diagnostic sections display correctly
- Plain text formatting preserved (no HTML artifacts)
- Copy button copies same format as displayed
- Refresh button updates all data including timestamp
- IP address loads asynchronously and updates display
- Print preview hides buttons but shows all diagnostic text

## External Dependencies

- **api.ipify.org** - Free IP address lookup API (read-only, no authentication)
- No build tools, frameworks, or npm packages required

## Privacy & Security

- All diagnostics run client-side in the browser
- Only external request: GET to api.ipify.org for public IP address
- No data is sent to any servers
- No cookies or tracking
- Safe to share output (contains no secrets, only browser/network metadata)
