# MediAItor Universal Links

This repository hosts the `.well-known` files for iOS Universal Links and Android App Links for the MediAItor app.

## Files

- `.well-known/apple-app-site-association` - iOS Universal Links configuration
- `.well-known/assetlinks.json` - Android App Links configuration

## Domain

This is deployed at: `https://mediaitor.junotech.app`

## Cloudflare Pages Deployment

This repo is automatically deployed via Cloudflare Pages:
1. Connected to GitHub
2. Auto-deploys on push to main branch
3. Custom domain: `mediaitor.junotech.app`

## Updating

To update the configuration:
1. Edit files in `.well-known/`
2. Commit and push to main
3. Cloudflare automatically redeploys

## Verification

Check if files are accessible:
```bash
curl https://mediaitor.junotech.app/.well-known/apple-app-site-association
curl https://mediaitor.junotech.app/.well-known/assetlinks.json
```

## App Configuration

- **iOS Bundle ID:** `com.junotech.mediaitor`
- **Android Package:** `com.junotech.mediaitor`
- **Apple Team ID:** `TEAMID` (placeholder - update when Developer account approved)
