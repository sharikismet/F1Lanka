# F1 Lanka - Vercel Deployment Guide

## Complete Step-by-Step Deployment Instructions

This guide will walk you through deploying your F1 Lanka e-commerce website to Vercel with Supabase backend.

---

## 📋 Prerequisites

Before you begin, make sure you have:
- [ ] A GitHub account
- [ ] A Vercel account (free tier is fine) - Sign up at [vercel.com](https://vercel.com)
- [ ] A Supabase account - Sign up at [supabase.com](https://supabase.com)
- [ ] Git installed on your computer

---

## Part 1: Supabase Setup (Backend)

### Step 1: Create Supabase Project

1. Go to [supabase.com](https://supabase.com) and sign in
2. Click **"New Project"**
3. Fill in the details:
   - **Name:** F1 Lanka Store (or any name)
   - **Database Password:** Create a strong password (SAVE THIS!)
   - **Region:** Choose closest to Sri Lanka (e.g., Singapore)
4. Click **"Create new project"**
5. Wait 2-3 minutes for project to be ready

### Step 2: Get Your Supabase Credentials

1. In your Supabase project dashboard, click **"Settings"** (gear icon at bottom left)
2. Click **"API"** in the settings menu
3. **COPY AND SAVE** these values (you'll need them later):
   ```
   Project URL: https://xxxxxxxxxxxxx.supabase.co
   anon public key: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.ey...
   service_role key: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.ey... (click "Reveal" to see)
   ```
4. Go to **"Settings"** → **"Database"**
5. Scroll down to **"Connection string"** → **"URI"**
6. Copy the connection string (it looks like: `postgresql://postgres:[YOUR-PASSWORD]@...`)

### Step 3: Create the Database Table

1. In Supabase dashboard, click **"SQL Editor"** (left sidebar)
2. Click **"New Query"**
3. **Paste this SQL code:**

```sql
-- Create the key-value store table
CREATE TABLE IF NOT EXISTS kv_store_a97df12b (
  key TEXT PRIMARY KEY,
  value JSONB NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Create index for better performance
CREATE INDEX IF NOT EXISTS idx_kv_store_key ON kv_store_a97df12b(key);

-- Enable Row Level Security (RLS)
ALTER TABLE kv_store_a97df12b ENABLE ROW LEVEL SECURITY;

-- Create policy to allow service role full access
CREATE POLICY "Service role has full access" ON kv_store_a97df12b
  FOR ALL
  TO service_role
  USING (true)
  WITH CHECK (true);

-- Create policy to allow anon read access
CREATE POLICY "Anon can read" ON kv_store_a97df12b
  FOR SELECT
  TO anon
  USING (true);
```

4. Click **"Run"** (or press Ctrl+Enter)
5. You should see "Success. No rows returned" ✅

### Step 4: Deploy Edge Function (Server)

1. Install Supabase CLI on your computer:

**Windows (PowerShell):**
```powershell
scoop install supabase
```
Or download from: https://github.com/supabase/cli/releases

**Mac:**
```bash
brew install supabase/tap/supabase
```

**Linux:**
```bash
brew install supabase/tap/supabase
```

2. Open terminal/command prompt in your project folder

3. Login to Supabase:
```bash
supabase login
```
- This will open a browser window
- Click "Authorize" to connect the CLI

4. Link to your project:
```bash
supabase link --project-ref YOUR_PROJECT_REF
```
- Replace `YOUR_PROJECT_REF` with your project reference
- Find it in Supabase Dashboard → Settings → General → Reference ID
- Example: `xyzabcdefghijk`

5. Deploy the Edge Function:
```bash
supabase functions deploy make-server-a97df12b
```

6. Wait for deployment to complete (30-60 seconds)

7. **Verify it worked:**
   - Go to Supabase Dashboard → Edge Functions (left sidebar)
   - You should see `make-server-a97df12b` listed ✅

### Step 5: Test Your Server

Open this URL in your browser (replace with your project URL):
```
https://YOUR_PROJECT_ID.supabase.co/functions/v1/make-server-a97df12b/health
```

You should see:
```json
{"status":"ok","message":"F1 Lanka API Server is running"}
```

✅ **Supabase setup complete!**

---

## Part 2: Push Your Code to GitHub

### Step 1: Initialize Git (if not already done)

1. Open terminal in your project folder
2. Run:
```bash
git init
git add .
git commit -m "Initial commit - F1 Lanka Store"
```

### Step 2: Create GitHub Repository

1. Go to [github.com](https://github.com) and sign in
2. Click **"+"** icon (top right) → **"New repository"**
3. Fill in:
   - **Repository name:** `f1-lanka-store` (or any name)
   - **Description:** "F1 merchandise e-commerce website"
   - **Visibility:** Public or Private (your choice)
   - ⚠️ **DO NOT** check "Add README" or "Add .gitignore"
4. Click **"Create repository"**

### Step 3: Push Code to GitHub

1. Copy the commands from GitHub (under "…or push an existing repository")
2. In your terminal, run:
```bash
git remote add origin https://github.com/YOUR_USERNAME/f1-lanka-store.git
git branch -M main
git push -u origin main
```

✅ **Code is now on GitHub!**

---

## Part 3: Deploy to Vercel

### Step 1: Import Project to Vercel

1. Go to [vercel.com](https://vercel.com) and sign in
2. Click **"Add New..."** → **"Project"**
3. Click **"Import Git Repository"**
4. Find your `f1-lanka-store` repository
5. Click **"Import"**

### Step 2: Configure Project Settings

On the import screen:

**Framework Preset:** Vite (should auto-detect)

**Root Directory:** `./` (leave as is)

**Build Command:** 
```
npm run build
```

**Output Directory:** 
```
dist
```

**Install Command:** 
```
npm install
```

### Step 3: Add Environment Variables

⚠️ **CRITICAL STEP** - Add these environment variables:

Click **"Environment Variables"** section and add:

1. **VITE_SUPABASE_URL**
   - Value: Your Supabase Project URL (from Step 2 of Part 1)
   - Example: `https://xxxxxxxxxxxxx.supabase.co`

2. **VITE_SUPABASE_ANON_KEY**
   - Value: Your Supabase anon public key (from Step 2 of Part 1)
   - Example: `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...`

**Important:** 
- Make sure variable names start with `VITE_`
- No quotes around the values
- No spaces before or after

### Step 4: Deploy!

1. Click **"Deploy"**
2. Wait 2-3 minutes for build to complete
3. You'll see confetti 🎉 when it's done!

### Step 5: Get Your Live URL

Your site is now live at:
```
https://f1-lanka-store.vercel.app
```
(or similar URL)

Click **"Visit"** to see your live site! 🚀

---

## Part 4: Add Sample Products

Your store is live but empty! Let's add products:

### Option 1: Load Sample Data (Quick Start)

1. Visit your live Vercel site
2. You'll see "Your store is empty!"
3. Click **"Load Sample Products"** button
4. Sample F1 products will be added automatically ✅

### Option 2: Add Products Manually via Supabase

1. Go to Supabase Dashboard → SQL Editor
2. Run this SQL to add a product:

```sql
-- Insert sample product
INSERT INTO kv_store_a97df12b (key, value) VALUES 
(
  'product_1',
  '{
    "id": "1",
    "name": "Red Bull Racing Team T-Shirt",
    "price": 3500,
    "category": "tshirts",
    "gender": "men",
    "team": "Red Bull Racing",
    "image": "https://images.unsplash.com/photo-1521572163474-6864f9cf17ab?w=800",
    "description": "Official Red Bull Racing team merchandise"
  }'::jsonb
);
```

3. Refresh your website to see the product!

---

## Part 5: Managing Products

### View All Products

1. Go to Supabase Dashboard
2. Click **"Table Editor"** (left sidebar)
3. Select **"kv_store_a97df12b"** table
4. You'll see all your products

### Add New Product

1. In Table Editor, click **"Insert row"**
2. Or use SQL Editor:

```sql
INSERT INTO kv_store_a97df12b (key, value) VALUES 
(
  'product_YOUR_ID',
  '{
    "id": "YOUR_ID",
    "name": "Product Name",
    "price": 4500,
    "category": "hoodies",
    "gender": "women",
    "team": "Mercedes",
    "image": "YOUR_IMAGE_URL",
    "description": "Product description"
  }'::jsonb
);
```

### Edit Product

1. In Table Editor, click on the row
2. Click **"Edit"**
3. Modify the JSON value
4. Click **"Save"**

### Delete Product

1. In Table Editor, click on the row
2. Click **"Delete"**
3. Confirm deletion

---

## Part 6: Custom Domain (Optional)

### Add Your Own Domain

1. Buy a domain (e.g., from Namecheap, GoDaddy)
2. In Vercel Dashboard, go to your project
3. Click **"Settings"** → **"Domains"**
4. Click **"Add Domain"**
5. Enter your domain (e.g., `f1lanka.lk`)
6. Follow Vercel's instructions to update DNS records
7. Wait 24-48 hours for DNS propagation

---

## Part 7: Troubleshooting

### ❌ Site shows "Server Connection Error"

**Solution:**
1. Check if Edge Function is deployed:
   - Go to Supabase → Edge Functions
   - Should see `make-server-a97df12b`
2. Verify environment variables in Vercel:
   - Vercel Dashboard → Settings → Environment Variables
   - Check `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY`
3. Redeploy:
   - Vercel Dashboard → Deployments → Click "..." → Redeploy

### ❌ Products not loading

**Solution:**
1. Check database table exists:
   - Supabase → Table Editor → Look for `kv_store_a97df12b`
2. Run the table creation SQL again (Part 1, Step 3)
3. Check browser console for errors (F12 key)

### ❌ WhatsApp checkout not working

**Solution:**
1. Verify WhatsApp number in `/App.tsx`:
   ```tsx
   const WHATSAPP_NUMBER = '94710773717'; // Should be your number
   ```
2. Format: Country code + number (no + or spaces)
3. Redeploy after changing

### ❌ Build fails on Vercel

**Solution:**
1. Check build logs in Vercel Dashboard
2. Common fixes:
   ```bash
   # Locally test build
   npm install
   npm run build
   ```
3. If errors, check:
   - All imports are correct
   - No missing dependencies
   - No TypeScript errors

---

## Part 8: Updating Your Site

### Make Changes

1. Edit files locally in your project
2. Test locally:
   ```bash
   npm run dev
   ```
3. Commit changes:
   ```bash
   git add .
   git commit -m "Description of changes"
   git push
   ```
4. Vercel automatically deploys! ⚡
5. Check deployment in Vercel Dashboard

---

## 🎯 Quick Reference

### Important URLs

- **Live Site:** `https://your-site.vercel.app`
- **Vercel Dashboard:** https://vercel.com/dashboard
- **Supabase Dashboard:** https://app.supabase.com
- **GitHub Repo:** `https://github.com/YOUR_USERNAME/f1-lanka-store`

### Important Commands

```bash
# Local development
npm install
npm run dev

# Deploy Edge Function
supabase functions deploy make-server-a97df12b

# Push to GitHub (triggers Vercel deploy)
git add .
git commit -m "Update message"
git push
```

### Contact Information in Your Site

Currently set to:
- **WhatsApp:** +94 71 077 3717
- **Email:** f1lankabusiness@gmail.com

To change: Edit `/App.tsx` and `/components/Footer.tsx`

---

## ✅ Deployment Checklist

Use this checklist to ensure everything is set up:

- [ ] Supabase project created
- [ ] Database table `kv_store_a97df12b` created
- [ ] Edge Function deployed to Supabase
- [ ] Code pushed to GitHub
- [ ] Vercel project created and connected to GitHub
- [ ] Environment variables added to Vercel
- [ ] Site deployed successfully
- [ ] Sample products loaded
- [ ] WhatsApp checkout tested
- [ ] Footer contact info verified

---

## 🎉 You're Live!

Congratulations! Your F1 Lanka e-commerce store is now live and accessible worldwide!

**Next Steps:**
1. Add your actual product images
2. Update product descriptions
3. Test all checkout flows
4. Share your site link!
5. Set up analytics (optional)

**Need Help?**
- Vercel Discord: https://vercel.com/discord
- Supabase Discord: https://discord.supabase.com
- Check browser console (F12) for errors

---

## 📊 Cost Breakdown

**Free Forever:**
- Vercel Hosting (100GB bandwidth/month)
- Supabase Database (500MB storage)
- Supabase Edge Functions (500K invocations/month)

**This is more than enough for:**
- 1000+ products
- Thousands of monthly visitors
- Professional e-commerce store

**Perfect for starting your business with ZERO hosting costs!** 💰

---

*Last Updated: March 5, 2026*
