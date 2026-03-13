# FranzAudio.com — Deployment Guide

## Step 1: Create a GitHub Repository

1. Go to [github.com/new](https://github.com/new)
2. Name the repo: `franzaudio.com` (or any name you like)
3. Set it to **Public**
4. Click **Create repository**

## Step 2: Push Your Site Files

Open a terminal in this folder and run:

```bash
git init
git add index.html CNAME
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/franzaudio.com.git
git push -u origin main
```

Replace `YOUR_USERNAME` with your GitHub username.

## Step 3: Enable GitHub Pages

1. Go to your repo on GitHub → **Settings** → **Pages**
2. Under **Source**, select **Deploy from a branch**
3. Branch: `main`, folder: `/ (root)`
4. Click **Save**
5. Under **Custom domain**, enter `franzaudio.com` and click **Save**
6. Check **Enforce HTTPS** once it becomes available

## Step 4: Configure Cloudflare DNS

1. Log in to [Cloudflare](https://dash.cloudflare.com)
2. Add your site `franzaudio.com` if you haven't already
3. Go to **DNS** → **Records** and add these records:

### For the apex domain (franzaudio.com):

| Type  | Name | Content              | Proxy |
|-------|------|----------------------|-------|
| A     | @    | 185.199.108.153      | DNS only (grey cloud) |
| A     | @    | 185.199.109.153      | DNS only (grey cloud) |
| A     | @    | 185.199.110.153      | DNS only (grey cloud) |
| A     | @    | 185.199.111.153      | DNS only (grey cloud) |

### For www subdomain (optional):

| Type  | Name | Content              | Proxy |
|-------|------|----------------------|-------|
| CNAME | www  | YOUR_USERNAME.github.io | DNS only (grey cloud) |

> **Important:** Set the proxy status to **DNS only** (grey cloud icon), not **Proxied** (orange cloud). GitHub Pages handles SSL itself, and proxying through Cloudflare can cause redirect loops.

## Step 5: Cloudflare SSL Settings

1. Go to **SSL/TLS** → **Overview**
2. Set encryption mode to **Full** (not Full Strict, not Flexible)
3. Go to **Edge Certificates** and turn OFF **Always Use HTTPS** (let GitHub handle this)

## Step 6: Wait & Verify

- DNS changes can take a few minutes to propagate
- GitHub Pages may take up to 10 minutes to issue an SSL certificate
- Visit `https://franzaudio.com` to verify everything works

## Troubleshooting

- **Redirect loop?** Make sure Cloudflare proxy is OFF (grey cloud) for the A records
- **SSL error?** Set Cloudflare SSL to "Full" and wait for GitHub to provision the certificate
- **404 error?** Make sure the CNAME file is in the repo root and GitHub Pages is enabled
- **Wrong content?** Clear your browser cache or try incognito mode
