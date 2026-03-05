# 🤝 Contributing to F1 Lanka Store

Thank you for your interest in contributing! This document provides guidelines for adding features, fixing bugs, and improving the codebase.

---

## 📋 Table of Contents
- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Code Standards](#code-standards)
- [Adding Features](#adding-features)
- [Testing](#testing)
- [Pull Request Process](#pull-request-process)

---

## Getting Started

### Prerequisites
1. Basic knowledge of React and TypeScript
2. Understanding of Tailwind CSS
3. Familiarity with Supabase (for backend changes)

### Setup
1. Clone the project (if deploying outside Figma Make)
2. Review `/README.md` for setup instructions
3. Check `/DEPENDENCIES.md` for tech stack details
4. Read `/STEP_BY_STEP_USER_GUIDE.md` to understand features

---

## Development Workflow

### Making Changes

**1. Identify the scope:**
- Frontend only (UI/UX)
- Backend only (API/Database)
- Full-stack (Both)

**2. Create a branch (if using Git):**
```bash
git checkout -b feature/your-feature-name
```

**3. Make changes incrementally:**
- Change one thing at a time
- Test after each change
- Commit frequently

**4. Test thoroughly:**
- Desktop browser
- Mobile browser
- Different screen sizes
- Edge cases

---

## Code Standards

### TypeScript

**Use proper typing:**
```typescript
// ✅ Good
interface Product {
  id: string;
  name: string;
  price: number;
}

function getProduct(id: string): Product | null {
  // ...
}

// ❌ Avoid
function getProduct(id: any): any {
  // ...
}
```

**Use type imports:**
```typescript
import type { Product } from './lib/api';
```

### React Components

**Use functional components:**
```typescript
// ✅ Good
export function ProductCard({ product }: { product: Product }) {
  return <div>{product.name}</div>;
}

// ❌ Avoid class components
export class ProductCard extends React.Component {
  // ...
}
```

**Use hooks properly:**
```typescript
// ✅ Good - hooks at top level
function MyComponent() {
  const [state, setState] = useState(0);
  const { data } = useQuery();
  
  return <div>{data}</div>;
}

// ❌ Bad - conditional hooks
function MyComponent() {
  if (someCondition) {
    const [state, setState] = useState(0); // ❌
  }
}
```

### Styling

**Use Tailwind CSS:**
```typescript
// ✅ Good
<div className="flex items-center gap-4 p-4 bg-gray-50 rounded-lg">

// ❌ Avoid inline styles
<div style={{ display: 'flex', padding: '16px' }}>
```

**Use cn() utility for conditional classes:**
```typescript
import { cn } from './components/ui/utils';

<div className={cn(
  "base-classes",
  isActive && "active-classes",
  isDisabled && "disabled-classes"
)}>
```

### File Organization

**Component files:**
```typescript
// ProductCard.tsx
import { ... } from '...'  // External imports first
import { ... } from './...' // Local imports second

interface ProductCardProps {
  // Props interface
}

export function ProductCard({ ... }: ProductCardProps) {
  // Hooks
  const [state, setState] = useState();
  
  // Event handlers
  const handleClick = () => { };
  
  // Render logic
  return <div>...</div>;
}
```

---

## Adding Features

### New Product Field

**Example: Adding a "Size" field**

**Step 1:** Update type definition
```typescript
// /lib/api.ts
export interface Product {
  // ... existing fields
  size?: string; // ADD THIS
}
```

**Step 2:** Update AdminPanel form
```typescript
// /components/AdminPanel.tsx
const [formData, setFormData] = useState({
  // ... existing fields
  size: '', // ADD THIS
});

// Add input in form
<div>
  <Label htmlFor="size">Size (Optional)</Label>
  <Input
    id="size"
    value={formData.size}
    onChange={(e) => setFormData({ ...formData, size: e.target.value })}
  />
</div>
```

**Step 3:** Include in submission
```typescript
// /components/AdminPanel.tsx - handleSubmit
const productData = {
  // ... existing fields
  size: formData.size || undefined,
};
```

**Step 4:** Display in UI
```typescript
// /components/ProductDetailDialog.tsx
{product.size && (
  <div className="flex justify-between py-2 border-b">
    <span className="text-gray-600">Size:</span>
    <span className="font-medium">{product.size}</span>
  </div>
)}
```

**Step 5:** Update handleEdit
```typescript
// /components/AdminPanel.tsx - handleEdit
setFormData({
  // ... existing fields
  size: product.size || '',
});
```

### New Checkout Method

**Example: Adding Stripe payment**

**Step 1:** Install Stripe
```typescript
// Just import - auto-installs
import { loadStripe } from '@stripe/stripe-js';
```

**Step 2:** Create component
```typescript
// /components/StripeCheckoutDialog.tsx
import { Elements } from '@stripe/react-stripe-js';
import { loadStripe } from '@stripe/stripe-js';

const stripePromise = loadStripe('pk_test_...');

export function StripeCheckoutDialog({ open, onOpenChange, cartItems }) {
  return (
    <Dialog open={open} onOpenChange={onOpenChange}>
      <DialogContent>
        <Elements stripe={stripePromise}>
          {/* Payment form */}
        </Elements>
      </DialogContent>
    </Dialog>
  );
}
```

**Step 3:** Add to CartDrawer
```typescript
// /components/CartDrawer.tsx
import { StripeCheckoutDialog } from './StripeCheckoutDialog';

export function CartDrawer({ ... }) {
  const [showStripe, setShowStripe] = useState(false);
  
  return (
    <>
      {/* Existing buttons */}
      <Button onClick={() => setShowStripe(true)}>
        Pay with Card
      </Button>
      
      <StripeCheckoutDialog
        open={showStripe}
        onOpenChange={setShowStripe}
        cartItems={items}
      />
    </>
  );
}
```

**Step 4:** Backend endpoint
```typescript
// /supabase/functions/server/index.tsx
app.post('/create-payment-intent', async (c) => {
  // Stripe payment intent logic
});
```

### New Backend Endpoint

**Example: Get popular products**

**Step 1:** Add route
```typescript
// /supabase/functions/server/index.tsx
app.get('/make-server-a97df12b/products/popular', async (c) => {
  try {
    // Get all products
    const products = await getByPrefix('product:');
    
    // Sort by some criteria (e.g., sales count)
    const popular = products
      .sort((a, b) => (b.salesCount || 0) - (a.salesCount || 0))
      .slice(0, 10);
    
    return c.json({ success: true, products: popular });
  } catch (error) {
    return c.json({ success: false, error: error.message }, 500);
  }
});
```

**Step 2:** Frontend API function
```typescript
// /lib/api.ts
export async function getPopularProducts(): Promise<Product[]> {
  try {
    const res = await fetch(
      `${API_URL}/products/popular`,
      {
        headers: { 'Authorization': `Bearer ${publicAnonKey}` },
      }
    );
    const data = await res.json();
    return data.products || [];
  } catch (error) {
    console.error('Error fetching popular products:', error);
    return [];
  }
}
```

**Step 3:** Use in component
```typescript
// /components/PopularProducts.tsx
export function PopularProducts() {
  const [products, setProducts] = useState<Product[]>([]);
  
  useEffect(() => {
    loadPopularProducts();
  }, []);
  
  const loadPopularProducts = async () => {
    const popular = await getPopularProducts();
    setProducts(popular);
  };
  
  return (
    <div className="grid grid-cols-3 gap-4">
      {products.map(product => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
}
```

---

## Testing

### Manual Testing Checklist

**For UI Changes:**
- [ ] Works on Chrome
- [ ] Works on Firefox
- [ ] Works on Safari
- [ ] Works on mobile Chrome
- [ ] Works on mobile Safari
- [ ] Responsive at all breakpoints (375px, 768px, 1024px, 1440px)
- [ ] No console errors
- [ ] No TypeScript errors
- [ ] Accessible (keyboard navigation, screen readers)

**For Backend Changes:**
- [ ] API returns expected response
- [ ] Error handling works
- [ ] Authentication works (if protected)
- [ ] No console errors in server logs
- [ ] Database operations succeed
- [ ] Edge cases handled

**For Features:**
- [ ] Feature works as expected
- [ ] Doesn't break existing functionality
- [ ] Performance is acceptable
- [ ] User experience is intuitive
- [ ] Documentation updated

### Testing Tools

**Browser DevTools:**
```
1. Open DevTools (F12)
2. Check Console tab for errors
3. Check Network tab for API calls
4. Use Responsive Design Mode for mobile testing
```

**Supabase Dashboard:**
```
1. Check Edge Function logs
2. View database contents
3. Monitor API usage
4. Check authentication logs
```

---

## Pull Request Process

### Before Submitting

1. **Test everything:**
   - Run through manual testing checklist
   - Test on multiple browsers
   - Test on mobile

2. **Update documentation:**
   - Update README if needed
   - Update relevant guide files
   - Add comments to complex code

3. **Clean up code:**
   - Remove console.logs
   - Remove commented code
   - Format consistently

### PR Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Changes Made
- Change 1
- Change 2
- Change 3

## Testing
- [ ] Tested on desktop
- [ ] Tested on mobile
- [ ] No console errors
- [ ] Documentation updated

## Screenshots (if UI change)
[Add screenshots]

## Related Issues
Closes #123
```

---

## Best Practices

### State Management

**Use React Context for global state:**
```typescript
// ✅ Good - global cart state
<CartProvider>
  <App />
</CartProvider>

// ❌ Bad - prop drilling
<App cart={cart} setCart={setCart}>
  <Header cart={cart} setCart={setCart}>
    <CartIcon cart={cart} />
  </Header>
</App>
```

### Error Handling

**Always handle errors:**
```typescript
// ✅ Good
try {
  const data = await fetchData();
  setData(data);
} catch (error) {
  console.error('Error fetching data:', error);
  toast.error('Failed to load data');
}

// ❌ Bad
const data = await fetchData();
setData(data);
```

### API Calls

**Use proper async/await:**
```typescript
// ✅ Good
async function loadProducts() {
  setLoading(true);
  try {
    const products = await getProducts();
    setProducts(products);
  } catch (error) {
    console.error(error);
    toast.error('Failed to load products');
  } finally {
    setLoading(false);
  }
}

// ❌ Bad
function loadProducts() {
  getProducts().then(setProducts);
}
```

### Accessibility

**Use semantic HTML:**
```typescript
// ✅ Good
<button onClick={handleClick}>Click me</button>
<nav>...</nav>
<main>...</main>

// ❌ Bad
<div onClick={handleClick}>Click me</div>
<div className="nav">...</div>
<div className="main">...</div>
```

**Add ARIA labels:**
```typescript
<button aria-label="Close dialog">
  <X className="w-4 h-4" />
</button>
```

---

## Common Pitfalls

### 1. State Updates Not Reflecting
```typescript
// ❌ Wrong - mutating state
const updateItem = () => {
  items[0].name = 'New name';
  setItems(items);
};

// ✅ Correct - new array
const updateItem = () => {
  setItems(items.map((item, i) => 
    i === 0 ? { ...item, name: 'New name' } : item
  ));
};
```

### 2. useEffect Dependencies
```typescript
// ❌ Wrong - missing dependency
useEffect(() => {
  loadData(productId);
}, []); // Missing productId

// ✅ Correct
useEffect(() => {
  loadData(productId);
}, [productId]);
```

### 3. Async in useEffect
```typescript
// ❌ Wrong - async useEffect
useEffect(async () => {
  const data = await fetchData();
}, []);

// ✅ Correct - async inside
useEffect(() => {
  async function load() {
    const data = await fetchData();
  }
  load();
}, []);
```

---

## Questions?

If you have questions:
1. Check existing documentation files
2. Review similar code in the codebase
3. Test your changes thoroughly
4. Ask for clarification if needed

---

**Thank you for contributing to F1 Lanka Store!** 🏎️💨
