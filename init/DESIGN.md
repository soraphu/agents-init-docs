## 1. Styling & Theming System

- **CSS Variables Only**: Always use design tokens and CSS variables defined in `main.css` (e.g., `var(--primary)`, `var(--background)` or mapped utility classes).
- **No Hardcoded Colors**: Strictly prohibit inline hex, RGB, or arbitrary CSS color values (e.g., `bg-[#1e293b]`). All colors must be driven by `main.css` for instant theme customization.

---

## 2. Layout & Responsiveness

- **Mobile-First Approach**: Write base styles for mobile viewports first. Expand layout structures progressively using breakpoint modifiers (`sm:`, `md:`, `lg:`, `xl:`).
- **Standard Web Layout**: Focus on clean, functional, and accessible web standards. Avoid over-engineered, flashy, or cluttered visual gimmicks (e.g., excessive glow effects, floating elements, or heavy animations).

---

## 3. SEO & Semantic HTML

- **Semantic Tags**: Use proper HTML5 semantic tags (`<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<footer>`) to ensure accessibility and SEO readability.
- **Strict Heading Hierarchy**: Every page MUST contain exactly one `h1`. Subsections must strictly follow `h2` and `h3` hierarchy without skipping levels.

---

## 4. Typography Scale

Apply uniform font scales across the entire project:

- **`h1`**: `text-3xl font-bold tracking-tight md:text-4xl lg:text-5xl`
- **`h2`**: `text-2xl font-semibold tracking-tight md:text-3xl`
- **`h3`**: `text-xl font-medium tracking-tight md:text-2xl`
- **Body**: `text-base leading-relaxed text-muted-foreground`

---

## 5. UI Architecture & Directory Structure

### 5.1 Componentization Rules

- **Always Decompose**: Deconstruct UI into components at all times—even for elements rendered only once.
- **Extract Shared Components**: Any UI pattern used **2 or more times (>= 2)** must be moved to a shared component.

### 5.2 Framework-Agnostic UI / Logic Separation

- **View (Presentation Layer)**: Pure UI markup and styling. Contains NO business logic, complex data transformations, or direct API fetch logic.
- **Logic Controller / State Layer**: Handles state, user events, and side-effects (React Custom Hooks, Vue Composables, or Svelte Stores).

### 5.3 State Scope Rules

- **Local UI State** (e.g., modal open/close): Kept inside the View component.
- **Form / Page State** (e.g., form inputs, page data): Managed inside the Logic Controller.
- **Global App State** (e.g., user session, theme): Handled via Global Store (Zustand, Pinia, Redux).

### 5.4 Directory Organization

```text
src/components/
├── shared/                  # Reusable components used >= 2 times across pages
│   ├── ui/                  # Primitives (Button, Input, Modal, Card)
│   └── layout/              # Global Navbar, Footer, Sidebar
│
└── pages/                   # Page-specific components (Used strictly on 1 page)
    ├── home/                # Home page components (e.g., HeroBanner)
    └── checkout/            # Checkout page components (e.g., PaymentForm)

```

---

## 6. Async UI States Standard

All components fetching asynchronous data MUST handle 3 distinct UI states:

1. **Loading State**: Render Skeleton screens or Spinners; disable submit buttons.
2. **Error State**: Display clear, user-friendly error messages with retry options.
3. **Empty State**: Provide a dedicated empty container (Icon + Title + Description + Action button) when data array is empty.

---

## 7. Component Code Structure Example

### Logic Controller (`useLoginForm.ts` / `useLoginForm.ts` composable)

```typescript
// Framework-Agnostic Logic Layer (React Hook / Vue Composable)
export function useLoginForm() {
  const [email, setEmail] = useState("");
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    setIsLoading(true);
    try {
      // Execute login logic
    } catch (err) {
      setError("Invalid credentials");
    } finally {
      setIsLoading(false);
    }
  };

  return { email, setEmail, isLoading, error, handleSubmit };
}
```

### View (`login-form.tsx`)

```tsx
// Pure Presentation Layer
import { useLoginForm } from "./useLoginForm";

export function LoginForm() {
  const { email, setEmail, isLoading, error, handleSubmit } = useLoginForm();

  return (
    <form
      onSubmit={handleSubmit}
      className="w-full max-w-md p-4 bg-card text-card-foreground rounded-lg md:p-6"
    >
      <h2 className="text-2xl font-semibold tracking-tight md:text-3xl mb-4">
        Sign In
      </h2>

      {error && (
        <div className="p-3 mb-4 text-sm bg-destructive/10 text-destructive rounded-md">
          {error}
        </div>
      )}

      <div className="flex flex-col gap-4">
        <input
          type="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          className="w-full px-3 py-2 border border-input bg-background text-foreground rounded-md focus:ring-2 focus:ring-primary"
          placeholder="Email address"
        />
        <button
          type="submit"
          disabled={isLoading}
          className="w-full py-2 px-4 bg-primary text-primary-foreground font-medium rounded-md disabled:opacity-50"
        >
          {isLoading ? "Signing in..." : "Sign In"}
        </button>
      </div>
    </form>
  );
}
```
