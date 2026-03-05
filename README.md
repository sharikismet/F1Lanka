# 🏎️ F1 Lanka Store - Formula 1 Merchandise E-Commerce

A full-featured e-commerce platform for selling F1 merchandise with dual checkout options (WhatsApp + Direct), complete admin panel, order management, and mobile-responsive design.

![Status](https://img.shields.io/badge/status-production%20ready-brightgreen)
![Platform](https://img.shields.io/badge/platform-Figma%20Make-purple)
![Framework](https://img.shields.io/badge/framework-React%2018-blue)

---

## 📋 Table of Contents
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Quick Start](#-quick-start)
- [Environment Setup](#-environment-setup)
- [Project Structure](#-project-structure)
- [Configuration](#-configuration)
- [Documentation](#-documentation)
- [Deployment](#-deployment)

---

## ✨ Features

### Customer Features
- 🛒 **Shopping Cart System** - Add multiple items, adjust quantities, persistent storage
- 🔍 **Advanced Filtering** - Filter by category, gender, team, and search
- 🏎️ **Team Selection** - Browse products by F1 team (Red Bull, Ferrari, Mercedes, etc.)
- 💬 **Dual Checkout Options**:
  - WhatsApp integration for instant messaging
  - Direct checkout form for users without WhatsApp
- 📦 **Stock Indicators** - "Out of Stock" and "Low Stock" badges
- 📱 **Mobile Responsive** - Optimized for all devices
- 🎨 **F1 Racing Aesthetics** - Professional racing-inspired design

### Admin Features
- 🔐 **Secure Authentication** - Protected admin panel with Supabase Auth
- 📦 **Product Management** - Add, edit, delete products with inventory tracking
- 📊 **Order Dashboard** - View and manage all orders with status tracking
- 📈 **Order Status Flow** - Pending → Confirmed → Shipped → Delivered
- 🔒 **API Protection** - All write operations require authentication

### Technical Features
- ⚡ **Real-time Updates** - Instant cart and inventory updates
- 💾 **Persistent Cart** - Cart data saved in localStorage
- 🔄 **Optimistic UI** - Smooth user experience with instant feedback
- 🛡️ **Security** - Backend validation and protected endpoints
- 📡 **RESTful API** - Clean API architecture with Supabase Edge Functions

---

## 🛠 Tech Stack

### Frontend
- **React 18** - UI library
- **TypeScript** - Type safety
- **Tailwind CSS v4** - Utility-first styling
- **shadcn/ui** - UI component library
- **Radix UI** - Accessible component primitives
- **Lucide React** - Icon library
- **Sonner** - Toast notifications

### Backend
- **Supabase** - Backend as a Service
  - PostgreSQL database
  - Edge Functions (Deno runtime)
  - Authentication
  - Storage (for future use)
- **Hono** - Web framework for Edge Functions

### State Management
- **React Context API** - Cart state management
- **localStorage** - Client-side persistence

---

## 🚀 Quick Start

### 1. Configure Environment

The application requires Supabase connection. The following environment variables are pre-configured:
- `SUPABASE_URL` - Your Supabase project URL
- `SUPABASE_ANON_KEY` - Your Supabase anonymous key
- `SUPABASE_SERVICE_ROLE_KEY` - Service role key (backend only)
- `SUPABASE_DB_URL` - Database connection string

**These are automatically set up in the Figma Make environment.**

### 2. Update WhatsApp Number

**File:** `/App.tsx` (Line 33)
```tsx
const WHATSAPP_NUMBER = '94706298754'; // Replace with your number
```

**Format:** Country code + number (no + or spaces)
- Sri Lanka: `94771234567`
- USA: `11234567890`
- UK: `447700900000`

### 3. Deploy Backend

The backend Edge Function must be deployed to Supabase:

1. Go to your Supabase Dashboard
2. Navigate to **Edge Functions**
3. The function `/supabase/functions/server/index.tsx` should be deployed
4. Ensure it's running (green status)

### 4. Load Sample Data

On first visit:
1. Click "Load Sample Products" button
2. 6 sample products will be created
3. Or use the admin panel to add your own products

### 5. Customize (Optional)

- Store name: `/components/Header.tsx`
- Categories: `/components/ProductFilters.tsx`
- Teams: `/components/TeamSelector.tsx`
- Colors: Search/replace `bg-red-600`

**See `/COMPLETE_CUSTOMIZATION_GUIDE.md` for detailed instructions.**

---

## 🔧 Environment Setup

### Required Environment Variables

While the core Supabase variables are pre-configured, if you plan to add external integrations, create a `.env` file:

```env
# Supabase (Auto-configured in Figma Make)
SUPABASE_URL=your-project-url.supabase.co
SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key

# Optional: Add your own
VITE_WHATSAPP_NUMBER=94706298754
VITE_STORE_NAME="F1 Lanka"
VITE_FREE_SHIPPING_THRESHOLD=5000

# Future integrations (if needed)
STRIPE_PUBLIC_KEY=pk_test_...
PAYMENT_GATEWAY_KEY=...
```

### API Keys You Might Need

**Currently Using:**
- ✅ Supabase (configured)

**Future Integrations:**
- Stripe/PayPal for payment processing
- SendGrid/Mailgun for email notifications
- Cloudinary for image hosting
- Google Analytics for tracking

---

## 📁 Project Structure

```
/
├── src/
│   ├── App.tsx                 # Main application component
│   │
│   ├── components/             # React components
│   │   ├── Header.tsx          # Navigation & search
│   │   ├── ProductCard.tsx     # Product display card
│   │   ├── ProductDetailDialog.tsx  # Product popup
│   │   ├── ProductFilters.tsx  # Category/gender filters
│   │   ├── CartDrawer.tsx      # Shopping cart slide-out
│   │   ├── DirectCheckoutDialog.tsx # Checkout form
│   │   ├── TeamSelector.tsx    # F1 team selection
│   │   ├── AdminPanel.tsx      # Product management
│   │   ├── OrderManagement.tsx # Order tracking
│   │   │
│   │   └── ui/                 # shadcn/ui components
│   │       ├── button.tsx
│   │       ├── dialog.tsx
│   │       ├── sheet.tsx
│   │       └── ... (40+ components)
│   │
│   ├── lib/                    # Business logic
│   │   ├── api.ts              # Backend API calls
│   │   ├── auth.ts             # Authentication
│   │   ├── CartContext.tsx     # Cart state management
│   │   └── mockProducts.ts     # Sample data
│   │
│   ├── supabase/functions/server/
│   │   ├── index.tsx           # Edge Function server (Hono)
│   │   └── kv_store.tsx        # Key-value database utilities
│   │
│   ├── styles/
│   │   └── globals.css         # Global styles & Tailwind
│   │
│   └── utils/
│       └── supabase/info.tsx   # Supabase connection info
│
├── Documentation/              # Guides & references
│   ├── README.md               # This file
│   ├── QUICK_START_README.md   # 3-minute setup
│   ├── STEP_BY_STEP_USER_GUIDE.md  # Complete manual
│   ├── COMPLETE_CUSTOMIZATION_GUIDE.md  # Customization
│   ├── IMPLEMENTATION_GUIDE.md # Technical details
│   └── ERRORS_FIXED.md         # Troubleshooting
│
└── .env.example                # Environment variables template
```

### Key Directories Explained

**`/components/`**
- Reusable React components
- Each component is self-contained
- UI components from shadcn/ui in `/ui/` subdirectory

**`/lib/`**
- Business logic and state management
- API client functions
- Context providers

**`/supabase/functions/server/`**
- Backend Edge Function (runs on Deno)
- RESTful API endpoints
- Database operations

**`/styles/`**
- Global CSS and Tailwind configuration
- Custom color tokens
- Typography settings

---

## ⚙️ Configuration

### 1. Store Settings

**File:** `/App.tsx`
```tsx
// WhatsApp number for checkout
const WHATSAPP_NUMBER = '94706298754';

// Server connection test
await testServerConnection();
```

### 2. Categories

**File:** `/components/ProductFilters.tsx`
```tsx
const categories = [
  'T-Shirts',
  'Hoodies & Sweatshirts',
  'Caps & Hats',
  'Pants',
  'Model Cars',
  'Accessories',
  'Collectibles & Memorabilia',
];
```

### 3. F1 Teams

**File:** `/components/TeamSelector.tsx`
```tsx
const teams: Team[] = [
  {
    name: 'Red Bull Racing',
    color: 'from-blue-900 to-blue-700',
    logo: '🏎️',
  },
  // ... more teams
];
```

### 4. Shipping Threshold

**File:** `/components/CartDrawer.tsx`
```tsx
{totalPrice >= 5000 && (
  <p>🎉 You qualify for FREE shipping!</p>
)}
```

### 5. Currency

**Global:** Search and replace `LKR` with your currency (e.g., `USD`, `EUR`)

---

## 📚 Documentation

Comprehensive guides are available:

1. **[QUICK_START_README.md](./QUICK_START_README.md)**
   - 3-minute setup guide
   - Essential configuration
   - Quick testing checklist

2. **[STEP_BY_STEP_USER_GUIDE.md](./STEP_BY_STEP_USER_GUIDE.md)**
   - Complete usage instructions
   - Customer journey walkthrough
   - Admin panel guide
   - Order management
   - Mobile experience

3. **[COMPLETE_CUSTOMIZATION_GUIDE.md](./COMPLETE_CUSTOMIZATION_GUIDE.md)**
   - Change store name & branding
   - Modify categories and teams
   - Customize colors and theme
   - Add new product fields
   - Footer and mobile settings

4. **[IMPLEMENTATION_GUIDE.md](./IMPLEMENTATION_GUIDE.md)**
   - Technical architecture
   - Security implementation
   - Backend API documentation
   - Authentication setup

5. **[ERRORS_FIXED.md](./ERRORS_FIXED.md)**
   - Common issues and solutions
   - Troubleshooting guide

---

## 🚢 Deployment

### Deploying the Backend

**Supabase Edge Function:**
1. Install Supabase CLI: `npm install -g supabase`
2. Login: `supabase login`
3. Link project: `supabase link --project-ref your-project-ref`
4. Deploy function:
   ```bash
   supabase functions deploy server
   ```

### Frontend (Figma Make)

The frontend is automatically deployed in Figma Make. To deploy elsewhere:

**Vercel:**
```bash
npm install -g vercel
vercel
```

**Netlify:**
```bash
npm install -g netlify-cli
netlify deploy
```

**Manual Build:**
```bash
npm run build
# Outputs to /dist
```

---

## 🔐 Security

### Protected Endpoints

All write operations require authentication:

- ✅ `POST /products` - Create product
- ✅ `PUT /products/:id` - Update product
- ✅ `DELETE /products/:id` - Delete product
- ✅ `PUT /orders/:id/status` - Update order status

### Public Endpoints

- ✅ `GET /products` - List products
- ✅ `GET /products/:id` - Get product
- ✅ `POST /orders` - Create order (customer)
- ✅ `GET /orders` - List orders (requires auth for admin)

### Authentication Flow

1. Admin logs in via Supabase Auth
2. Access token stored in localStorage
3. Token sent in `Authorization` header
4. Backend validates token before processing

---

## 🎯 User Flows

### Customer Shopping Journey

```
Browse → Filter/Search → View Details → Add to Cart → Review Cart → Checkout
                                                                        ↓
                                                              WhatsApp OR Direct
```

### Order Processing

```
Customer Places Order → Admin Receives → Confirms → Ships → Delivers
                                              ↓
                                          (Can Cancel)
```

---

## 🤝 Contributing

### Adding New Features

1. **New Product Field:**
   - Update `Product` interface in `/lib/api.ts`
   - Add to AdminPanel form
   - Display in ProductDetailDialog

2. **New Payment Method:**
   - Create new checkout dialog component
   - Add to cart drawer
   - Integrate with payment gateway

3. **Email Notifications:**
   - Add email service integration
   - Send order confirmations
   - Notify admin of new orders

---

## 📊 Database Schema

### Products Table (via KV Store)
```typescript
{
  id: string;
  name: string;
  price: number;
  originalPrice?: number;
  image: string;
  category: string;
  gender: 'all' | 'men' | 'women' | 'kids';
  team?: string;
  description?: string;
  isClearance?: boolean;
  stockQuantity?: number;
}
```

### Orders Table (via KV Store)
```typescript
{
  id: string;
  items: Array<{
    productId: string;
    productName: string;
    quantity: number;
    price: number;
  }>;
  customerName: string;
  customerPhone: string;
  customerEmail?: string;
  shippingAddress?: string;
  notes?: string;
  totalAmount: number;
  status: 'pending' | 'confirmed' | 'shipped' | 'delivered' | 'cancelled';
  createdAt: string;
  updatedAt?: string;
}
```

---

## 🐛 Troubleshooting

### Common Issues

**1. Products not loading**
- Check Supabase Edge Function status
- Verify environment variables
- Check browser console for errors

**2. Cart not persisting**
- Check if localStorage is enabled
- Clear browser cache
- Test in incognito mode

**3. WhatsApp not opening**
- Verify WHATSAPP_NUMBER format (no + or spaces)
- Test on actual mobile device
- Ensure WhatsApp is installed

**4. Orders not saving**
- Check backend deployment
- Verify API endpoint URLs
- Check network tab for errors

**See [ERRORS_FIXED.md](./ERRORS_FIXED.md) for more solutions.**

---

## 📝 License

This project is created for educational and commercial use. Feel free to modify and use for your business.

---

## 🙏 Acknowledgments

- **shadcn/ui** - Component library
- **Radix UI** - Accessible primitives
- **Supabase** - Backend infrastructure
- **Tailwind CSS** - Styling framework
- **Figma Make** - Development platform

---

## 📧 Support

For issues or questions:
1. Check documentation files
2. Review [STEP_BY_STEP_USER_GUIDE.md](./STEP_BY_STEP_USER_GUIDE.md)
3. Test features following [QUICK_START_README.md](./QUICK_START_README.md)

---

## 🚀 What's Next?

### Recommended Enhancements

**Phase 1: MVP (Current)**
- ✅ Product browsing
- ✅ Shopping cart
- ✅ Dual checkout
- ✅ Order management

**Phase 2: Payments**
- [ ] Stripe integration
- [ ] PayPal integration
- [ ] Payment confirmation emails
- [ ] Receipt generation

**Phase 3: Advanced Features**
- [ ] User accounts
- [ ] Order history
- [ ] Wishlist
- [ ] Product reviews
- [ ] Related products
- [ ] Size/variant selection

**Phase 4: Marketing**
- [ ] Discount codes
- [ ] Email marketing
- [ ] Abandoned cart recovery
- [ ] Analytics dashboard

---

**Built with ❤️ for F1 fans worldwide** 🏎️💨

Last Updated: 2026-02-19
