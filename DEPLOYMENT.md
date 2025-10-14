# Deployment Instructions

Your repository is ready to deploy! Follow these steps to get your universal links live.

## Repository Status
- ✅ Git initialized
- ✅ Initial commit created
- ✅ All files ready

## Step 1: Create GitHub Repository

1. Go to https://github.com/new
2. Repository name: `mediaitor-universal-links`
3. Description: "Universal Links configuration for MediAItor iOS and Android apps"
4. Public or Private: **Public** (required for Cloudflare Pages free tier)
5. **DO NOT** initialize with README, .gitignore, or license (we already have these)
6. Click **Create repository**

## Step 2: Push to GitHub

After creating the repository on GitHub, run these commands from this directory:

```bash
cd /Users/emorywinters/Development/mediaitor-universal-links

git remote add origin https://github.com/YOUR_USERNAME/mediaitor-universal-links.git
git branch -M main
git push -u origin main
```

Replace `YOUR_USERNAME` with your actual GitHub username.

## Step 3: Configure Cloudflare Pages

1. Go to Cloudflare Dashboard: https://dash.cloudflare.com
2. Click **Pages** in the left sidebar
3. Click **Create a project**
4. Click **Connect to Git**
5. Authorize Cloudflare to access your GitHub account
6. Select the repository: `mediaitor-universal-links`
7. Configure build settings:
   - **Project name:** `mediaitor-universal-links`
   - **Production branch:** `main`
   - **Build command:** Leave empty (static site)
   - **Build output directory:** `/`
8. Click **Save and Deploy**

## Step 4: Add Custom Domain

1. After the first deployment completes, go to **Custom domains** tab
2. Click **Set up a custom domain**
3. Enter: `mediaitor.junotech.app`
4. Cloudflare will automatically configure DNS (since you already use Cloudflare for junotech.app)
5. Wait 1-2 minutes for DNS propagation

## Step 5: Verify Deployment

Check that your universal links files are accessible:

```bash
# Test iOS Universal Links
curl https://mediaitor.junotech.app/.well-known/apple-app-site-association

# Test Android App Links
curl https://mediaitor.junotech.app/.well-known/assetlinks.json

# Test landing page
curl https://mediaitor.junotech.app
```

Expected results:
- ✅ AASA file returns JSON (not HTML)
- ✅ Asset Links returns JSON array
- ✅ No redirects (direct 200 OK response)
- ✅ HTTPS enabled automatically

## Step 6: Configure Supabase Redirect URL

1. Go to Supabase Dashboard: https://supabase.com/dashboard/project/awafqomcarixnvoisluq
2. Navigate to **Authentication** → **URL Configuration**
3. Under **Redirect URLs**, add:
   ```
   https://mediaitor.junotech.app/auth/callback
   ```
4. Click **Save**

## Step 7: Test Email Verification Flow

1. In the MediAItor app, sign up with a new test account
2. Check email for verification link
3. Click the verification link
4. **Expected:** App should open automatically (not browser)
5. **Expected:** User is verified and logged in

## Troubleshooting

### iOS App Not Opening

If clicking the verification link opens Safari instead of the app:

1. **Wait 5-10 minutes** - iOS caches AASA files aggressively
2. **Verify AASA is accessible:**
   ```bash
   curl -v https://mediaitor.junotech.app/.well-known/apple-app-site-association
   ```
   Should return 200 OK with JSON (no redirects!)
3. **Uninstall and reinstall** the app to refresh universal links cache
4. **Update Apple Team ID** once your Developer account is approved:
   - Edit `.well-known/apple-app-site-association`
   - Replace all instances of `TEAMID` with your actual Team ID
   - Commit and push to GitHub
   - Cloudflare will auto-deploy

### Android App Not Opening

If the link opens in Chrome instead of the app:

1. **Generate SHA-256 fingerprint:**
   ```bash
   keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android | grep SHA256
   ```
2. **Update Asset Links:**
   - Edit `.well-known/assetlinks.json`
   - Replace `YOUR_SHA256_CERT_FINGERPRINT_HERE` with your actual fingerprint
   - Commit and push to GitHub
3. **Clear app data** and reinstall

### Supabase Redirect Issues

If emails still redirect to localhost:

1. Wait 5 minutes for Supabase to update cache
2. Delete test accounts and create fresh ones
3. Verify redirect URL is saved in Supabase dashboard

## Production Checklist

Before launching:

- [ ] GitHub repository created and pushed
- [ ] Cloudflare Pages deployed successfully
- [ ] Custom domain `mediaitor.junotech.app` configured
- [ ] AASA file accessible via HTTPS
- [ ] Asset Links file accessible via HTTPS
- [ ] Apple Team ID updated in AASA file
- [ ] Android SHA-256 fingerprint added to Asset Links
- [ ] Supabase redirect URL configured
- [ ] Tested on physical iOS device
- [ ] Tested on physical Android device
- [ ] Full email verification flow tested

## Updating Configuration

To update universal links configuration in the future:

1. Edit files in `.well-known/` directory
2. Commit changes: `git add . && git commit -m "Update universal links config"`
3. Push to GitHub: `git push`
4. Cloudflare Pages will automatically redeploy (takes ~30 seconds)

## Support

If you encounter issues:

1. Check Cloudflare Pages deployment logs
2. Verify DNS is pointing to Cloudflare Pages
3. Check browser console for errors
4. Review iOS and Android device logs

---

**Next Step:** Create the GitHub repository and push your code!
