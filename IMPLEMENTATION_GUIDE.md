# F1 Store - Complete Implementation Guide

## ✅ COMPLETED IMPROVEMENTS

### 1. 🔒 SECURITY (CRITICAL) - ✅ DONE
**Backend Protection:**
- ✅ Added `requireAuth` middleware to protect write operations (POST, PUT, DELETE)
- ✅ All product creation, updates, and deletions now require authentication
- ✅ Added `/signup` endpoint for creating admin users
- ✅ Auth checks verify user tokens using Supabase Auth

**What This Fixes:**
- Hackers can NO LONGER delete/modify products without authentication
- Only logged-in admin users can manage inventory
- Public users can still browse products (read-only)

### 2. 📦 INVENTORY MANAGEMENT - ✅ DONE  
**Stock Tracking:**
- ✅ Added `stockQuantity` field to Product interface and database
- ✅ Sample data includes realistic stock levels (one product has 0 stock)
- ✅ Backend stores and manages stock quantities

**Next Steps (UI Updates Needed):**
- Update ProductCard to show "Out of Stock" badge when stock Quantity === 0
- Update ProductDetailDialog to disable buy buttons when out of stock
- Show low stock warnings (e.g., "Only 3 left!")

### 3. 📊 ORDER MANAGEMENT - ✅ DONE
**Backend Complete:**
- ✅ Created Order interface with status tracking
- ✅ POST `/orders` - Create new order
- ✅ GET `/orders` - Fetch all orders (auth required)
- ✅ PUT `/orders/:id/status` - Update order status (auth required)

**Order Statuses:**
- pending → confirmed → shipped → delivered
- Can also be marked as cancelled

**Next Steps (UI Updates Needed):**
- Create OrderManagement component for AdminPanel
- Display order list with status badges
- Add status update dropdown

---

## 🚧 REMAINING TASKS

### 4. 🛒 SHOPPING CART (HIGH PRIORITY)
**Create Cart Context:** `/lib/CartContext.tsx`
```tsx
import { createContext, useContext, useState, ReactNode } from 'react';
import type { CartItem, Product } from './api';

interface CartContextType {
  items: CartItem[];
  addToCart: (product: Product) => void;
  removeFromCart: (productId: string) => void;
  updateQuantity: (productId: string, quantity: number) => void;
  clearCart: () => void;
  totalItems: number;
  totalPrice: number;
}

const CartContext = createContext<CartContextType | undefined>(undefined);

export function CartProvider({ children }: { children: ReactNode }) {
  const [items, setItems] = useState<CartItem[]>([]);

  const addToCart = (product: Product) => {
    setItems(prev => {
      const existing = prev.find(item => item.id === product.id);
      if (existing) {
        return prev.map(item =>
          item.id === product.id
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      }
      return [...prev, { ...product, quantity: 1 }];
    });
  };

  const removeFromCart = (productId: string) => {
    setItems(prev => prev.filter(item => item.id !== productId));
  };

  const updateQuantity = (productId: string, quantity: number) => {
    if (quantity <= 0) {
      removeFromCart(productId);
      return;
    }
    setItems(prev =>
      prev.map(item =>
        item.id === productId ? { ...item, quantity } : item
      )
    );
  };

  const clearCart = () => setItems([]);

  const totalItems = items.reduce((sum, item) => sum + item.quantity, 0);
  const totalPrice = items.reduce((sum, item) => sum + item.price * item.quantity, 0);

  return (
    <CartContext.Provider value={{
      items,
      addToCart,
      removeFromCart,
      updateQuantity,
      clearCart,
      totalItems,
      totalPrice,
    }}>
      {children}
    </CartContext.Provider>
  );
}

export function useCart() {
  const context = useContext(CartContext);
  if (!context) {
    throw new Error('useCart must be used within CartProvider');
  }
  return context;
}
```

**Update App.tsx:**
```tsx
import { CartProvider } from './lib/CartContext';

function App() {
  return (
    <CartProvider>
      {/* existing app content */}
    </CartProvider>
  );
}
```

**Create CartDrawer Component:** `/components/CartDrawer.tsx`
Use the `Sheet` component for a slide-out cart with:
- List of cart items with quantity controls
- Total price calculation
- "Checkout via WhatsApp" button that sends ALL items in one message

**Update Header:**
- Show cart icon with badge displaying `totalItems`
- Click cart icon to open CartDrawer

### 5. 🔐 LOGIN PAGE (HIGH PRIORITY)
**Create LoginPage:** `/components/LoginPage.tsx`
```tsx
import { useState } from 'react';
import { signIn, signUp } from '../lib/auth';
import { Button } from './ui/button';
import { Input } from './ui/input';
import { Card } from './ui/card';

export function LoginPage({ onLoginSuccess }: { onLoginSuccess: () => void }) {
  const [isSignup, setIsSignup] = useState(false);
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [name, setName] = useState('');
  const [error, setError] = useState('');

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    setError('');

    if (isSignup) {
      const { user, error } = await signUp(email, password, name);
      if (error) {
        setError(error);
        return;
      }
      alert('Account created! Now sign in with your credentials.');
      setIsSignup(false);
    } else {
      const { session, error } = await signIn(email, password);
      if (error) {
        setError(error);
        return;
      }
      if (session) {
        // Store session in localStorage
        localStorage.setItem('session', JSON.stringify(session));
        onLoginSuccess();
      }
    }
  };

  return (
    <div className="min-h-screen flex items-center justify-center bg-gray-50">
      <Card className="w-full max-w-md p-8">
        <h1 className="text-2xl font-bold mb-6">
          {isSignup ? 'Create Admin Account' : 'Admin Login'}
        </h1>

        <form onSubmit={handleSubmit} className="space-y-4">
          {isSignup && (
            <div>
              <label className="block text-sm font-medium mb-1">Name</label>
              <Input
                type="text"
                value={name}
                onChange={(e) => setName(e.target.value)}
                required
              />
            </div>
          )}

          <div>
            <label className="block text-sm font-medium mb-1">Email</label>
            <Input
              type="email"
              value={email}
              onChange={(e) => setEmail(e.target.value)}
              required
            />
          </div>

          <div>
            <label className="block text-sm font-medium mb-1">Password</label>
            <Input
              type="password"
              value={password}
              onChange={(e) => setPassword(e.target.value)}
              required
            />
          </div>

          {error && <p className="text-red-600 text-sm">{error}</p>}

          <Button type="submit" className="w-full">
            {isSignup ? 'Sign Up' : 'Sign In'}
          </Button>
        </form>

        <button
          onClick={() => setIsSignup(!isSignup)}
          className="w-full text-center text-sm text-gray-600 mt-4"
        >
          {isSignup ? 'Already have an account? Sign in' : 'Need an account? Sign up'}
        </button>
      </Card>
    </div>
  );
}
```

**Update App.tsx:**
- Check for session on mount using `getSession()` from `/lib/auth.ts`
- Show LoginPage if not authenticated AND admin panel is requested
- Store access_token for API calls

### 6. 📄 LEGAL PAGES (MEDIUM PRIORITY)
Create these pages in `/components/legal/`:

**PrivacyPolicy.tsx**
**TermsOfService.tsx**
**RefundPolicy.tsx**
**ContactUs.tsx**

Add links in Footer component.

### 7. 🎨 UI ENHANCEMENTS

**ProductCard - Show Stock Status:**
```tsx
{stockQuantity === 0 && (
  <Badge className="absolute top-2 right-2 bg-gray-800">
    Out of Stock
  </Badge>
)}
{stockQuantity > 0 && stockQuantity < 10 && (
  <Badge className="absolute top-2 right-2 bg-orange-600">
    Only {stockQuantity} left!
  </Badge>
)}
```

**ProductDetailDialog - Check Stock:**
```tsx
const isOutOfStock = product.stockQuantity === 0;

<Button
  disabled={isOutOfStock}
  onClick={handleBuyNow}
>
  {isOutOfStock ? 'Out of Stock' : 'Buy Now'}
</Button>
```

---

## 🚀 DEPLOYMENT CHECKLIST

1. **Deploy Supabase Edge Function:**
   - Go to your Supabase Dashboard → Functions
   - Deploy `/supabase/functions/server/index.tsx`
   - Verify deployment with health check endpoint

2. **Create First Admin User:**
   - Use the `/signup` endpoint or create via Supabase dashboard
   - Email: your-email@example.com
   - Password: secure-password

3. **Update WhatsApp Number:**
   - In `App.tsx`, line 26: `const WHATSAPP_NUMBER = 'YOUR_NUMBER_HERE';`
   - Format: Country code + number (no + symbol)
   - Example Sri Lanka: `94771234567`

4. **Test Authentication:**
   - Try creating a product without logging in → Should fail
   - Log in → Should succeed
   - Delete a product → Should require auth

5. **Load Sample Data:**
   - Click "Load Sample Products" button
   - Verify products appear with stock quantities
   - Check that one product shows "Out of Stock"

---

## 📝 NOTES

**Auth Flow:**
1. Admin clicks "Show Admin Panel"
2. If not logged in, show LoginPage
3. After login, store session in localStorage
4. Use access_token for all admin API calls
5. Show logout button in admin panel

**Order Tracking:**
- WhatsApp purchases can manually be logged in admin panel
- Each order stores customer name, phone, items, and status
- Admin can update status as order progresses

**Security:**
- Never expose SUPABASE_SERVICE_ROLE_KEY to frontend
- Only use publicAnonKey for read operations
- All write operations require user authentication
- Backend validates all requests

**Stock Management:**
- Currently manual - update via admin panel
- Future: Auto-decrement stock on purchase confirmation
- Low stock warnings help prevent overselling

---

## 🐛 TROUBLESHOOTING

**"Unauthorized" errors:**
- Check if you're logged in
- Verify access_token is being sent in requests
- Check Supabase Auth logs

**Products not updating:**
- Ensure Edge Function is deployed
- Check network tab for API responses
- Verify you're using correct access_token

**Cart not persisting:**
- Cart is in-memory only (resets on page refresh)
- Future: Save to localStorage or database
- Orders are saved to database

---

## ✨ FUTURE ENHANCEMENTS

1. **Payment Integration:** Stripe, PayPal, or local payment gateway
2. **Email Notifications:** Order confirmations, shipping updates
3. **Advanced Search:** Filter by team, price range
4. **Customer Accounts:** Save addresses, order history
5. **Product Reviews:** Star ratings and comments
6. **Shipping Calculator:** Based on location
7. **Multi-currency:** Support USD, EUR, GBP
8. **Inventory Alerts:** Email when stock is low
9. **Analytics Dashboard:** Sales reports, popular products
10. **Image Optimization:** Compress and serve optimized images

---

Good luck with your F1 store! 🏎️
