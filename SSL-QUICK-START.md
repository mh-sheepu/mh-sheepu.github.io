# Quick Start: Enable SSL on GitHub Pages with Namecheap

**Time Required:** 5-10 minutes (+ up to 24 hours for DNS propagation and certificate provisioning)

## Fast Track Setup

### Step 1: Namecheap DNS (5 minutes)

1. Login to Namecheap → Domain List → Manage (sheepu.me) → Advanced DNS
2. Add these 4 A Records for `@`:
   - `185.199.108.153`
   - `185.199.109.153`
   - `185.199.110.153`
   - `185.199.111.153`
3. Add CNAME Record: `www` → `mh-sheepu.github.io`
4. Save changes

### Step 2: GitHub Pages Settings (2 minutes)

1. Go to repository Settings → Pages
2. Custom domain: Enter `sheepu.me` → Save
3. Wait for DNS check (✅ appears when ready)
4. Check "Enforce HTTPS"

### Step 3: Wait & Verify (0-24 hours)

- DNS propagation: 0-48 hours (usually faster)
- SSL certificate: 0-24 hours (usually within 1 hour)
- Visit https://sheepu.me to verify

## That's It!

GitHub automatically provides and renews your free SSL certificate from Let's Encrypt. No purchase needed from Namecheap.

**Need detailed instructions?** See [SSL-SETUP.md](SSL-SETUP.md)

## Troubleshooting Quick Fixes

| Issue | Solution |
|-------|----------|
| "DNS check failed" | Wait 24-48 hours for DNS propagation |
| "Enforce HTTPS" not showing | DNS not propagated yet, wait longer |
| Certificate not working | Wait up to 24 hours after DNS check passes |
| Mixed content warning | Update all URLs in code to use `https://` |

## Verify DNS (Optional)

Check if DNS is propagated:
```bash
nslookup sheepu.me
# Should show GitHub Pages IP addresses

dig sheepu.me
# Should show the 4 GitHub Pages IPs
```

## Check SSL Status (After Setup)

- Browser: Click padlock icon → Certificate should say "Let's Encrypt"
- Online test: https://www.ssllabs.com/ssltest/analyze.html?d=sheepu.me
