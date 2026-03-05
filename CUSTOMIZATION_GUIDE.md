# 🎨 F1 Lanka Store - Complete Customization Guide

This document explains how to customize every aspect of your F1 store. Follow these instructions to make changes without breaking anything!

---

## 📋 TABLE OF CONTENTS

1. [Site Branding](#site-branding)
2. [Product Categories](#product-categories)
3. [Teams & Logos](#teams--logos)
4. [Colors & Styling](#colors--styling)
5. [WhatsApp Number](#whatsapp-number)
6. [Filter Options](#filter-options)
7. [Sample Data](#sample-data)
8. [Mobile Responsiveness](#mobile-responsiveness)

---

## 1. 🏷️ SITE BRANDING

### Change Site Name

**File:** `/components/Header.tsx`

**Find (lines 49-53):**
```tsx
<div className="bg-red-600 px-3 py-1 text-white font-bold text-xl">F1</div>
<div className="hidden sm:flex flex-col">
  <span className="text-2xl font-bold leading-none">Lanka</span>
  <span className="text-xs text-gray-500 hidden md:block">Formula 1 Experience in Sri lanka</span>
</div>
```

**Change to:**
```tsx
<div className="bg-red-600 px-3 py-1 text-white font-bold text-xl">YOUR</div>
<div className="hidden sm:flex flex-col">
  <span className="text-2xl font-bold leading-none">BRAND</span>
  <span className="text-xs text-gray-500 hidden md:block">Your tagline here</span>
</div>
```

### Change Footer Text

**File:** `/App.tsx`

**Find (around line 270):**
```tsx
<footer className="border-t mt-16 py-8 bg-gray-50">
  <div className="container mx-auto px-4 text-center text-gray-600">
    <p className="text-sm">© 2026 F1 Store - Formula 1 Experience. All rights reserved.</p>
    <p className="text-xs mt-2">Official F1 Merchandise Store</p>
  </div>
</footer>
```

**Change to whatever you want!**

---

## 2. 📦 PRODUCT CATEGORIES

### Add/Remove/Rename Categories

**File:** `/components/ProductFilters.tsx`

**Find (around line 23):**
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

**To Add a Category:**
```tsx
const categories = [
  'T-Shirts',
  'Hoodies & Sweatshirts',
  'Caps & Hats',
  'Pants',
  'Model Cars',
  'Accessories',
  'Collectibles & Memorabilia',
  'Jackets',           // NEW CATEGORY
  'Keychains',         // NEW CATEGORY
];
```

**To Rename:**
Simply change the text:
```tsx
'Caps & Hats'  →  'Headwear'
'Model Cars'   →  'Diecast Models'
```

### Update Header Navigation

After changing categories, update the header links:

**File:** `/components/Header.tsx`

**Find (around line 90-140):**
```tsx
<li>
  <a 
    href="#" 
    className="hover:text-red-600 transition-colors"
    onClick={(e) => handleCategoryClick(e, 'Caps & Hats')}
  >
    Headwear
  </a>
</li>
```

Make sure the `onClick` category name matches your new category!

---

## 3. 🏎️ TEAMS & LOGOS

### Add/Remove/Edit Teams

**File:** `/components/TeamSelector.tsx`

**Find (around line 17):**
```tsx
const teams: Team[] = [
  {
    name: 'Ferrari',
    color: '#DC0000',
    logo: 'https://images.unsplash.com/photo-1763558134668-e185bbab7aca?w=400&h=400&fit=crop',
  },
  {
    name: 'Red Bull Racing',
    color: '#0600EF',
    logo: 'https://images.unsplash.com/photo-1552820728-8b83bb6b773f?w=400&h=400&fit=crop',
  },
  // ... more teams
];
```

### To Add a New Team:

```tsx
{
  name: 'Haas F1',
  color: '#B6BABD',  // Team color (hex code)
  logo: 'YOUR_IMAGE_URL_HERE',
},
```

### To Get Team Logos:

1. **Option 1: Use Unsplash** (free stock images)
   - Go to https://unsplash.com
   - Search for "formula 1" or "racing"
   - Copy image URL

2. **Option 2: Use Official Logos** (better)
   - Upload team logos to Supabase Storage
   - Get the public URL
   - Use that URL in the `logo` field

### Team Colors Reference:

```tsx
Ferrari:          '#DC0000' (Red)
Red Bull:         '#0600EF' (Blue)
Mercedes:         '#00D2BE' (Teal)
McLaren:          '#FF8700' (Orange)
Aston Martin:     '#006F62' (Green)
Alpine:           '#0090FF' (Blue)
Williams:         '#005AFF' (Blue)
Audi:             '#000000' (Black)
Alfa Romeo:       '#900000' (Burgundy)
Haas:             '#B6BABD' (Gray)
```

---

## 4. 🎨 COLORS & STYLING

### Change Brand Color (Red to Your Color)

**File:** `/styles/globals.css`

**Find:**
```css
/* Example: Current red accent is bg-red-600, text-red-600, etc. */
```

**Search and replace across ALL files:**
- `bg-red-600` → `bg-YOUR_COLOR-600`
- `hover:bg-red-700` → `hover:bg-YOUR_COLOR-700`
- `text-red-600` → `text-YOUR_COLOR-600`

**Available Tailwind colors:**
- `blue`, `green`, `purple`, `pink`, `yellow`, `indigo`, `teal`

**Example:**
Change all `red-600` to `blue-600` for a blue theme!

### Change Logo Background Color

**File:** `/components/Header.tsx`

**Find:**
```tsx
<div className="bg-red-600 px-3 py-1 text-white font-bold text-xl">F1</div>
```

**Change `bg-red-600` to:**
- `bg-blue-600` (Blue)
- `bg-green-600` (Green)
- `bg-black` (Black)
- `bg-purple-600` (Purple)

---

## 5. 📱 WHATSAPP NUMBER

### Update Your WhatsApp Business Number

**File:** `/App.tsx`

**Find (line 28):**
```tsx
const WHATSAPP_NUMBER = '94771234567'; // Example: Sri Lanka number
```

**Change to YOUR number:**
```tsx
const WHATSAPP_NUMBER = '94XXXXXXXXX'; // Your number WITHOUT the + symbol
```

**Format Examples:**
- Sri Lanka: `94771234567`
- USA: `14155551234`
- UK: `447911123456`
- India: `919876543210`

**Important:**
- NO + symbol
- NO spaces
- NO dashes
- Include country code

---

## 6. 🔧 FILTER OPTIONS

### Change Gender/Age Options

**File:** `/components/ProductFilters.tsx`

**Find (around line 94):**
```tsx
<RadioGroup value={selectedGender} onValueChange={onGenderChange}>
  <div className="flex items-center space-x-2 mb-3">
    <RadioGroupItem value="all" id="all" />
    <Label htmlFor="all">All</Label>
  </div>
  <div className="flex items-center space-x-2 mb-3">
    <RadioGroupItem value="men" id="men" />
    <Label htmlFor="men">Men</Label>
  </div>
  // ...
</RadioGroup>
```

**To Add "Unisex" option:**
```tsx
<div className="flex items-center space-x-2 mb-3">
  <RadioGroupItem value="unisex" id="unisex" />
  <Label htmlFor="unisex">Unisex</Label>
</div>
```

### Remove Filters Sidebar (Show Only Products)

**File:** `/App.tsx`

**Find (around line 200):**
```tsx
<div className="flex gap-8">
  {/* Filters Sidebar */}
  <ProductFilters ... />

  {/* Products Grid */}
  <div className="flex-1">
```

**Change to:**
```tsx
<div className="flex gap-8">
  {/* Products Grid - Full Width */}
  <div className="w-full">
```

---

## 7. 📊 SAMPLE DATA

### Modify Initial Products

**File:** `/supabase/functions/server/index.tsx`

**Find (around line 102):**
```tsx
const sampleProducts = [
  {
    id: '1',
    name: 'Scuderia Ferrari 2025 Drivers Oversized T-Shirt - Red',
    price: 40.00,
    originalPrice: 81.00,
    image: 'https://images.unsplash.com/...',
    category: 'T-Shirts',
    gender: 'men',
    isClearance: true,
    team: 'Ferrari',
    description: 'Official Ferrari racing team t-shirt',
    stockQuantity: 25,
  },
  // ... more products
];
```

**To Add Your Own Products:**
```tsx
{
  id: '7',  // Make sure IDs are unique!
  name: 'Your Product Name',
  price: 50.00,
  originalPrice: 100.00,  // Optional - for sale items
  image: 'YOUR_IMAGE_URL',
  category: 'T-Shirts',  // Must match your categories list
  gender: 'men',  // men, women, kids, or all
  isClearance: false,  // true for clearance badge
  team: 'Mercedes',  // Optional - team name
  description: 'Your product description here',
  stockQuantity: 10,  // How many you have in stock
},
```

---

## 8. 📱 MOBILE RESPONSIVENESS

### Already Implemented Features:

✅ **Header:**
- Logo shrinks on small screens
- Search bar responsive
- Mobile navigation with horizontal scroll
- Sticky header (stays at top when scrolling)

✅ **Products Grid:**
- 1 column on mobile
- 2 columns on tablets
- 3 columns on desktop

✅ **Cart Drawer:**
- Full width on mobile
- Max width on desktop

✅ **Team Selector:**
- 2 columns on mobile
- 4 columns on desktop

### Test on Mobile:

1. **Browser DevTools:**
   - Press `F12`
   - Click mobile icon
   - Test different screen sizes

2. **Real Device:**
   - Open site on your phone
   - All features should work!

---

## 📝 COMMON CUSTOMIZATIONS CHECKLIST

### Before Launch:

- [ ] Change site name from "F1 Lanka" to your brand
- [ ] Update WhatsApp number
- [ ] Add your team logos
- [ ] Customize categories for your products
- [ ] Update footer copyright text
- [ ] Change brand color if desired
- [ ] Add real product images
- [ ] Set correct stock quantities
- [ ] Test on mobile device
- [ ] Test checkout flow (WhatsApp & Direct)
- [ ] Deploy Supabase Edge Function

---

## 🆘 NEED HELP?

### Common Issues:

**Products not showing?**
- Make sure Supabase Edge Function is deployed
- Click "Load Sample Products" button
- Check browser console for errors

**Cart not working?**
- Make sure CartProvider wraps the app (check App.tsx)
- Clear localStorage: `localStorage.clear()` in console

**Team selector not showing teams?**
- Check TeamSelector.tsx teams array
- Make sure images load (check URLs)

**Mobile menu cut off?**
- Use horizontal scroll (`overflow-x-auto`)
- Reduce number of nav items for mobile

---

## 🎯 QUICK REFERENCE

| What to Change | File Location | Line Number (Approx) |
|----------------|---------------|----------------------|
| Site Name | `/components/Header.tsx` | 49-53 |
| WhatsApp Number | `/App.tsx` | 28 |
| Product Categories | `/components/ProductFilters.tsx` | 23 |
| Teams & Logos | `/components/TeamSelector.tsx` | 17 |
| Brand Color | Search `bg-red-600` in all files | All |
| Sample Products | `/supabase/functions/server/index.tsx` | 102 |
| Gender Filters | `/components/ProductFilters.tsx` | 94 |
| Footer Text | `/App.tsx` | 270 |

---

## 🚀 ADVANCED: Add Custom Fields

### Add "Size" to Products:

1. **Update Product Interface** (`/lib/api.ts`):
```tsx
export interface Product {
  // ... existing fields
  size?: string; // Add this
}
```

2. **Update AdminPanel Form** (`/components/AdminPanel.tsx`):
```tsx
// Add size input field
<Input
  label="Size"
  value={formData.size}
  onChange={(e) => setFormData({ ...formData, size: e.target.value })}
/>
```

3. **Display in ProductDetailDialog**:
```tsx
<div className="flex justify-between py-2 border-b">
  <span className="text-gray-600">Size:</span>
  <span className="font-medium">{product.size}</span>
</div>
```

---

**Happy Customizing! 🏎️**

*Last Updated: February 2026*
