# 🏗️ F1 Lanka - Architecture Overview

## System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                         CUSTOMERS                            │
│                    (Browse & Purchase)                       │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                    VERCEL (Frontend)                         │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  React App (Vite)                                    │   │
│  │  - Product Display                                   │   │
│  │  - Shopping Cart                                     │   │
│  │  - Filters & Search                                  │   │
│  │  - WhatsApp Integration                              │   │
│  └──────────────────────────────────────────────────────┘   │
│  URL: https://f1-lanka-store.vercel.app                     │
└────────────────────────┬────────────────────────────────────┘
                         │
                         │ API Calls
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                 SUPABASE (Backend)                           │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  Edge Function (Hono Server)                         │   │
│  │  - /health                                           │   │
│  │  - /products                                         │   │
│  │  - /orders                                           │   │
│  │  Route: /make-server-a97df12b/*                     │   │
│  └──────────────────────────────────────────────────────┘   │
│                         │                                    │
│                         ▼                                    │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  PostgreSQL Database                                 │   │
│  │  Table: kv_store_a97df12b                           │   │
│  │  - Products data                                     │   │
│  │  - Orders data                                       │   │
│  └──────────────────────────────────────────────────────┘   │
└────────────────────────┬────────────────────────────────────┘
                         │
                         │ Order placed
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                      WHATSAPP                                │
│  Customer sends order details to: +94 71 077 3717          │
└─────────────────────────────────────────────────────────────┘
```

---

## Data Flow

### 1. Customer Browses Products

```
Customer → Vercel (React App) → Supabase Edge Function → Database
                                                          ↓
Customer ← Product Data ←─────────────────────────────────┘
```

### 2. Customer Makes Purchase

```
Customer adds to cart → Shopping Cart (Frontend State)
                              ↓
Customer clicks "Checkout via WhatsApp"
                              ↓
WhatsApp opens with pre-filled message
                              ↓
Order sent to +94 71 077 3717
                              ↓
You receive order → Confirm → Process → Ship
```

---

## File Structure

```
f1-lanka-store/
├── src/
│   ├── App.tsx                    # Main app component
│   ├── components/
│   │   ├── Header.tsx             # Navigation & search
│   │   ├── Footer.tsx             # Contact & legal docs
│   │   ├── ProductCard.tsx        # Product display card
│   │   ├── ProductFilters.tsx     # Category & gender filters
│   │   ├── CartDrawer.tsx         # Shopping cart
│   │   ├── TeamSelector.tsx       # F1 team filter
│   │   └── ui/                    # ShadCN components
│   ├── lib/
│   │   ├── api.ts                 # API functions
│   │   ├── CartContext.tsx        # Cart state management
│   │   └── supabase.ts            # Supabase client
│   └── styles/
│       └── globals.css            # Global styles
│
├── supabase/
│   └── functions/
│       └── server/
│           ├── index.tsx          # Edge Function (API server)
│           └── kv_store.tsx       # Database utilities
│
├── public/                        # Static assets
├── package.json                   # Dependencies
└── vite.config.ts                 # Vite configuration
```

---

## Technology Stack

### Frontend (Vercel)
- **React 18** - UI framework
- **Vite** - Build tool & dev server
- **TypeScript** - Type safety
- **Tailwind CSS** - Styling
- **ShadCN UI** - Component library
- **Lucide React** - Icons

### Backend (Supabase)
- **Supabase Edge Functions** - Serverless API (Deno runtime)
- **Hono** - Web framework for Edge Functions
- **PostgreSQL** - Database
- **Row Level Security** - Data protection

### Communication
- **WhatsApp Business API** - Order processing
- **Email** - Customer support (f1lankabusiness@gmail.com)

---

## Environment Variables

### Frontend (Vercel) - Public
```env
VITE_SUPABASE_URL=https://xxxxx.supabase.co
VITE_SUPABASE_ANON_KEY=eyJhbGci...
```
*(Safe to expose - used in browser)*

### Backend (Supabase) - Private
```env
SUPABASE_URL=https://xxxxx.supabase.co
SUPABASE_ANON_KEY=eyJhbGci...
SUPABASE_SERVICE_ROLE_KEY=eyJhbGci...  # Admin access
SUPABASE_DB_URL=postgresql://...        # Database connection
```
*(Automatically available to Edge Functions)*

---

## Database Schema

### Table: `kv_store_a97df12b`

```sql
┌──────────────┬──────────────────────────┬─────────────┐
│ Column       │ Type                     │ Description │
├──────────────┼──────────────────────────┼─────────────┤
│ key          │ TEXT (Primary Key)       │ Unique ID   │
│ value        │ JSONB                    │ Data        │
│ created_at   │ TIMESTAMP WITH TIME ZONE │ Created     │
│ updated_at   │ TIMESTAMP WITH TIME ZONE │ Updated     │
└──────────────┴──────────────────────────┴─────────────┘
```

### Product Data Structure (JSONB)
```json
{
  "id": "1",
  "name": "Red Bull Racing T-Shirt",
  "price": 3500,
  "category": "tshirts",
  "gender": "men",
  "team": "Red Bull Racing",
  "image": "https://...",
  "description": "Official merchandise",
  "sizes": ["S", "M", "L", "XL"],
  "stock": 50
}
```

### Order Data Structure (JSONB)
```json
{
  "orderId": "order_123",
  "customerName": "John Doe",
  "customerPhone": "+94712345678",
  "customerEmail": "john@example.com",
  "items": [...],
  "totalAmount": 7000,
  "status": "pending",
  "createdAt": "2026-03-05T10:30:00Z"
}
```

---

## API Endpoints

### Base URL
```
https://YOUR_PROJECT.supabase.co/functions/v1/make-server-a97df12b
```

### Endpoints

**1. Health Check**
```http
GET /health
Response: {"status":"ok","message":"F1 Lanka API Server is running"}
```

**2. Get All Products**
```http
GET /products
Response: [{"id":"1","name":"...","price":3500,...}]
```

**3. Get Single Product**
```http
GET /products/:id
Response: {"id":"1","name":"...","price":3500,...}
```

**4. Create Order**
```http
POST /orders
Body: {"customerName":"...","items":[...],"totalAmount":7000}
Response: {"orderId":"order_123","status":"created"}
```

**5. Initialize Sample Data**
```http
POST /init-sample-data
Response: {"success":true,"message":"Sample data created"}
```

---

## Deployment Process

```
┌─────────────┐
│  Local Dev  │  npm run dev (localhost:5173)
└──────┬──────┘
       │ git push
       ▼
┌─────────────┐
│   GitHub    │  Code repository
└──────┬──────┘
       │ Auto-deploy (Vercel webhook)
       ▼
┌─────────────┐
│   Vercel    │  Build & Deploy
│             │  1. npm install
│             │  2. npm run build
│             │  3. Deploy to CDN
└──────┬──────┘
       │
       ▼
┌─────────────┐
│    LIVE!    │  https://f1-lanka-store.vercel.app
└─────────────┘
```

### Edge Function Deployment
```
Local → supabase functions deploy → Supabase Dashboard → LIVE!
```

---

## Security Features

### Frontend Security
✅ Environment variables prefixed with `VITE_` (public)
✅ HTTPS enforced by Vercel
✅ Content Security Policy headers
✅ XSS protection via React

### Backend Security
✅ Row Level Security (RLS) on database
✅ Service role key never exposed to frontend
✅ CORS headers configured properly
✅ Input validation and sanitization
✅ Rate limiting on API endpoints

### Data Privacy
✅ GDPR compliant
✅ Sri Lankan PDPA compliant (Act No. 9 of 2022)
✅ Privacy Policy included
✅ Terms & Conditions included

---

## Performance Features

### Frontend Performance
- ⚡ Vite for fast builds
- 📦 Code splitting
- 🗜️ Asset optimization
- 🌐 Vercel Edge Network (global CDN)
- 💾 Browser caching

### Backend Performance
- 🚀 Edge Functions (run close to users)
- 📊 Database indexes
- 🔄 Connection pooling
- 💨 Serverless auto-scaling

---

## Monitoring & Analytics

### Vercel Dashboard
- Real-time visitor analytics
- Build logs & deployment history
- Performance metrics
- Error tracking

### Supabase Dashboard
- Database queries
- Edge Function logs
- API usage statistics
- Error monitoring

---

## Backup & Recovery

### Database Backups
- Automatic daily backups (Supabase)
- Point-in-time recovery
- Manual backup via SQL export

### Code Backups
- Version control via Git/GitHub
- Deployment history on Vercel
- Rollback to any previous version

---

## Scaling Capacity

### Current Free Tier Limits
- **Vercel:** 100GB bandwidth/month
- **Supabase:** 
  - 500MB database storage
  - 500K Edge Function invocations/month
  - 2GB file storage

### This Supports:
- ✅ 1000+ products
- ✅ 10,000+ monthly visitors
- ✅ Thousands of transactions
- ✅ Perfect for small to medium business

### When to Upgrade:
- 📈 More than 100GB bandwidth/month
- 📈 More than 500K API calls/month
- 📈 More than 500MB product data

---

## Support & Maintenance

### Regular Tasks
- Add/update products via Supabase
- Monitor orders via WhatsApp
- Check Vercel analytics weekly
- Update prices as needed

### Monthly Reviews
- Check bandwidth usage
- Review API call limits
- Update product inventory
- Analyze popular products

---

## Future Enhancements

### Potential Additions
- 📧 Email notifications (via SendGrid)
- 💳 Online payment gateway (Stripe)
- 📦 Order tracking system
- ⭐ Product reviews
- 🎁 Discount codes
- 📱 Mobile app (React Native)
- 🔐 Customer accounts
- 📊 Admin dashboard

---

## Contact & Support

**Your Store:**
- WhatsApp: +94 71 077 3717
- Email: f1lankabusiness@gmail.com

**Technical Support:**
- Vercel Docs: https://vercel.com/docs
- Supabase Docs: https://supabase.com/docs
- React Docs: https://react.dev

---

**🏁 Your F1 Lanka store is built for success!**

*Professional, scalable, and ready to grow with your business.*

---

*Last Updated: March 5, 2026*
