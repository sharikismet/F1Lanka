# 🎨 F1 Store - Complete Customization Guide

This guide will help you customize every aspect of your F1 merchandise store. No coding experience required for basic changes!

---

## 📋 Table of Contents
1. [Change Store Name & Branding](#1-change-store-name--branding)
2. [Update WhatsApp Number](#2-update-whatsapp-number)
3. [Modify Filter Categories](#3-modify-filter-categories)
4. [Add/Remove F1 Teams](#4-addremove-f1-teams)
5. [Customize Colors & Theme](#5-customize-colors--theme)
6. [Modify Product Fields](#6-modify-product-fields)
7. [Change Footer Information](#7-change-footer-information)
8. [Mobile Responsiveness Settings](#8-mobile-responsiveness-settings)
9. [Payment & Shipping Settings](#9-payment--shipping-settings)

---

## 1. Change Store Name & Branding

### Update Site Name
**File:** `/components/Header.tsx`  
**Lines:** 46-50

```tsx
<div className="bg-red-600 px-3 py-1 text-white font-bold text-xl">F1</div>
<div className="hidden sm:block">
  <span className="text-2xl font-bold">Lanka</span>
  <span className="text-sm text-gray-500 ml-2 hidden md:inline">Formula 1 Experience in Sri lanka</span>
</div>
```

**To Change:**
- Replace `F1` with your logo text
- Replace `Lanka` with your store name
- Replace `Formula 1 Experience in Sri lanka` with your tagline

**Example:**
```tsx
<div className="bg-red-600 px-3 py-1 text-white font-bold text-xl">MY</div>
<div className="hidden sm:block">
  <span className="text-2xl font-bold">Racing Store</span>
  <span className="text-sm text-gray-500 ml-2 hidden md:inline">Best F1 Merchandise</span>
</div>
```

---

## 2. Update WhatsApp Number

**File:** `/App.tsx`  
**Line:** 33

```tsx
const WHATSAPP_NUMBER = '94706298754'; // Example: Sri Lanka number
```

**To Change:**
- Replace with your WhatsApp number
- Format: Country code + number (NO + symbol)
- Example for Sri Lanka: `94771234567`
- Example for USA: `11234567890`

---

## 3. Modify Filter Categories

### Change Product Categories
**File:** `/components/ProductFilters.tsx`  
**Lines:** 23-31

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

**To Change:**
- Add new categories: Insert a new line with `'Your Category',`
- Remove categories: Delete the line
- Rename categories: Change the text

**Example - Adding "Jackets":**
```tsx
const categories = [
  'T-Shirts',
  'Hoodies & Sweatshirts',
  'Jackets', // NEW
  'Caps & Hats',
  'Pants',
  'Model Cars',
  'Accessories',
  'Collectibles & Memorabilia',
];
```

### Update Navigation Links
**File:** `/components/Header.tsx`  
**Lines:** 81-143

Find and modify these sections to match your category names:
```tsx
<a onClick={(e) => handleCategoryClick(e, 'Caps & Hats')}>
  Headwear
</a>
```

---

## 4. Add/Remove F1 Teams

**File:** `/components/TeamSelector.tsx`  
**Lines:** 13-63

```tsx
const teams: Team[] = [
  {
    name: 'Red Bull Racing',
    color: 'from-blue-900 to-blue-700',
    logo: '🏎️',
  },
  {
    name: 'Ferrari',
    color: 'from-red-700 to-red-600',
    logo: '🐎',
  },
  // ... more teams
];
```

**To Add a New Team:**
```tsx
{
  name: 'Your Team Name',
  color: 'from-color1 to-color2', // Tailwind gradient colors
  logo: '🏁', // Any emoji
},
```

**Popular Tailwind Gradient Colors:**
- Red: `from-red-700 to-red-600`
- Blue: `from-blue-900 to-blue-700`
- Green: `from-green-700 to-green-600`
- Orange: `from-orange-500 to-orange-600`
- Black: `from-gray-900 to-black`
- Purple: `from-purple-700 to-purple-600`

**To Remove a Team:**
- Delete the entire team object (from `{` to `},`)

---

## 5. Customize Colors & Theme

### Primary Brand Color (Red)
**File:** `/styles/globals.css`  
**Line:** Search for `--color-red-600`

Current red color is used for:
- Logo background
- "Buy Now" buttons
- Price highlights
- Cart badge

To change to a different color, find and replace in files:
- `bg-red-600` → `bg-blue-600` (or your color)
- `text-red-600` → `text-blue-600`
- `hover:bg-red-700` → `hover:bg-blue-700`

### Header Sticky Behavior
**File:** `/components/Header.tsx`  
**Line:** 35

```tsx
<header className="border-b sticky top-0 bg-white z-50 shadow-sm">
```

**Options:**
- Remove `sticky top-0` for non-sticky header
- Change `z-50` to adjust layer priority

---

## 6. Modify Product Fields

### Add New Product Field
**Example:** Adding a "Size" field

**Step 1:** Update Product Interface  
**File:** `/lib/api.ts` (Line ~7)
```tsx
export interface Product {
  id: string;
  name: string;
  price: number;
  // ... existing fields
  size?: string; // ADD THIS
}
```

**Step 2:** Update AdminPanel Form  
**File:** `/components/AdminPanel.tsx`
Add input field in the form (around line 150):
```tsx
<div>
  <Label htmlFor="size">Size (Optional)</Label>
  <Input
    id="size"
    value={formData.size || ''}
    onChange={(e) => setFormData({ ...formData, size: e.target.value })}
  />
</div>
```

**Step 3:** Display in ProductDetailDialog  
**File:** `/components/ProductDetailDialog.tsx` (around line 140)
```tsx
{product.size && (
  <div className="flex justify-between py-2 border-b">
    <span className="text-gray-600">Size:</span>
    <span className="font-medium">{product.size}</span>
  </div>
)}
```

---

## 7. Change Footer Information

**File:** `/App.tsx`  
**Lines:** Search for `<footer`

```tsx
<footer className="border-t mt-16 py-8 bg-gray-50">
  <div className="container mx-auto px-4 text-center text-gray-600">
    <p className="text-sm">© 2026 F1 Store - Formula 1 Experience. All rights reserved.</p>
    <p className="text-xs mt-2">Official F1 Merchandise Store</p>
  </div>
</footer>
```

**To Customize:**
- Change year: `2026` → Current year
- Change business name: `F1 Store - Formula 1 Experience`
- Add contact info, social links, or legal pages

**Example - Adding Links:**
```tsx
<footer className="border-t mt-16 py-8 bg-gray-50">
  <div className="container mx-auto px-4 text-center text-gray-600">
    <p className="text-sm">© 2026 My F1 Store. All rights reserved.</p>
    <div className="flex justify-center gap-4 mt-3 text-xs">
      <a href="#" className="hover:text-red-600">Privacy Policy</a>
      <a href="#" className="hover:text-red-600">Terms</a>
      <a href="#" className="hover:text-red-600">Contact</a>
    </div>
    <p className="text-xs mt-2">📧 info@myf1store.com | 📞 +94 77 123 4567</p>
  </div>
</footer>
```

---

## 8. Mobile Responsiveness Settings

### Hide Sidebar on Mobile
**File:** `/App.tsx`  
Find the ProductFilters section and wrap with responsive classes:

```tsx
{/* Filters Sidebar */}
<div className="hidden lg:block"> {/* ADD THIS WRAPPER */}
  <ProductFilters
    selectedCategories={selectedCategories}
    onCategoryChange={setSelectedCategories}
    selectedGender={selectedGender}
    onGenderChange={setSelectedGender}
  />
</div>
```

### Adjust Product Grid Columns
**File:** `/App.tsx` (search for "grid grid-cols")

```tsx
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
```

**Options:**
- `grid-cols-1` = 1 column on mobile
- `md:grid-cols-2` = 2 columns on tablets
- `lg:grid-cols-3` = 3 columns on desktop
- `xl:grid-cols-4` = 4 columns on large screens

**Example - 4 columns on desktop:**
```tsx
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
```

---

## 9. Payment & Shipping Settings

### Free Shipping Threshold
**File:** `/components/CartDrawer.tsx`  
**Line:** Search for `5000`

```tsx
{totalPrice >= 5000 && (
  <p className="text-sm text-green-600 mt-2 flex items-center gap-1">
    🎉 You qualify for FREE shipping!
  </p>
)}
```

**To Change:**
- Replace `5000` with your desired amount (e.g., `10000` for LKR 10,000)

### Currency Display
**Global Search & Replace:**
- Find: `LKR`
- Replace: `USD` or your currency

**For currency symbol:**
- Find: `LKR ${price.toFixed(2)}`
- Replace: `$${price.toFixed(2)}`

---

## 🚀 Quick Reference: Common Changes

| **What to Change** | **File** | **Search For** |
|---|---|---|
| Store Name | `/components/Header.tsx` | `Lanka` |
| WhatsApp Number | `/App.tsx` | `WHATSAPP_NUMBER` |
| Filter Categories | `/components/ProductFilters.tsx` | `const categories` |
| F1 Teams | `/components/TeamSelector.tsx` | `const teams` |
| Brand Color | Multiple files | `bg-red-600` |
| Footer Text | `/App.tsx` | `<footer` |
| Free Shipping Amount | `/components/CartDrawer.tsx` | `5000` |
| Currency | All product files | `LKR` |
| Product Grid Columns | `/App.tsx` | `grid-cols` |

---

## 💡 Tips for Safe Customization

1. **Always backup before changes** - Download your current files
2. **Change one thing at a time** - Test after each change
3. **Use Find & Replace carefully** - Preview changes before applying
4. **Keep original values** - Comment them out instead of deleting
5. **Test on mobile** - Check responsive design after layout changes

---

## 🆘 Need Help?

If you make a mistake:
1. Check browser console for errors (F12 key)
2. Undo your last change
3. Compare with the original files in this guide
4. Restart the development server

---

## 📝 Custom Modifications Tracker

Use this section to track your customizations:

```
Date: ___________
Changed: Store name from "F1 Lanka" to "________________"
File: /components/Header.tsx

Date: ___________
Changed: WhatsApp number to "________________"
File: /App.tsx

Date: ___________
Added new category: "________________"
File: /components/ProductFilters.tsx

Date: ___________
Changed brand color from red-600 to "________________"
Files: Multiple

Date: ___________
Changed free shipping threshold to "________________"
File: /components/CartDrawer.tsx
```

---

Good luck with your customizations! 🏎️💨
