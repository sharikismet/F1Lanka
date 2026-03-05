# 🚀 F1 Lanka Store - Quick Start Guide

## ⏱️ Get Your Store Running in 5 Minutes!

Follow these steps in order to launch your store:

---

## Step 1: Deploy Backend (2 minutes)

### A. Deploy Supabase Edge Function

1. Go to your Supabase Dashboard: https://supabase.com/dashboard
2. Select your project
3. Click "Edge Functions" in the left menu
4. Click "Deploy new function"
5. Select `/supabase/functions/server/index.tsx`
6. Wait for deployment to complete (green checkmark)

---

## Step 2: Configure Store (1 minute)

### A. Update WhatsApp Number

**File:** `/App.tsx`  
**Line:** 28

```tsx
const WHATSAPP_NUMBER = '94706298754'; // ← REPLACE THIS
```

**Format:** Country code + number (no + symbol, no spaces)

**Examples:**
- Sri Lanka: `94771234567`
- UAE: `971501234567`
- India: `919876543210`

### B. Change Store Name (Optional)

**File:** `/components/Header.tsx`  
**Line:** 48

```tsx
<span className="text-2xl font-bold">Lanka</span> {/* ← Change this */}
```

---

## Step 3: Load Products (30 seconds)

1. Refresh your site
2. Click "Load Sample Products" button
3. Wait for success message
4. 6 sample products will appear

---

## Step 4: Test Everything (1.5 minutes)

### A. Test Product Browsing
- [ ] Click a product card → Detail modal opens
- [ ] Check stock badges (Out of Stock, Low Stock)
- [ ] Close modal

### B. Test Shopping Cart
- [ ] Click "Add to Cart" on any product
- [ ] Cart badge updates (top right)
- [ ] Click cart icon → Drawer opens
- [ ] Adjust quantity with +/- buttons
- [ ] Remove an item
- [ ] Add more items

### C. Test WhatsApp Checkout
- [ ] Click "Checkout via WhatsApp" (green button)
- [ ] WhatsApp opens with formatted message
- [ ] Check all items are listed correctly
- [ ] Close WhatsApp (don't send yet)

### D. Test Direct Checkout
- [ ] Go back to cart
- [ ] Click "Direct Checkout (No WhatsApp)"
- [ ] Fill in form:
  - Name: Test User
  - Phone: 077 123 4567
  - Address: 123 Test Street
  - City: Colombo
- [ ] Click "Place Order"
- [ ] Success message appears with order ID
- [ ] Cart clears automatically

### E. Test Filters
- [ ] Click "Men" in header → Products filter
- [ ] Click "Women" → Products update
- [ ] Click "Shop by Team" → Team dialog opens
- [ ] Select "Ferrari" → Products filter by team
- [ ] Type in search bar → Results filter in real-time
- [ ] Click logo → All filters reset

### F. Test Mobile
- [ ] Open DevTools (F12)
- [ ] Click device icon (mobile view)
- [ ] Test cart on mobile
- [ ] Test team selector on mobile

---

## Step 5: Verify Orders in Database (30 seconds)

1. Go to Supabase Dashboard
2. Click "Database" → "Tables"
3. Find `kv_store_a97df12b` table
4. Look for keys starting with `order:`
5. Click to view order details

---

## ✅ Done! Your Store is Live!

---

## 🎯 What You Have Now

✅ Fully functional shopping cart  
✅ WhatsApp checkout  
✅ Direct checkout with order form  
✅ Team selector with 8 F1 teams  
✅ Mobile responsive design  
✅ Inventory tracking  
✅ Secure backend  
✅ Order management  

---

## 📚 Next Steps

### Add Your Own Products

1. Click "Show Admin Panel" button
2. Fill in product form:
   - Name
   - Price
   - Image URL (use Unsplash or your own)
   - Category
   - Gender
   - Stock Quantity
   - Team (optional)
3. Click "Add Product"

### Customize Appearance

See **CUSTOMIZATION_GUIDE.md** for:
- Changing colors
- Modifying categories
- Adding/removing teams
- Updating shipping threshold
- Changing currency

### View Orders

See **STEP_BY_STEP_GUIDE.md** for:
- How to access orders via API
- Understanding order structure
- Building admin orders dashboard

---

## 🔥 Pro Tips

### Performance
- Clear localStorage if cart acts weird: `localStorage.clear()`
- Check browser console (F12) for errors

### Testing
- Use incognito mode to test as a new customer
- Test on actual mobile devices, not just emulator

### Orders
- Check Supabase database regularly for new orders
- Update order status via API or build admin UI

### Marketing
- Update promo banner for sales
- Change team logos for special events
- Add seasonal products

---

## 🆘 Common Issues

| Issue | Solution |
|-------|----------|
| "Failed to fetch" errors | Deploy Edge Function in Supabase |
| Cart items disappear | Check localStorage is enabled |
| WhatsApp doesn't open | Verify number format (no + or spaces) |
| Products don't show | Click "Load Sample Products" |
| Mobile navigation cut off | Scroll horizontally or hide items |

---

## 📞 Need Help?

**Documentation Files:**
- **IMPLEMENTATION_SUMMARY.md** - Feature overview
- **STEP_BY_STEP_GUIDE.md** - Technical deep dive
- **CUSTOMIZATION_GUIDE.md** - Customization options
- **IMPLEMENTATION_GUIDE.md** - Security & deployment

**Check:**
1. Browser console for errors (F12)
2. Supabase Edge Function deployment status
3. Network tab for failed requests

---

## 🎨 Customization Quick Reference

| What to Change | File | Line |
|----------------|------|------|
| WhatsApp Number | `/App.tsx` | 28 |
| Store Name | `/components/Header.tsx` | 48 |
| Promo Banner | `/components/Header.tsx` | 155 |
| Shipping Threshold | `/components/CartDrawer.tsx` | 89, 94 |
| Product Categories | `/components/ProductFilters.tsx` | 23-31 |
| F1 Teams | `/components/TeamSelector.tsx` | 11-46 |
| Primary Color | Search & replace `red-600` | All files |
| Currency | Search & replace `LKR` | Multiple files |

---

## 🏁 You're Ready to Go!

Your F1 store is production-ready. Start selling! 🏎️

**Happy Racing! 🏁**

---

*Quick Start Guide v1.0 - F1 Lanka Store*  
*Last Updated: February 14, 2026*
