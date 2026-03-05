# 📦 Product Management Guide

Since the admin panel has been removed, you'll manage products directly through your **Supabase Dashboard**. This is actually more secure and gives you full control.

---

## 🎯 How to Add/Edit Products

### Option 1: Using Supabase Dashboard (Recommended)

1. **Go to Supabase Dashboard**
   - Visit: https://app.supabase.com/
   - Select your project

2. **Open Table Editor**
   - Click on "Table Editor" in the left sidebar
   - Find the table: `kv_store_a97df12b`

3. **Add a Product**
   - Click "Insert row" button
   - Fill in the fields:
     - **key**: `product:YOUR_UNIQUE_ID` (e.g., `product:ferrari-tshirt-001`)
     - **value**: Paste the JSON below (customize it):

```json
{
  "id": "ferrari-tshirt-001",
  "name": "Ferrari Racing T-Shirt - Red",
  "price": 45.00,
  "originalPrice": 65.00,
  "image": "https://images.unsplash.com/photo-XXXXX",
  "category": "T-Shirts",
  "gender": "men",
  "isClearance": false,
  "team": "Ferrari",
  "description": "Official Ferrari racing team t-shirt with premium quality fabric",
  "stockQuantity": 50,
  "createdAt": "2026-02-19T10:00:00.000Z"
}
```

4. **Product Fields Explained**

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `id` | string | ✅ Yes | Unique identifier (must match key) | `"ferrari-tshirt-001"` |
| `name` | string | ✅ Yes | Product name | `"Ferrari Racing T-Shirt"` |
| `price` | number | ✅ Yes | Current price | `45.00` |
| `originalPrice` | number | ❌ No | Original price (for discounts) | `65.00` |
| `image` | string | ✅ Yes | Image URL | `"https://images.unsplash.com/..."` |
| `category` | string | ✅ Yes | Product category | `"T-Shirts"` |
| `gender` | string | ✅ Yes | Target audience | `"men"`, `"women"`, `"kids"`, `"all"` |
| `isClearance` | boolean | ❌ No | Show clearance badge | `true` or `false` |
| `team` | string | ❌ No | F1 team name | `"Ferrari"`, `"Mercedes"`, etc. |
| `description` | string | ❌ No | Product details | `"Official Ferrari..."` |
| `stockQuantity` | number | ❌ No | Available stock | `50` |
| `createdAt` | string | ❌ No | Creation timestamp | `"2026-02-19T10:00:00.000Z"` |

---

## 📋 Valid Categories

Your store supports these categories (must match exactly):

- `T-Shirts`
- `Hoodies & Sweatshirts`
- `Caps & Hats`
- `Pants`
- `Model Cars`
- `Accessories`
- `Collectibles & Memorabilia`

---

## 👥 Valid Gender/Age Values

- `men` - Men's products
- `women` - Women's products
- `kids` - Kids' products
- `all` - Unisex/All ages

---

## 🏁 F1 Teams

Popular teams you can use:

- Ferrari
- Mercedes
- Red Bull Racing
- McLaren
- Aston Martin
- Alpine
- Williams
- AlphaTauri
- Alfa Romeo
- Haas

---

## 🖼️ Getting Product Images

### Unsplash (Free)

1. Go to https://unsplash.com/
2. Search for relevant images (e.g., "red t-shirt", "racing cap")
3. Click on an image
4. Click "Download" → Right-click → "Copy image address"
5. Paste the URL in the `image` field

**Example URLs:**
```
https://images.unsplash.com/photo-1763558134668-e185bbab7aca?w=1080
https://images.unsplash.com/photo-1752348512163-68e894f22046?w=1080
```

---

## 📦 Stock Management

### Stock Quantity Badges

Your store automatically shows badges based on stock:

- **`stockQuantity: 0`** → Shows "Out of Stock" badge (red)
- **`stockQuantity: 1-10`** → Shows "Low Stock" badge (orange)
- **`stockQuantity: 11+`** → No badge (normal)

---

## ✏️ Edit a Product

1. Go to Supabase Dashboard → Table Editor
2. Find the row with `key` = `product:YOUR_ID`
3. Click on the row
4. Edit the JSON in the `value` field
5. Click "Save"

---

## 🗑️ Delete a Product

1. Go to Supabase Dashboard → Table Editor
2. Find the row with `key` = `product:YOUR_ID`
3. Click the trash icon on the right
4. Confirm deletion

---

## 🚀 Quick Add Template

Copy this template and customize it for new products:

```json
{
  "id": "CHANGE_THIS_ID",
  "name": "Product Name Here",
  "price": 0.00,
  "originalPrice": 0.00,
  "image": "https://images.unsplash.com/photo-XXXXX",
  "category": "T-Shirts",
  "gender": "men",
  "isClearance": false,
  "team": "Ferrari",
  "description": "Product description here",
  "stockQuantity": 100,
  "createdAt": "2026-02-19T10:00:00.000Z"
}
```

**Remember:**
- Change the `id` field to something unique
- Use the same ID in the **key** field: `product:YOUR_ID`
- Update all other fields with your product info

---

## 💡 Pro Tips

1. **Use Descriptive IDs**
   ```
   ✅ product:ferrari-sf23-tshirt-red-xl
   ❌ product:1
   ```

2. **Keep Stock Updated**
   - Manually update `stockQuantity` as you sell products
   - Set to `0` when sold out

3. **Use High-Quality Images**
   - Minimum 1080px width
   - Clear product photos
   - Consistent background style

4. **Price Strategy**
   - Set `originalPrice` higher than `price` to show discount
   - If no discount, leave `originalPrice` empty or same as `price`

5. **Clearance Items**
   - Set `isClearance: true` for special deals
   - Shows a "Clearance" badge on the product card

---

## 🛠️ Load Sample Data

If you need to populate your store with sample products quickly:

1. Visit your store website
2. If the store is empty, you'll see a button: **"Load Sample Products"**
3. Click it
4. Sample products will be added automatically

This creates 6 sample F1 products with realistic data.

---

## ❓ Troubleshooting

### Products Not Showing Up?

1. **Check the key format**
   - Must be: `product:YOUR_ID`
   - NOT: `YOUR_ID` or `products:YOUR_ID`

2. **Check JSON validity**
   - Use a JSON validator: https://jsonlint.com/
   - Make sure all quotes are correct
   - No trailing commas

3. **Check required fields**
   - Must have: `id`, `name`, `price`, `image`, `category`, `gender`

4. **Refresh your store**
   - Products load when you visit the page
   - Hard refresh: `Ctrl + Shift + R` (Windows) or `Cmd + Shift + R` (Mac)

### Image Not Loading?

- Make sure the URL is a direct image link
- Test the URL in a new browser tab
- Use Unsplash or another reliable image host

---

## 🎯 Example: Adding a Ferrari Cap

**Step 1:** In Supabase Table Editor, click "Insert row"

**Step 2:** Fill in:
- **key:** `product:ferrari-cap-red-001`
- **value:**
```json
{
  "id": "ferrari-cap-red-001",
  "name": "Scuderia Ferrari 2025 Team Cap - Red",
  "price": 35.00,
  "originalPrice": 50.00,
  "image": "https://images.unsplash.com/photo-1752348512163-68e894f22046?w=1080",
  "category": "Caps & Hats",
  "gender": "all",
  "isClearance": false,
  "team": "Ferrari",
  "description": "Official Ferrari team cap with embroidered logo. Adjustable strap fits all sizes.",
  "stockQuantity": 75,
  "createdAt": "2026-02-19T12:00:00.000Z"
}
```

**Step 3:** Click "Save"

**Step 4:** Refresh your store - the new cap appears!

---

## ✅ You're All Set!

You can now manage your entire product catalog through Supabase. This approach is:

- ✅ **More secure** - No public admin panel
- ✅ **More flexible** - Full database access
- ✅ **Easier to backup** - Use Supabase export tools
- ✅ **No coding needed** - Just edit JSON

Happy selling! 🏎️💨
