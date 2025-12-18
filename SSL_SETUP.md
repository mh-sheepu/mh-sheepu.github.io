# Free SSL Certificate Setup Guide

This guide will help you enable a free SSL certificate for your GitHub Pages site using the custom domain `sheepu.me`.

## Overview

GitHub Pages provides **free SSL certificates** through [Let's Encrypt](https://letsencrypt.org/) for custom domains. Once configured, your site will be accessible via HTTPS, providing:

- 🔒 Encrypted traffic between visitors and your site
- ✅ Browser trust indicators (padlock icon)
- 🚀 Better SEO rankings
- 🛡️ Protection against man-in-the-middle attacks

## Prerequisites

- Custom domain configured (✅ Already done: `sheepu.me`)
- Access to your domain's DNS settings
- GitHub repository settings access

## Step 1: Configure DNS Records

To enable SSL on GitHub Pages, you need to configure your DNS records correctly.

### Option A: Using A Records (Recommended for apex domains)

Add the following **A records** for `sheepu.me`:

```
Type: A
Host: @
Value: 185.199.108.153

Type: A
Host: @
Value: 185.199.109.153

Type: A
Host: @
Value: 185.199.110.153

Type: A
Host: @
Value: 185.199.111.153
```

### Option B: Using CNAME Record (For www subdomain)

If you also want `www.sheepu.me` to work:

```
Type: CNAME
Host: www
Value: mh-sheepu.github.io
```

### Important DNS Notes

1. **Remove any conflicting records**: Delete old A records or CNAME records pointing to different IPs
2. **TTL**: Set TTL to 3600 seconds (1 hour) or lower for faster propagation
3. **CAA Records** (Optional): Add CAA records to authorize Let's Encrypt:
   ```
   Type: CAA
   Host: @
   Value: 0 issue "letsencrypt.org"
   ```

## Step 2: Wait for DNS Propagation

After updating DNS records:

1. **Wait 24-48 hours** for DNS changes to propagate worldwide
2. Verify DNS propagation using:
   - [DNS Checker](https://dnschecker.org/)
   - Command line: `dig sheepu.me +short`
   - Expected result: Should show GitHub Pages IP addresses

```bash
# Check if DNS is pointing to GitHub Pages
dig sheepu.me +short
# Should return:
# 185.199.108.153
# 185.199.109.153
# 185.199.110.153
# 185.199.111.153
```

## Step 3: Enable HTTPS in GitHub Pages Settings

Once DNS is properly configured:

1. Go to your GitHub repository: `https://github.com/mh-sheepu/mh-sheepu.github.io`
2. Click on **Settings** tab
3. Scroll down to **Pages** section (in the left sidebar under "Code and automation")
4. Under **Custom domain**, verify `sheepu.me` is shown
5. Wait for the DNS check to complete (green checkmark should appear)
6. Once DNS check passes, check the box: **☑ Enforce HTTPS**

### Important Notes

- The "Enforce HTTPS" checkbox may be disabled initially
- It becomes available only after DNS check succeeds
- GitHub automatically provisions a Let's Encrypt SSL certificate
- Certificate provisioning can take a few minutes to several hours

## Step 4: Verify SSL Certificate

After enabling HTTPS:

1. Visit `https://sheepu.me` in your browser
2. Click the padlock icon in the address bar
3. Verify the certificate is issued by "Let's Encrypt"
4. Certificate should be valid for 90 days and auto-renews

### Testing SSL Configuration

```bash
# Check SSL certificate details
curl -vI https://sheepu.me 2>&1 | grep -A 5 "SSL certificate"

# Or use online tools:
# - https://www.ssllabs.com/ssltest/
# - https://www.whynopadlock.com/
```

## Troubleshooting

### Issue: "Enforce HTTPS" checkbox is disabled

**Causes:**
- DNS not properly configured
- DNS still propagating
- CNAME file missing or incorrect

**Solutions:**
1. Verify DNS records are correct
2. Wait 24-48 hours for DNS propagation
3. Check CNAME file contains exactly: `sheepu.me`
4. Try removing and re-adding the custom domain in GitHub settings

### Issue: Certificate Error or "Not Secure" Warning

**Causes:**
- Certificate not yet provisioned
- Certificate expired (shouldn't happen with auto-renewal)
- Mixed content (HTTP resources on HTTPS page)

**Solutions:**
1. Wait a few hours for certificate provisioning
2. Clear browser cache and cookies
3. Check for mixed content in browser console
4. Ensure all resources (images, scripts) use HTTPS or relative URLs

### Issue: DNS Check Fails in GitHub Settings

**Causes:**
- Incorrect DNS records
- Multiple conflicting records
- DNS not propagated yet

**Solutions:**
1. Double-check A records match GitHub's IPs exactly
2. Remove any old/conflicting DNS records
3. Wait longer for DNS propagation (up to 48 hours)
4. Use `dig` or `nslookup` to verify DNS resolution

### Issue: Site Shows "404 There isn't a GitHub Pages site here"

**Causes:**
- CNAME file missing or incorrect
- GitHub Pages not enabled

**Solutions:**
1. Verify CNAME file exists and contains `sheepu.me`
2. Ensure repository is public or GitHub Pages is enabled for private repositories
3. Check Pages settings in repository settings

## Security Best Practices

### 1. HTTP to HTTPS Redirect

Once HTTPS is enabled, GitHub Pages automatically redirects HTTP to HTTPS when "Enforce HTTPS" is checked.

### 2. Update All Links

Update any hardcoded links in your site:

```html
<!-- Before -->
<link rel="stylesheet" href="http://sheepu.me/style.css">

<!-- After -->
<link rel="stylesheet" href="https://sheepu.me/style.css">

<!-- Or better, use relative/protocol-relative URLs -->
<link rel="stylesheet" href="/style.css">
```

### 3. Content Security Policy (Optional)

Add CSP meta tag to enhance security:

```html
<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests">
```

This upgrades any HTTP requests to HTTPS automatically.

### 4. HSTS Header (Automatic)

GitHub Pages automatically sets HSTS (HTTP Strict Transport Security) headers, forcing browsers to always use HTTPS.

## Certificate Renewal

- Let's Encrypt certificates are valid for **90 days**
- GitHub Pages **automatically renews** certificates before expiration
- No manual intervention required
- Monitor certificate status in your repository's Pages settings

## Additional Resources

- [GitHub Pages Documentation](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)
- [Securing your GitHub Pages site with HTTPS](https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https)
- [Let's Encrypt Documentation](https://letsencrypt.org/docs/)
- [DNS Configuration Help](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)

## Quick Reference Checklist

- [ ] Configure DNS A records for apex domain
- [ ] Configure CNAME record for www subdomain (optional)
- [ ] Wait 24-48 hours for DNS propagation
- [ ] Verify DNS using `dig` or online tools
- [ ] Enable GitHub Pages in repository settings
- [ ] Add custom domain `sheepu.me` in Pages settings
- [ ] Wait for DNS check to pass (green checkmark)
- [ ] Enable "Enforce HTTPS" checkbox
- [ ] Wait for SSL certificate provisioning (a few hours)
- [ ] Test HTTPS access at `https://sheepu.me`
- [ ] Verify certificate in browser (padlock icon)
- [ ] Update any hardcoded HTTP links to HTTPS

## Support

If you encounter issues not covered in this guide:
1. Check [GitHub Status](https://www.githubstatus.com/) for ongoing issues
2. Review [GitHub Community Forums](https://github.community/)
3. Contact GitHub Support for Pages-specific issues

---

**Note**: This site uses GitHub Pages' free SSL certificate service. No third-party SSL providers or manual certificate installation required!
