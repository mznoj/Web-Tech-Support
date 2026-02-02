# Cloudflare Pages Deployment Guide

This guide documents the deployment setup for the Browser & Network Diagnostic Tool.

## Current Deployment Status

✅ **ACTIVE**: Automatic deployment via GitHub Actions
- **Live URLs**:
  - Production: https://web-tech-support.pages.dev
  - Custom Domain: https://web-tech-support.mattz.cc
- **Method**: GitHub Actions workflow (`.github/workflows/deploy.yml`)
- **Trigger**: Every push to `main` branch
- **Deploy Time**: ~30 seconds

## Deployment Methods

### Method 1: GitHub Actions (Current - Recommended)

The project is currently deployed using GitHub Actions, which provides:
- Automatic deployment on every push to `main`
- Preview deployments for pull requests
- Full control via code (Infrastructure as Code)
- Easy monitoring via `gh` CLI

**How it works:**
1. Push code to `main` branch
2. GitHub Actions workflow triggers automatically
3. Uses `cloudflare/pages-action@v1` to deploy
4. Updates live site in ~30 seconds

**Monitor deployments:**
```bash
gh run list          # View recent deployments
gh run watch         # Watch current deployment
gh run view --log    # View deployment logs
```

No additional setup needed - it's already working!

### Method 2: Cloudflare Dashboard (Alternative)

You can also connect GitHub directly via the Cloudflare dashboard for native Git integration. This is optional since GitHub Actions is already handling deployments.

## Prerequisites (for Dashboard Method)

- A Cloudflare account (free tier works fine)
- GitHub repository: `https://github.com/mznoj/Web-Tech-Support.git`
- Admin access to the GitHub repository

## Step-by-Step Setup

### 1. Access Cloudflare Pages

1. Log in to your [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. Click on **Workers & Pages** in the left sidebar
3. Click **Create application** button
4. Select the **Pages** tab
5. Click **Connect to Git**

### 2. Connect to GitHub

1. Click **Connect GitHub** button
2. Authorize Cloudflare Pages to access your GitHub account
3. Select **Only select repositories**
4. Choose **mznoj/Web-Tech-Support** from the dropdown
5. Click **Install & Authorize**

### 3. Configure Build Settings

Once GitHub is connected, configure your project:

**Project name:** `web-tech-support` (or your preferred name)

**Production branch:** `main`

**Build settings:**
- **Framework preset:** None
- **Build command:** (leave empty)
- **Build output directory:** `/`

**Environment variables:** (none required)

Click **Save and Deploy**

### 4. Deployment

- Cloudflare Pages will immediately deploy your site
- Deployment takes ~30 seconds
- You'll get a URL like: `https://web-tech-support.pages.dev`

### 5. Automatic Deployments

Going forward:
- **Every push to `main`** → Automatic production deployment
- **Pull requests** → Automatic preview deployments with unique URLs
- **Branch commits** → Optional preview deployments (configurable)

## Custom Domain (Optional)

To use a custom domain:

1. Go to your Pages project → **Custom domains**
2. Click **Set up a custom domain**
3. Enter your domain (e.g., `diagnostics.yourdomain.com`)
4. Follow DNS configuration instructions
5. Cloudflare automatically provisions SSL certificate

## Build Configuration

This project uses `.cloudflare-pages.json` for build settings:
- **No build command** - Pure static HTML/CSS/JS
- **Output directory:** Root (`/`)
- No dependencies or build tools required

## Deployment Triggers

Deployments are triggered by:
- ✅ Push to main branch
- ✅ Merged pull requests
- ✅ Manual deployments via dashboard
- ✅ Cloudflare Pages API (advanced)

## Performance Features

Cloudflare Pages automatically provides:
- Global CDN distribution (200+ data centers)
- Automatic HTTPS/SSL
- HTTP/2 and HTTP/3 support
- Brotli compression
- Smart caching
- DDoS protection
- Web Analytics (optional, privacy-focused)

## Monitoring Deployments

View deployment status:
1. Go to **Workers & Pages** → **Your project**
2. Check **Deployments** tab for history
3. Click any deployment to view build logs

## Troubleshooting

**Deployment failed:**
- Check build logs in Cloudflare dashboard
- Ensure `index.html` exists in repository root
- Verify GitHub permissions are still active

**DNS issues:**
- Wait 24-48 hours for DNS propagation
- Verify DNS records in Cloudflare DNS settings
- Check domain registrar nameservers point to Cloudflare

**Preview deployments not working:**
- Go to Settings → Builds & deployments
- Enable "Preview deployments" for pull requests

## Rollback

To rollback to a previous deployment:
1. Go to **Deployments** tab
2. Find the working deployment
3. Click **⋯** (three dots) → **Rollback to this deployment**

## Local Testing

Test locally before deploying:
```bash
# Simple HTTP server (Python 3)
python3 -m http.server 8000

# Or using npx (no install needed)
npx serve .

# Or using PHP
php -S localhost:8000
```

Visit: `http://localhost:8000`

## GitHub Actions (Optional Advanced)

For more control, you can use GitHub Actions with Cloudflare Pages:
- Direct deployment via Wrangler CLI
- Custom build steps
- Integration testing before deploy
- Deployment notifications

See: https://developers.cloudflare.com/pages/how-to/use-direct-upload-with-continuous-integration/

## Support & Documentation

- [Cloudflare Pages Docs](https://developers.cloudflare.com/pages/)
- [Pages Pricing](https://pages.cloudflare.com/#pricing) (500 builds/month free)
- [Community Forum](https://community.cloudflare.com/c/developers/pages/)
- [Status Page](https://www.cloudflarestatus.com/)
