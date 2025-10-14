# MediAItor Universal Links - Pending Items

## ⏳ Items to Complete

### 1. iOS Universal Links - Apple Team ID

**File:** `.well-known/apple-app-site-association`
**Status:** ⏳ Waiting for Apple Developer Account Approval
**Current Value:** `TEAMID` (placeholder)
**Required Action:** Replace `TEAMID` with your actual Apple Team ID

**Steps:**
1. Wait for Apple Developer account approval
2. Login to https://developer.apple.com
3. Go to Account → Membership
4. Copy your Team ID (format: `ABC123DEFG` - 10 characters)
5. Replace all instances of `TEAMID` in `.well-known/apple-app-site-association` with your actual Team ID
6. Commit and push changes to GitHub
7. Cloudflare Pages will auto-deploy the update

**Example:**
```json
// Before:
"appID": "TEAMID.com.junotech.mediaitor"

// After (with Team ID ABC123DEFG):
"appID": "ABC123DEFG.com.junotech.mediaitor"
```

---

### 2. Android App Links - SHA-256 Certificate Fingerprint

**File:** `.well-known/assetlinks.json`
**Status:** ⏳ Needs to be generated
**Current Value:** `YOUR_SHA256_CERT_FINGERPRINT_HERE` (placeholder)
**Required Action:** Generate and replace with your app's SHA-256 fingerprint

**Steps:**

#### For Debug Build (Development):
```bash
# Navigate to your Android project
cd ~/Development/mediaitor/android

# Generate debug SHA-256 fingerprint
keytool -list -v -keystore ~/.android/debug.keystore \
  -alias androiddebugkey -storepass android -keypass android | \
  grep -A 1 "SHA256"

# Copy the SHA-256 value (format: XX:XX:XX:... 32 pairs of hex digits)
```

#### For Release Build (Production):
```bash
# You'll need to create a release keystore first
# Then run:
keytool -list -v -keystore /path/to/your-release-key.keystore \
  -alias your-alias | grep -A 1 "SHA256"
```

#### Update the File:
1. Copy the SHA-256 fingerprint (remove colons for the JSON format)
2. Open `.well-known/assetlinks.json`
3. Replace `YOUR_SHA256_CERT_FINGERPRINT_HERE` with your fingerprint
4. Commit and push changes
5. Cloudflare Pages will auto-deploy

**Example:**
```json
// Before:
"sha256_cert_fingerprints": ["YOUR_SHA256_CERT_FINGERPRINT_HERE"]

// After:
"sha256_cert_fingerprints": ["14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"]
```

---

### 3. Optional: Update Supabase Site URL

**Location:** Supabase Dashboard → Authentication → URL Configuration
**Status:** ✅ Working (but can be optimized)
**Current Value:** `https://mediaitor.junotech.app`
**Recommended Value:** `https://mediaitor.junotech.app/auth/callback`

**Why:**
- Current setup works fine (email links go to root, then JavaScript redirects)
- Recommended setup is more direct (email links go straight to callback endpoint)
- Not urgent, both work correctly

**Steps (if you want to optimize):**
1. Login to Supabase Dashboard
2. Go to Project → Authentication → URL Configuration
3. Update "Site URL" to `https://mediaitor.junotech.app/auth/callback`
4. Save changes

---

## ✅ Completed Items

- ✅ Universal links files created and deployed to Cloudflare Pages
- ✅ GitHub repository configured (https://github.com/winte010/mediaitor-universal-links)
- ✅ Beautiful email verification landing page with auto-open functionality
- ✅ iOS app configured with associated domains
- ✅ Android app configured with intent filters
- ✅ Cloudflare Pages connected to custom domain (mediaitor.junotech.app)

---

## 📝 Testing Checklist

Once you complete the above items, test with these steps:

### iOS Testing:
1. Sign up a new user in the app
2. Click email verification link on a real iPhone (not simulator)
3. Link should open directly in the app (no Safari intermediate step)
4. User should be marked as verified in database

### Android Testing:
1. Build release APK with your release keystore
2. Sign up a new user
3. Click email verification link on Android device
4. Link should open directly in the app
5. User should be marked as verified

### Fallback Testing:
1. Click link on desktop/device without app installed
2. Should see beautiful landing page at mediaitor.junotech.app
3. "Open MediAItor App" button should attempt to open app
4. App store links should be visible as fallback

---

## 🔗 Useful Links

- Apple Developer Portal: https://developer.apple.com
- Android Keystore Documentation: https://developer.android.com/studio/publish/app-signing
- Universal Links Testing Tool: https://branch.io/resources/aasa-validator/
- Cloudflare Pages Dashboard: https://dash.cloudflare.com
- GitHub Repo: https://github.com/winte010/mediaitor-universal-links

---

**Last Updated:** October 14, 2025
**Status:** Deployed to production, pending Team ID and SHA-256 fingerprint
