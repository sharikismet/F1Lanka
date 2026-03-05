# Environment Variables Configuration

## For Vercel Deployment

When deploying to Vercel, you MUST add these environment variables in the Vercel dashboard.

---

## 📍 Where to Add These

1. Go to your Vercel project dashboard
2. Click **"Settings"**
3. Click **"Environment Variables"** in the left sidebar
4. Add each variable below

---

## 🔑 Required Environment Variables

### 1. VITE_SUPABASE_URL

**Name:**
```
VITE_SUPABASE_URL
```

**Value:** Your Supabase Project URL

**Where to find it:**
- Supabase Dashboard → Settings → API → Project URL

**Example:**
```
https://abcdefghijklmnop.supabase.co
```

**Important:**
- Must include `https://`
- No trailing slash at the end
- Must start with `VITE_` for Vite to expose it to the browser

---

### 2. VITE_SUPABASE_ANON_KEY

**Name:**
```
VITE_SUPABASE_ANON_KEY
```

**Value:** Your Supabase Anon Public Key

**Where to find it:**
- Supabase Dashboard → Settings → API → Project API keys → anon public

**Example:**
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImFiY2RlZmdoaWprbG1ub3AiLCJyb2xlIjoiYW5vbiIsImlhdCI6MTYxNjE2MTYxNiwiZXhwIjoxOTMxNzM3NjE2fQ.xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

**Important:**
- Very long string (starts with `eyJ`)
- Copy the entire key
- This is safe to expose in the browser (it's the "public" key)

---

## 🚫 DO NOT Add These to Vercel

These are for Supabase Edge Functions ONLY (not for frontend):

❌ **SUPABASE_SERVICE_ROLE_KEY** - This is secret and only used by the Edge Function
❌ **SUPABASE_DB_URL** - Only used by the Edge Function

The Edge Function will automatically have access to these through Supabase's environment.

---

## ✅ Verification Checklist

After adding environment variables:

- [ ] Variable name is exactly `VITE_SUPABASE_URL` (case-sensitive)
- [ ] Variable name is exactly `VITE_SUPABASE_ANON_KEY` (case-sensitive)
- [ ] Both start with `VITE_` prefix
- [ ] No quotes around the values
- [ ] No extra spaces before or after values
- [ ] URL includes `https://`
- [ ] Anon key is the complete long string

---

## 📝 Step-by-Step in Vercel

### Adding First Variable (VITE_SUPABASE_URL)

1. In Vercel project settings, find "Environment Variables" section
2. In the "Key" field, type: `VITE_SUPABASE_URL`
3. In the "Value" field, paste your Supabase URL: `https://yourproject.supabase.co`
4. Select all environments: Production, Preview, Development
5. Click "Add"

### Adding Second Variable (VITE_SUPABASE_ANON_KEY)

1. Click "Add Another" or the + button
2. In the "Key" field, type: `VITE_SUPABASE_ANON_KEY`
3. In the "Value" field, paste your complete anon key (the long eyJ... string)
4. Select all environments: Production, Preview, Development
5. Click "Add"

### Save and Redeploy

1. Click "Save" at the bottom
2. Go to "Deployments" tab
3. Click the "..." menu on the latest deployment
4. Click "Redeploy"
5. Wait for deployment to complete

---

## 🔍 How to Get Your Supabase Keys

### Visual Guide:

**Step 1:** Go to your Supabase project dashboard

**Step 2:** Click the ⚙️ "Settings" icon in the bottom left

**Step 3:** Click "API" in the settings menu

**Step 4:** You'll see a page with "Project API keys"

**Step 5:** Copy these two values:

```
Configuration
├── Project URL: https://xxxxx.supabase.co  ← Copy this
└── Project API keys
    ├── anon public: eyJhbG... ← Copy this
    └── service_role: eyJhbG... ← DON'T use this in Vercel
```

---

## 🧪 Test Your Configuration

After deployment, your site should:

1. ✅ Load without errors
2. ✅ Show products (or "empty store" message)
3. ✅ NOT show "Server Connection Error"

If you see errors:
1. Check browser console (F12)
2. Verify environment variables spelling
3. Verify values are correct
4. Redeploy after fixing

---

## 📱 Local Development vs Production

### For Local Development (.env file)

If you want to run locally, create a `.env` file in your project root:

```env
VITE_SUPABASE_URL=https://yourproject.supabase.co
VITE_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

Then run:
```bash
npm run dev
```

### For Production (Vercel)

Use the Vercel dashboard to add variables (as described above).

**Important:** Don't commit the `.env` file to GitHub! It's already in `.gitignore`.

---

## 🔐 Security Notes

✅ **Safe to expose in browser:**
- `VITE_SUPABASE_URL` - Your project URL
- `VITE_SUPABASE_ANON_KEY` - The "anon public" key

❌ **NEVER expose in browser:**
- `SUPABASE_SERVICE_ROLE_KEY` - Admin access key
- Database passwords
- API keys from other services

The `VITE_` prefix means these variables will be embedded in your frontend JavaScript bundle. Only use public, non-sensitive keys with this prefix.

---

## 🆘 Troubleshooting

### Error: "Cannot read environment variables"

**Fix:** 
- Make sure variables start with `VITE_`
- Redeploy after adding variables

### Error: "Server Connection Error"

**Fix:**
1. Check Edge Function is deployed in Supabase
2. Verify `VITE_SUPABASE_URL` is correct
3. Check for typos in variable names

### Error: "Unauthorized" or "Invalid API key"

**Fix:**
- Verify `VITE_SUPABASE_ANON_KEY` is the complete key
- Make sure you copied the "anon public" key, not "service_role"
- Check for extra spaces or characters

---

## 📋 Quick Copy Template

Use this template to keep your credentials organized:

```
=================================
F1 LANKA - SUPABASE CREDENTIALS
=================================

Project Name: F1 Lanka Store
Project ID: _______________

VITE_SUPABASE_URL:
https://_____________________________.supabase.co

VITE_SUPABASE_ANON_KEY:
eyJ_________________________________________________
____________________________________________________
____________________________________________________

Database Password: ____________________

Date Setup: March 5, 2026
```

---

## ✅ Final Check

Before closing this guide, confirm:

- [ ] Both environment variables added to Vercel
- [ ] Variable names spelled correctly
- [ ] Values copied completely (no truncation)
- [ ] Redeployed after adding variables
- [ ] Site loads without errors
- [ ] Products can be loaded/displayed

---

**You're all set!** 🎉

Your environment variables are configured correctly and your F1 Lanka store should be running smoothly on Vercel.

---

*For full deployment guide, see VERCEL_DEPLOYMENT_GUIDE.md*
