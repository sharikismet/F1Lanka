# 🔧 Vercel Build Error Fix

## Error You're Seeing

```
npm error 404 Not Found - GET https://registry.npmjs.org/@jsr%2fsupabase__supabase-js
npm error 404  The requested resource '@jsr/supabase__supabase-js@^2.49.8' could not be found
Error: Command "npm install" exited with 1
```

## Why This Happens

Your project has two parts:
1. **Frontend** (React app) → Deploys to Vercel
2. **Backend** (Supabase Edge Functions) → Deploys to Supabase

The Supabase Edge Functions use **JSR imports** (JavaScript Registry) which are for Deno, not npm. Vercel is scanning your entire project including the `/supabase/` folder and trying to install JSR packages as npm packages, which fails.

## ✅ Solution

I've already created two files to fix this:

### 1. `.vercelignore` File
This tells Vercel to ignore the Supabase folder during deployment.

**Already created!** The file contains:
```
supabase/
```

### 2. `vercel.json` File  
This configures Vercel's build process properly.

**Already created!** The file contains proper build configuration.

## 🚀 What You Need to Do

### Option A: Re-push to GitHub (Recommended)

Since I just created the `.vercelignore` and `vercel.json` files, you need to push these changes:

```bash
# In your project folder
git add .vercelignore vercel.json
git commit -m "Fix Vercel build - ignore Supabase folder"
git push
```

Vercel will automatically redeploy with the fix! ✅

---

### Option B: If Still Having Issues

If the error persists, follow this workaround:

#### Step 1: Temporarily Remove Supabase Folder

```bash
# In your project folder

# Create a backup of the supabase folder
cp -r supabase ../supabase-backup

# Remove from git tracking (but keep locally)
git rm -r --cached supabase
echo "supabase/" >> .gitignore

# Commit and push
git add .gitignore
git commit -m "Exclude Supabase functions from frontend deployment"
git push
```

#### Step 2: Deploy Supabase Functions Separately

The Supabase Edge Function is deployed **separately** to Supabase, not to Vercel:

```bash
# Make sure you're in your project root
cd path/to/your/project

# Restore supabase folder locally (if you removed it)
cp -r ../supabase-backup ./supabase

# Deploy to Supabase (not Vercel)
supabase functions deploy make-server-a97df12b
```

---

## 📋 Understanding the Two-Part Deployment

Your F1 Lanka store has a **two-tier architecture**:

```
┌─────────────────┐
│  VERCEL         │  Frontend (React)
│  - Product UI   │  ← git push deploys here
│  - Cart         │
│  - Filters      │
└────────┬────────┘
         │ API calls
         ▼
┌─────────────────┐
│  SUPABASE       │  Backend (Edge Functions + Database)
│  - API Server   │  ← supabase functions deploy
│  - Database     │
└─────────────────┘
```

### What Goes Where

**Vercel (Frontend):**
- `/App.tsx`
- `/components/`
- `/lib/`
- `/styles/`
- Everything EXCEPT `/supabase/`

**Supabase (Backend):**
- `/supabase/functions/server/` folder
- Deployed with: `supabase functions deploy make-server-a97df12b`
- **NOT** deployed via git push

---

## ✅ Correct Deployment Process

### 1. Deploy Backend to Supabase (First Time Only)

```bash
# Login to Supabase CLI
supabase login

# Link to your project
supabase link --project-ref YOUR_PROJECT_REF

# Deploy Edge Function
supabase functions deploy make-server-a97df12b
```

### 2. Deploy Frontend to Vercel (Every Update)

```bash
# Push changes to GitHub
git add .
git commit -m "Your changes"
git push

# Vercel automatically deploys!
```

---

## 🔍 Verify the Fix

After pushing the `.vercelignore` file:

1. **Check Vercel Build Logs**
   - Go to Vercel Dashboard → Deployments
   - Click on the latest deployment
   - Check the build logs

2. **Should See Success**
   ```
   ✓ Build completed successfully
   ✓ Deployment ready
   ```

3. **Visit Your Site**
   - Click "Visit" in Vercel
   - Site should load without errors

---

## 🆘 Still Having Issues?

### Check 1: Verify Files Were Pushed

```bash
# Check if .vercelignore exists in GitHub
git ls-files | grep vercelignore

# Should output: .vercelignore
```

### Check 2: Verify Supabase Function is Deployed

```bash
# Test the Edge Function directly
curl https://YOUR_PROJECT.supabase.co/functions/v1/make-server-a97df12b/health

# Should return: {"status":"ok","message":"F1 Lanka API Server is running"}
```

### Check 3: Clear Vercel Cache

1. Go to Vercel Dashboard
2. Settings → General
3. Scroll to "Deployment Protection"
4. Click "Clear Cache"
5. Go to Deployments → Click "..." → Redeploy

---

## 📝 Summary of Changes

I've created these files to fix the build error:

1. **`.vercelignore`** - Tells Vercel to ignore Supabase folder
2. **`vercel.json`** - Configures Vercel build properly
3. **This guide** - Explains the fix

**Action Required:**
```bash
git add .vercelignore vercel.json VERCEL_BUILD_FIX.md
git commit -m "Fix Vercel build error"
git push
```

That's it! Your next deployment should succeed! ✅

---

## 🎯 Quick Fix Checklist

- [ ] `.vercelignore` file created (already done)
- [ ] `vercel.json` file created (already done)
- [ ] Push changes to GitHub
- [ ] Vercel automatically redeploys
- [ ] Build succeeds
- [ ] Site loads correctly
- [ ] Supabase Edge Function deployed separately
- [ ] Test: Products load on site
- [ ] Test: WhatsApp checkout works

---

## 💡 Pro Tip

**Keep Supabase Functions Separate:**
- Supabase Edge Functions run on **Deno** (not Node.js)
- They use JSR imports, which are Deno-specific
- Vercel runs on **Node.js** and uses npm
- That's why we deploy them separately!

Frontend (Vercel) ← `git push`
Backend (Supabase) ← `supabase functions deploy`

---

**Your build should work now!** 🎉

If you continue to have issues, share the new build logs and I'll help further.

---

*Last Updated: March 5, 2026*
