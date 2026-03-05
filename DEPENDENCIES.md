# 📦 Dependencies & Tech Stack

## About This Environment

This project runs on **Figma Make**, which has special dependency handling:
- Dependencies are auto-installed when imported
- No traditional `package.json` management needed
- Some packages work differently than standard React apps

---

## Core Dependencies

### Framework & Runtime
```json
{
  "react": "^18.0.0",
  "react-dom": "^18.0.0",
  "typescript": "latest"
}
```
**Auto-installed in Figma Make**

### Build Tool
- **Vite** - Fast build tool and dev server
- **Auto-configured in Figma Make**

---

## Frontend Libraries

### UI Framework
```typescript
// Tailwind CSS v4 - Utility-first CSS
// Auto-configured via /styles/globals.css
import "tailwindcss/base"
import "tailwindcss/components"
import "tailwindcss/utilities"

// Utilities
import { cn } from './components/ui/utils' // tailwind-merge + clsx
```

### UI Components (shadcn/ui)
All components from [shadcn/ui](https://ui.shadcn.com/) are included:

**Installed Components:**
- `Dialog` - Modal dialogs
- `Sheet` - Slide-out panels (cart drawer)
- `Button` - Button variants
- `Input` - Text inputs
- `Label` - Form labels
- `Select` - Dropdown selects
- `Checkbox` - Checkboxes
- `Badge` - Status badges
- `Card` - Content cards
- `Alert` - Alert messages
- `Textarea` - Multi-line inputs
- And 30+ more...

**Based on:**
```json
{
  "@radix-ui/react-dialog": "latest",
  "@radix-ui/react-select": "latest",
  "@radix-ui/react-checkbox": "latest",
  "@radix-ui/react-label": "latest",
  // ... etc
}
```

### Icons
```typescript
// Lucide React - Icon library
import { ShoppingCart, Plus, Trash2, Search } from 'lucide-react'
```

**Package:**
```json
{
  "lucide-react": "latest"
}
```

### Notifications
```typescript
// Sonner - Toast notifications
import { toast } from 'sonner@2.0.3'
import { Toaster } from './components/ui/sonner'
```

**Note:** Must use specific version `sonner@2.0.3` in Figma Make

---

## Backend & Database

### Supabase
```typescript
// Supabase Client - Backend as a Service
import { createClient } from '@supabase/supabase-js'
```

**Includes:**
- PostgreSQL Database
- Edge Functions (Deno runtime)
- Authentication
- Real-time subscriptions
- Storage

**Package:**
```json
{
  "@supabase/supabase-js": "latest"
}
```

**Environment Variables Required:**
```env
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
```

### Web Framework (Backend)
```typescript
// Hono - Web framework for Edge Functions
import { Hono } from 'npm:hono'
import { cors } from 'npm:hono/cors'
import { logger } from 'npm:hono/logger'
```

**Package (Deno):**
```json
{
  "hono": "npm:hono@latest"
}
```

---

## Utilities

### Styling Utilities
```typescript
// tailwind-merge - Merge Tailwind classes
import { twMerge } from 'tailwind-merge'

// clsx - Class name utility
import { clsx } from 'clsx'

// Combined utility
import { cn } from './components/ui/utils'
```

---

## NOT Needed (Figma Make Handles These)

### ❌ Not Required

The following are typically needed in React apps but are **auto-handled** in Figma Make:

**Build Tools:**
- ❌ `postcss` - Auto-configured
- ❌ `autoprefixer` - Auto-configured
- ❌ `vite` config - Auto-configured

**TypeScript:**
- ❌ `@types/react` - Auto-installed
- ❌ `@types/react-dom` - Auto-installed
- ❌ `@types/node` - Auto-installed
- ❌ `tsconfig.json` - Auto-configured

**Linting:**
- ❌ `eslint` - Not needed in Figma Make
- ❌ `prettier` - Not needed in Figma Make

**Package Managers:**
- ❌ `package.json` - Not used
- ❌ `package-lock.json` - Not used
- ❌ `node_modules/` - Virtual

---

## Import Syntax

### Standard Imports
```typescript
// Regular npm packages
import { useState } from 'react'
import { Button } from './components/ui/button'
```

### Versioned Imports (Figma Make Special)
```typescript
// Some packages require version specification
import { toast } from 'sonner@2.0.3'
import { useForm } from 'react-hook-form@7.55.0'
```

### Backend Imports (Deno)
```typescript
// Edge Functions use npm: prefix
import { Hono } from 'npm:hono'
import { cors } from 'npm:hono/cors'

// Node built-ins need node: prefix
import process from 'node:process'
```

---

## How Dependencies Work in Figma Make

### Frontend (Browser)
1. Import any npm package
2. Figma Make auto-resolves and bundles
3. No manual installation needed

**Example:**
```typescript
// Just import - auto-installed
import { motion } from 'motion/react'
```

### Backend (Deno)
1. Use `npm:` prefix for npm packages
2. Use `jsr:` prefix for JSR packages
3. Use `node:` prefix for Node.js built-ins

**Example:**
```typescript
import { Hono } from 'npm:hono'
import { z } from 'npm:zod'
```

---

## Adding New Dependencies

### Frontend

**Step 1:** Just import it
```typescript
import { NewLibrary } from 'new-library'
```

**Step 2:** Use it
```typescript
<NewLibrary />
```

That's it! Figma Make handles installation.

### Backend (Edge Function)

**Step 1:** Import with prefix
```typescript
import { newLib } from 'npm:new-library'
```

**Step 2:** Deploy function
```bash
supabase functions deploy server
```

---

## Version-Specific Packages

Some packages require specific versions in Figma Make:

```typescript
// ✅ Correct
import { toast } from 'sonner@2.0.3'
import { useForm } from 'react-hook-form@7.55.0'

// ❌ Wrong (will fail)
import { toast } from 'sonner'
```

**Current Version-Locked:**
- `sonner@2.0.3`
- `react-hook-form@7.55.0`

---

## Recommended Packages

### For E-commerce Enhancements

**Payment:**
```typescript
import { loadStripe } from '@stripe/stripe-js'
import { Elements } from '@stripe/react-stripe-js'
```

**Form Validation:**
```typescript
import { useForm } from 'react-hook-form@7.55.0'
import { zodResolver } from '@hookform/resolvers/zod'
import { z } from 'zod'
```

**Date Handling:**
```typescript
import { format } from 'date-fns'
```

**Animations:**
```typescript
import { motion } from 'motion/react'
```

**Charts (Analytics):**
```typescript
import { LineChart, BarChart } from 'recharts'
```

---

## Dependency Tree

```
F1 Lanka Store
│
├── React 18
│   ├── React DOM
│   └── React Context API (Cart)
│
├── TypeScript
│   └── Type definitions (auto)
│
├── Tailwind CSS v4
│   ├── tailwind-merge
│   └── clsx
│
├── shadcn/ui
│   ├── Radix UI primitives
│   │   ├── Dialog
│   │   ├── Select
│   │   ├── Checkbox
│   │   └── 30+ components
│   └── Lucide Icons
│
├── Supabase
│   ├── PostgreSQL
│   ├── Edge Functions
│   │   └── Hono (web framework)
│   ├── Auth
│   └── Storage
│
└── Utilities
    ├── Sonner (toasts)
    └── date-fns (optional)
```

---

## Bundle Size

### Estimated Bundle Sizes

**Frontend (Production):**
- React + React DOM: ~130 KB
- Radix UI components: ~80 KB
- Tailwind CSS: ~10 KB (after purge)
- Lucide Icons: ~5 KB (tree-shaken)
- Supabase Client: ~40 KB
- **Total:** ~265 KB gzipped

**Backend (Edge Function):**
- Hono: ~20 KB
- Supabase Server: ~50 KB
- **Total:** ~70 KB

---

## Performance Optimizations

### Code Splitting
```typescript
// Lazy load admin panel
const AdminPanel = lazy(() => import('./components/AdminPanel'))
```

### Tree Shaking
```typescript
// Import only what you need
import { ShoppingCart, Plus } from 'lucide-react'
// NOT: import * as Icons from 'lucide-react'
```

### Tailwind Purging
```css
/* globals.css automatically purges unused classes */
@import "tailwindcss";
```

---

## Troubleshooting

### Common Dependency Issues

**1. "Module not found"**
```typescript
// ❌ Wrong
import { Button } from '@/components/ui/button'

// ✅ Correct
import { Button } from './components/ui/button'
```

**2. "Cannot find package"**
```typescript
// Try version-specific import
import { toast } from 'sonner@2.0.3'
```

**3. "Type errors"**
```typescript
// Add type assertion
import type { Product } from './lib/api'
```

---

## Upgrading Dependencies

### Frontend

Figma Make auto-updates to latest compatible versions.

To use specific version:
```typescript
import { lib } from 'package@1.2.3'
```

### Backend

Update in import statement:
```typescript
// Old
import { Hono } from 'npm:hono@3.0.0'

// New
import { Hono } from 'npm:hono@4.0.0'
```

Then redeploy:
```bash
supabase functions deploy server
```

---

## Security

### Frontend
- ✅ Use HTTPS for all API calls
- ✅ Never expose service role key
- ✅ Validate user input
- ✅ Use environment variables

### Backend
- ✅ Validate auth tokens
- ✅ Sanitize database inputs
- ✅ Use CORS properly
- ✅ Rate limit API calls

---

## Future Dependencies (Planned)

**Phase 2: Payments**
- `@stripe/stripe-js`
- `@stripe/react-stripe-js`

**Phase 3: Advanced Features**
- `react-hook-form@7.55.0` (form management)
- `zod` (validation)
- `date-fns` (date formatting)
- `recharts` (analytics charts)

**Phase 4: Optimization**
- `@tanstack/react-query` (data fetching)
- `immer` (immutable state)

---

## References

- [Figma Make Docs](https://help.figma.com/hc/en-us/sections/16504970990999-Figma-Make)
- [React Documentation](https://react.dev/)
- [Tailwind CSS v4](https://tailwindcss.com/)
- [shadcn/ui](https://ui.shadcn.com/)
- [Supabase Docs](https://supabase.com/docs)
- [Hono Documentation](https://hono.dev/)

---

**Last Updated:** 2026-02-19  
**Environment:** Figma Make
