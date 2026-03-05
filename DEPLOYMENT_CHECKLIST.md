# 🚀 Quick Deployment Checklist

## Before You Start
- [ ] GitHub account created
- [ ] Vercel account created  
- [ ] Supabase account created
- [ ] Git installed on computer

---

## 1️⃣ Supabase Setup (15 minutes)

### Create Project
- [ ] Go to supabase.com
- [ ] Click "New Project"
- [ ] Name: F1 Lanka Store
- [ ] Choose region: Singapore
- [ ] Save database password ⚠️

### Copy Credentials
- [ ] Settings → API
- [ ] Copy Project URL
- [ ] Copy anon public key
- [ ] Copy service_role key (click Reveal)
- [ ] Settings → Database → Copy connection string

### Create Database Table
- [ ] SQL Editor → New Query
- [ ] Paste table creation SQL (from guide)
- [ ] Click Run
- [ ] See "Success" message ✅

### Deploy Edge Function
- [ ] Install Supabase CLI
- [ ] Open terminal in project folder
- [ ] Run: `supabase login`
- [ ] Run: `supabase link --project-ref YOUR_REF`
- [ ] Run: `supabase functions deploy make-server-a97df12b`
- [ ] Verify in Supabase Dashboard → Edge Functions

### Test Server
- [ ] Visit: `https://YOUR_PROJECT.supabase.co/functions/v1/make-server-a97df12b/health`
- [ ] See: `{"status":"ok"}`

---

## 2️⃣ GitHub Push (5 minutes)

### Initialize Git
- [ ] Open terminal in project folder
- [ ] Run: `git init`
- [ ] Run: `git add .`
- [ ] Run: `git commit -m "Initial commit"`

### Create GitHub Repo
- [ ] Go to github.com
- [ ] Click + → New repository
- [ ] Name: f1-lanka-store
- [ ] Click Create (don't add README)

### Push Code
- [ ] Copy commands from GitHub
- [ ] Run: `git remote add origin YOUR_REPO_URL`
- [ ] Run: `git branch -M main`
- [ ] Run: `git push -u origin main`
- [ ] Refresh GitHub to see code ✅

---

## 3️⃣ Vercel Deployment (10 minutes)

### Import Project
- [ ] Go to vercel.com
- [ ] Click "Add New..." → Project
- [ ] Click "Import Git Repository"
- [ ] Select f1-lanka-store repo
- [ ] Click Import

### Configure Settings
- [ ] Framework: Vite (auto-detected)
- [ ] Build Command: `npm run build`
- [ ] Output Directory: `dist`

### Add Environment Variables ⚠️ CRITICAL
- [ ] Click "Environment Variables"
- [ ] Add: `VITE_SUPABASE_URL` = Your Supabase URL
- [ ] Add: `VITE_SUPABASE_ANON_KEY` = Your anon key
- [ ] Double-check spelling (must start with VITE_)

### Deploy
- [ ] Click "Deploy"
- [ ] Wait 2-3 minutes
- [ ] See confetti 🎉
- [ ] Click "Visit" to see live site

---

## 4️⃣ Add Products (5 minutes)

### Quick Start
- [ ] Visit your live site
- [ ] Click "Load Sample Products"
- [ ] See products appear ✅

### Or Add Manually
- [ ] Supabase → SQL Editor
- [ ] Paste product INSERT SQL
- [ ] Click Run
- [ ] Refresh website

---

## 5️⃣ Final Checks (5 minutes)

### Test Features
- [ ] Products load on homepage
- [ ] Search works
- [ ] Filters work (category, gender)
- [ ] Click product to see details
- [ ] Add to cart
- [ ] Open cart drawer
- [ ] Click "Checkout via WhatsApp"
- [ ] WhatsApp opens with order details
- [ ] Footer links work
- [ ] Legal documents open

### Verify Contact Info
- [ ] WhatsApp: +94 71 077 3717 ✅
- [ ] Email: f1lankabusiness@gmail.com ✅
- [ ] Check footer
- [ ] Check Privacy Policy
- [ ] Check Terms & Conditions

---

## ✅ Success Criteria

Your site is ready when:
- ✅ Site loads at Vercel URL
- ✅ No "Server Connection Error"
- ✅ Products display correctly
- ✅ WhatsApp checkout works
- ✅ All filters and search work
- ✅ Mobile responsive (test on phone)
- ✅ Footer contact info correct

---

## 🆘 Quick Fixes

### Server Connection Error
```bash
# Redeploy Edge Function
supabase functions deploy make-server-a97df12b

# Check Vercel environment variables
# Settings → Environment Variables
```

### Products Not Loading
```sql
-- Re-create table in Supabase SQL Editor
CREATE TABLE IF NOT EXISTS kv_store_a97df12b (
  key TEXT PRIMARY KEY,
  value JSONB NOT NULL
);
```

### Site Won't Build
```bash
# Test locally first
npm install
npm run build

# If works locally, push to GitHub
git push
```

---

## 📋 Information Sheet

Keep this handy:

**Supabase**
- Project URL: _________________________
- Anon Key: _________________________
- Service Role Key: _________________________

**GitHub**
- Repo URL: _________________________

**Vercel**
- Live URL: _________________________

**Contact**
- WhatsApp: +94 71 077 3717
- Email: f1lankabusiness@gmail.com

---

## 🎯 Total Time: ~40 minutes

1. Supabase Setup: 15 min
2. GitHub Push: 5 min
3. Vercel Deploy: 10 min
4. Add Products: 5 min
5. Final Testing: 5 min

---

## 🎉 Done!

Your F1 Lanka store is LIVE! 🏁

Share your link:
`https://your-site.vercel.app`

**Next Actions:**
1. ✅ Test on mobile
2. ✅ Add real product photos
3. ✅ Update product prices
4. ✅ Share with customers!

---

*See VERCEL_DEPLOYMENT_GUIDE.md for detailed instructions*
