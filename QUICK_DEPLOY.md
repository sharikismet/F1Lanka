# 🎯 5-Minute Quick Start - Deploy to Vercel

**Already have Supabase and GitHub set up? Use this express guide!**

---

## ⚡ Express Deployment (5 steps)

### 1️⃣ Push to GitHub (2 min)

```bash
# In your project folder terminal
git init
git add .
git commit -m "F1 Lanka Store"
git remote add origin https://github.com/YOUR_USERNAME/f1-lanka-store.git
git push -u origin main
```

---

### 2️⃣ Deploy Supabase Function (1 min)

```bash
# Login once
supabase login

# Link your project (get REF from Supabase → Settings → General)
supabase link --project-ref YOUR_PROJECT_REF

# Deploy
supabase functions deploy make-server-a97df12b
```

---

### 3️⃣ Create Database Table (1 min)

**Supabase Dashboard → SQL Editor → New Query → Paste & Run:**

```sql
CREATE TABLE IF NOT EXISTS kv_store_a97df12b (
  key TEXT PRIMARY KEY,
  value JSONB NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE INDEX IF NOT EXISTS idx_kv_store_key ON kv_store_a97df12b(key);

ALTER TABLE kv_store_a97df12b ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Service role has full access" ON kv_store_a97df12b
  FOR ALL TO service_role USING (true) WITH CHECK (true);

CREATE POLICY "Anon can read" ON kv_store_a97df12b
  FOR SELECT TO anon USING (true);
```

---

### 4️⃣ Deploy to Vercel (2 min)

1. **Go to:** [vercel.com/new](https://vercel.com/new)
2. **Import** your GitHub repository
3. **Framework:** Vite (auto-detected)
4. **Add Environment Variables:**
   - `VITE_SUPABASE_URL` = `https://yourproject.supabase.co`
   - `VITE_SUPABASE_ANON_KEY` = `eyJhbGci...` (from Supabase → Settings → API)
5. **Click Deploy** 🚀

---

### 5️⃣ Load Products (30 sec)

Visit your live site → Click **"Load Sample Products"** → Done! ✅

---

## 🎉 That's It!

Your F1 Lanka store is LIVE!

**Test it:**
- Products load ✅
- Search works ✅
- Add to cart ✅
- WhatsApp checkout ✅

---

## 📱 Your Contact Info

Currently set to:
- **WhatsApp:** +94 71 077 3717
- **Email:** f1lankabusiness@gmail.com

All set in:
- `/App.tsx` (line 47)
- `/components/Footer.tsx`

---

## 🔄 Make Updates

```bash
# Edit your files
# Then push to GitHub
git add .
git commit -m "Updated products"
git push

# Vercel auto-deploys! ⚡
```

---

## 🆘 Problems?

### Server Connection Error
```bash
supabase functions deploy make-server-a97df12b
```

### Products Not Loading
- Check Supabase table exists
- Run table SQL again
- Click "Load Sample Products"

### Build Fails
- Check environment variables in Vercel
- Variable names must be exact:
  - `VITE_SUPABASE_URL`
  - `VITE_SUPABASE_ANON_KEY`

---

## 📚 Need More Details?

See the full guides:
- **VERCEL_DEPLOYMENT_GUIDE.md** - Complete step-by-step
- **DEPLOYMENT_CHECKLIST.md** - Detailed checklist
- **ENVIRONMENT_VARIABLES.md** - Env vars explained

---

## ⏱️ Time Breakdown

- GitHub Push: 2 min
- Supabase Function: 1 min
- Database Setup: 1 min  
- Vercel Deploy: 2 min
- Load Products: 30 sec

**Total: ~6.5 minutes** ⚡

---

## 🎯 Success Checklist

- [ ] Code on GitHub
- [ ] Edge Function deployed
- [ ] Database table created
- [ ] Site live on Vercel
- [ ] Products loaded
- [ ] WhatsApp works

---

## 🌟 Pro Tips

1. **Custom Domain:** Vercel Settings → Domains → Add your domain
2. **Analytics:** Vercel automatically tracks visitors
3. **Preview Deploys:** Every Git push creates a preview URL
4. **Rollback:** Vercel Deployments → Click old version → Promote

---

## 💰 Cost

**$0 / month** 

Free includes:
- Unlimited sites
- 100GB bandwidth/month
- SSL certificate
- Global CDN
- Automatic deployments

Perfect for starting! 🚀

---

**Ready to sell F1 merchandise! 🏁**

Share your link and start taking orders via WhatsApp!

---

*Last Updated: March 5, 2026*
*Your F1 Lanka E-commerce Store*
