# ⚡ Deploy Now - Quick Commands

## 🔴 You Just Got a Build Error - Here's the Fix!

I've created the fix files. Just run these commands:

---

## 🚀 Step 1: Push the Fix to GitHub

```bash
# Add the new fix files
git add .vercelignore vercel.json VERCEL_BUILD_FIX.md DEPLOY_NOW.md

# Commit
git commit -m "Fix Vercel build - ignore Supabase folder"

# Push (this triggers automatic redeploy on Vercel)
git push
```

**That's it!** Vercel will automatically redeploy with the fix. ✅

---

## ⏱️ Wait 2-3 Minutes

Vercel is now:
1. Pulling your updated code
2. Ignoring the `/supabase/` folder
3. Installing only frontend dependencies
4. Building your React app
5. Deploying to their CDN

---

## ✅ Check If It Worked

### Option 1: Check Vercel Dashboard

1. Go to [vercel.com/dashboard](https://vercel.com/dashboard)
2. Click on your project
3. Click "Deployments"
4. The latest one should show ✓ "Ready"
5. Click "Visit" to see your live site!

### Option 2: Check Build Logs

1. In Vercel Dashboard → Deployments
2. Click on the latest deployment
3. Scroll through the build log
4. Should see:
   ```
   ✓ Compiled successfully
   ✓ Build completed
   ```

---

## 🎉 Success!

Your site should now be live at:
```
https://your-project-name.vercel.app
```

Visit it and you should see your F1 Lanka store! 🏁

---

## 🔍 Next Steps After Deployment

### 1. Load Sample Products

Your store is empty! Visit your live site and:
- Click **"Load Sample Products"** button
- Sample F1 products will appear
- Or add products via Supabase Dashboard

### 2. Test Everything

- [ ] Products display correctly
- [ ] Search works
- [ ] Filters work (category, gender)
- [ ] Add to cart works
- [ ] Cart drawer opens
- [ ] WhatsApp checkout opens WhatsApp
- [ ] Footer shows correct contact info (+94 71 077 3717)
- [ ] Legal documents open (Privacy, Terms, etc.)

### 3. Make It Yours

**Add Real Products:**
1. Go to Supabase Dashboard
2. Table Editor → `kv_store_a97df12b`
3. Add your actual products
4. Update images, prices, descriptions

---

## ⚠️ If Build Still Fails

### Check 1: Verify .vercelignore Was Pushed

```bash
git ls-files | grep vercelignore
# Should show: .vercelignore
```

If not showing:
```bash
git add .vercelignore -f
git commit -m "Add vercelignore"
git push
```

### Check 2: Try Manual Redeploy

1. Vercel Dashboard → Deployments
2. Click "..." on latest deployment
3. Click "Redeploy"
4. Select "Use existing Build Cache" → Uncheck
5. Click "Redeploy"

### Check 3: Remove Supabase from Git

If `.vercelignore` doesn't work, use this workaround:

```bash
# Remove supabase from git tracking
git rm -r --cached supabase
echo "supabase/" >> .gitignore
git add .gitignore
git commit -m "Exclude supabase from deployment"
git push
```

---

## 🔧 Deploy Supabase Function (Separate)

Remember: The backend is deployed **separately** to Supabase!

```bash
# Make sure Supabase CLI is installed
# If not: brew install supabase/tap/supabase (Mac)
# Or: scoop install supabase (Windows)

# Login
supabase login

# Link your project (get REF from Supabase Dashboard → Settings → General)
supabase link --project-ref YOUR_PROJECT_REF

# Deploy the Edge Function
supabase functions deploy make-server-a97df12b
```

**Test it works:**
```
https://YOUR_PROJECT.supabase.co/functions/v1/make-server-a97df12b/health
```
Should return: `{"status":"ok"}`

---

## 📋 Complete Deployment Checklist

- [ ] Fixed files pushed to GitHub (`git push`)
- [ ] Vercel build succeeded (check dashboard)
- [ ] Frontend live on Vercel
- [ ] Supabase Edge Function deployed (`supabase functions deploy`)
- [ ] Database table created (from VERCEL_DEPLOYMENT_GUIDE.md)
- [ ] Sample products loaded
- [ ] WhatsApp number correct (+94 71 077 3717)
- [ ] Email correct (f1lankabusiness@gmail.com)
- [ ] Site tested on mobile
- [ ] All features working

---

## 🎯 One-Command Deploy (Future Updates)

After this initial fix, deploying updates is super easy:

```bash
# Make changes to your code
# Then:

git add .
git commit -m "Updated products"
git push

# Vercel auto-deploys in 2 minutes!
```

---

## 💬 Your Contact Info is Already Set

Your deployed site will show:
- **WhatsApp:** +94 71 077 3717
- **Email:** f1lankabusiness@gmail.com

These are in:
- `/App.tsx` (line 47)
- `/components/Footer.tsx` (multiple places)

All ready to go! ✅

---

## 🏁 You're Ready!

1. Run the git commands above
2. Wait 2-3 minutes
3. Visit your Vercel URL
4. Load sample products
5. Start selling F1 merchandise!

---

## 🆘 Need Help?

See these guides:
- **VERCEL_BUILD_FIX.md** - Detailed explanation of the fix
- **VERCEL_DEPLOYMENT_GUIDE.md** - Complete deployment guide
- **DEPLOYMENT_CHECKLIST.md** - Step-by-step checklist

Or share the new build logs if still having issues!

---

**Let's get your F1 Lanka store live! 🚀**

---

*Quick Deploy Guide - March 5, 2026*
