# SSL Certificate Setup Guide for GitHub Pages with Namecheap Domain

This guide explains how to enable free SSL/TLS certificate for your custom domain (sheepu.me) on GitHub Pages using Namecheap DNS.

## Overview

GitHub Pages provides free SSL certificates through Let's Encrypt for custom domains. No need to purchase or manually configure SSL certificates from Namecheap - GitHub handles this automatically once DNS is properly configured.

## Prerequisites

- A GitHub Pages repository (mh-sheepu.github.io)
- A custom domain registered with Namecheap (sheepu.me)
- Access to Namecheap DNS settings
- Access to GitHub repository settings

## Step-by-Step Setup

### 1. Configure DNS Settings in Namecheap

1. **Log in to Namecheap**
   - Go to https://www.namecheap.com
   - Sign in to your account

2. **Navigate to Domain Management**
   - Go to "Domain List"
   - Click "Manage" next to your domain (sheepu.me)

3. **Configure Advanced DNS Settings**
   - Click on the "Advanced DNS" tab
   - Add the following DNS records:

   **For Apex Domain (sheepu.me):**
   ```
   Type: A Record
   Host: @
   Value: 185.199.108.153
   TTL: Automatic
   
   Type: A Record
   Host: @
   Value: 185.199.109.153
   TTL: Automatic
   
   Type: A Record
   Host: @
   Value: 185.199.110.153
   TTL: Automatic
   
   Type: A Record
   Host: @
   Value: 185.199.111.153
   TTL: Automatic
   ```

   **For WWW Subdomain (www.sheepu.me):**
   ```
   Type: CNAME Record
   Host: www
   Value: mh-sheepu.github.io
   TTL: Automatic
   ```

4. **Save DNS Settings**
   - Remove any conflicting DNS records (old A records or CNAME for @)
   - Save all changes

### 2. Configure GitHub Pages Settings

1. **Access Repository Settings**
   - Go to https://github.com/mh-sheepu/mh-sheepu.github.io
   - Click on "Settings" tab
   - Navigate to "Pages" in the left sidebar

2. **Configure Custom Domain**
   - Under "Custom domain", enter: `sheepu.me`
   - Click "Save"
   - GitHub will automatically create/update the CNAME file in your repository

3. **Wait for DNS Check**
   - GitHub will verify DNS configuration
   - This can take a few minutes to 48 hours depending on DNS propagation
   - You'll see a green checkmark when DNS is properly configured

4. **Enable HTTPS**
   - Once DNS check passes, you'll see "Enforce HTTPS" option
   - Check the "Enforce HTTPS" checkbox
   - GitHub will automatically provision a free SSL certificate from Let's Encrypt
   - This process can take up to 24 hours

### 3. Verify SSL Certificate

After DNS propagates and GitHub provisions the certificate:

1. **Check HTTPS Access**
   - Visit https://sheepu.me
   - Click the padlock icon in your browser's address bar
   - Verify the certificate is issued by "Let's Encrypt"

2. **Verify HTTP to HTTPS Redirect**
   - Visit http://sheepu.me (without 's')
   - Confirm it automatically redirects to https://sheepu.me

3. **Test WWW Subdomain**
   - Visit https://www.sheepu.me
   - Confirm it works and shows a valid certificate

## Troubleshooting

### DNS Propagation Issues

**Problem:** DNS changes not reflecting immediately

**Solutions:**
- Wait 24-48 hours for full DNS propagation
- Clear your local DNS cache:
  - Windows: `ipconfig /flushdns`
  - Mac: `sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder`
  - Linux: `sudo systemd-resolve --flush-caches`
- Check DNS propagation: https://www.whatsmydns.net/#A/sheepu.me

### Certificate Not Provisioning

**Problem:** "Enforce HTTPS" option not appearing or certificate not issued

**Solutions:**
- Verify DNS records are correct using: `nslookup sheepu.me` or `dig sheepu.me`
- Ensure CNAME file contains only the domain name (sheepu.me)
- Try unchecking and re-saving the custom domain in GitHub settings
- Check GitHub Pages status: https://www.githubstatus.com/
- Wait up to 24 hours for certificate provisioning

### CAA Records (Advanced)

**Problem:** Certificate issuance blocked by CAA records

**Solution:**
If you have CAA records in Namecheap, add:
```
Type: CAA Record
Host: @
Value: 0 issue "letsencrypt.org"
TTL: Automatic

Type: CAA Record
Host: @
Value: 0 issue "pki.goog"
TTL: Automatic
```

### Mixed Content Warnings

**Problem:** Browser showing "Not Secure" despite HTTPS

**Solutions:**
- Ensure all resources (images, scripts, stylesheets) use HTTPS URLs
- Update any hardcoded `http://` URLs to `https://` or use protocol-relative URLs (`//`)
- Check browser console for mixed content warnings

## Maintenance

### Certificate Renewal

- GitHub automatically renews Let's Encrypt certificates
- No manual intervention required
- Certificates are renewed every 90 days automatically

### Monitoring SSL Status

Regularly check:
- https://www.ssllabs.com/ssltest/analyze.html?d=sheepu.me
- Verify certificate expiration date
- Ensure HTTPS enforcement remains enabled

## Additional Security Enhancements

### Security Headers

Consider adding a `_headers` file or using GitHub Pages plugins to add security headers:
- `Strict-Transport-Security` (HSTS)
- `X-Content-Type-Options`
- `X-Frame-Options`
- `X-XSS-Protection`
- `Referrer-Policy`
- `Content-Security-Policy`

Note: GitHub Pages has limited support for custom headers. Some headers can be added via meta tags in HTML.

### Content Security Policy

Add to your HTML `<head>` section:
```html
<meta http-equiv="Content-Security-Policy" content="default-src 'self' 'unsafe-inline' 'unsafe-eval'; img-src 'self' data: https:; font-src 'self' data:; connect-src 'self';">
```

## Resources

- [GitHub Pages Custom Domain Documentation](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)
- [GitHub Pages HTTPS Documentation](https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https)
- [Namecheap DNS Setup Guide](https://www.namecheap.com/support/knowledgebase/article.aspx/9645/2208/how-do-i-link-my-domain-to-github-pages/)
- [Let's Encrypt About](https://letsencrypt.org/about/)

## Summary

1. Configure DNS A records in Namecheap to point to GitHub Pages IP addresses
2. Add CNAME record for www subdomain
3. Set custom domain in GitHub Pages settings
4. Enable "Enforce HTTPS" after DNS verification
5. Wait for automatic SSL certificate provisioning (up to 24 hours)
6. Verify SSL certificate is working

The entire process is free - no need to purchase SSL certificates from Namecheap!
