# HTTPS/SSL Setup Guide for gaonchoiofficial.com

## Current Status

This repository has been configured with comprehensive HTTPS/SSL security headers in the code:
- ✅ Security meta tags in `index.html`
- ✅ `_headers` file for Netlify/GitHub Pages
- ✅ `.htaccess` file for Apache servers
- ✅ Content Security Policy with `upgrade-insecure-requests`
- ✅ HSTS (HTTP Strict Transport Security) headers

However, **HTTPS must be enabled in GitHub Pages settings** for the site to actually serve over HTTPS.

## How to Enable HTTPS on GitHub Pages

### Step 1: Verify Custom Domain is Set

1. Go to your repository on GitHub: `https://github.com/nicholasdudek/gaonchoiofficial.com`
2. Click on **Settings** tab
3. Scroll down to **Pages** section (in the left sidebar under "Code and automation")
4. Under "Custom domain", verify that `gaonchoiofficial.com` is entered
5. The CNAME file in the repository should contain: `gaonchoiofficial.com`

### Step 2: Configure DNS Settings

Ensure your domain's DNS is properly configured:

#### For Apex Domain (gaonchoiofficial.com):
Add these A records pointing to GitHub Pages:
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

#### Verify DNS:
You can verify DNS configuration using:
```bash
dig gaonchoiofficial.com +noall +answer
```

### Step 3: Enable HTTPS

1. In GitHub repository **Settings** → **Pages**
2. Wait for DNS check to complete (may take a few minutes)
3. Once DNS is verified, you'll see a checkbox: **"Enforce HTTPS"**
4. Check the **"Enforce HTTPS"** checkbox
5. GitHub will automatically provision a Let's Encrypt SSL certificate

### Step 4: Wait for Certificate Provisioning

- Certificate provisioning can take up to 24 hours
- Usually completes within a few minutes
- You'll see a message: "HTTPS is being provisioned for your domain"

### Step 5: Verify HTTPS is Working

Once enabled, verify:
```bash
# Check if HTTPS is working
curl -I https://gaonchoiofficial.com

# Verify security headers
curl -I https://gaonchoiofficial.com | grep -i "strict-transport-security"
```

You should see:
- Status: `200 OK`
- Security headers like `Strict-Transport-Security`

## Troubleshooting

### "HTTPS not available yet"
- **Cause**: DNS not propagated or incorrect
- **Solution**: Verify DNS A records point to GitHub Pages IPs
- **Wait**: DNS propagation can take 24-48 hours

### "Certificate provisioning failed"
- **Cause**: DNS CAA records blocking Let's Encrypt
- **Solution**: Ensure no CAA records, or add: `CAA 0 issue "letsencrypt.org"`

### "Enforce HTTPS checkbox is disabled"
- **Cause**: Custom domain DNS check not passing
- **Solution**: 
  1. Remove custom domain
  2. Wait 24 hours for DNS to clear
  3. Re-add custom domain
  4. Wait for DNS verification

### Mixed Content Warnings
All external resources in this site already use HTTPS:
- ✅ Google Fonts: `https://fonts.googleapis.com`
- ✅ Google Fonts CDN: `https://fonts.gstatic.com`
- ✅ Instagram Embed: `https://www.instagram.com`

## Security Headers Included

Once HTTPS is enabled, these security headers will be active:

### Browser-Level (via meta tags in index.html):
- **Content-Security-Policy**: Enforces HTTPS, whitelists trusted sources
- **X-Frame-Options**: SAMEORIGIN (prevents clickjacking)
- **X-Content-Type-Options**: nosniff (prevents MIME sniffing)
- **Referrer-Policy**: strict-origin-when-cross-origin
- **Permissions-Policy**: Restricts geolocation, microphone, camera

### Server-Level (via _headers for GitHub Pages):
- **Strict-Transport-Security**: max-age=31536000; includeSubDomains; preload
- All headers from meta tags for redundancy

### Apache Server (via .htaccess):
- Automatic HTTP → HTTPS redirect (301)
- Same security headers as above

## HSTS Preload (Optional)

For maximum security, submit to HSTS Preload list:
1. Ensure HSTS header has: `max-age=31536000; includeSubDomains; preload`
2. Visit: https://hstspreload.org/
3. Enter: `gaonchoiofficial.com`
4. Submit domain

⚠️ **Warning**: HSTS preload is a one-way operation that takes months to reverse. Only do this when you're certain HTTPS is stable.

## Resources

- [GitHub Pages Custom Domain Documentation](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)
- [Securing GitHub Pages with HTTPS](https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https)
- [HSTS Preload](https://hstspreload.org/)
- [Let's Encrypt](https://letsencrypt.org/)

## Quick Checklist

- [ ] CNAME file exists with `gaonchoiofficial.com`
- [ ] DNS A records point to GitHub Pages IPs
- [ ] DNS propagated (wait 24-48 hours after DNS changes)
- [ ] Custom domain verified in GitHub Pages settings
- [ ] "Enforce HTTPS" checkbox enabled in GitHub Pages settings
- [ ] Certificate provisioned by Let's Encrypt
- [ ] Site accessible via https://gaonchoiofficial.com
- [ ] Security headers visible in browser dev tools

---

**Note**: The code changes for HTTPS security are already complete. The remaining step is to enable HTTPS in the GitHub repository settings following the instructions above.
