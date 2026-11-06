## การใช้ค่าสีจาก Global CSS เพื่อรองรับการปรับเปลี่ยน Theme และความคงเส้นคงวาของ UI

```
## Styling Standards & Design Token Enforcement

- Always utilize global CSS variables and design tokens defined in `main.css` or `global.css` for color values and key styles.
- Never hardcode raw color values (e.g., HEX, RGB, HSL) directly inside components, utility classes, or inline styles.
- Rely strictly on semantic theme variables to ensure effortless theme switching and consistent UI formatting across the application.
- Maintain uniformity by ensuring all newly created components subscribe to the established global stylesheet tokens.
```

## การออกแบบ Responsive Layout แบบ Mobile-First และการจัดการ Breakpoints

```
## Mobile-First Responsive Architecture

- Adopt a strict Mobile-First strategy for all user interface implementations and layouts derived from reference designs.
- Define base styles for mobile viewports first, progressively building and enhancing layouts using standard breakpoints (`sm`, `md`, `lg`, `xl`).
- Avoid desktop-first styling patterns or destructive CSS overrides by scoping complex layout structures to larger breakpoint queries.
- Translate visual reference designs into fluid, adaptive components that render seamlessly on mobile devices without horizontal overflow.
- Maintain consistent breakpoint usage and responsive grid/flex configurations across all UI components and pages.
```

## การใช้โครงสร้าง HTML Semantic Elements ตามมาตรฐาน SEO และ Accessibility

```
## Semantic HTML & SEO Element Standards

- Use semantic HTML elements (`<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<footer>`, `<aside>`) to structure document layouts for optimal search engine crawling.
- Enforce a strict and logical heading hierarchy (`<h1>` to `<h6>`), ensuring exactly one primary `<h1>` tag per page without skipping heading levels.
- Avoid using generic container elements like `<div>` or `<span>` when an appropriate native semantic tag exists.
- Ensure all visual and interactive elements provide proper accessibility and SEO attributes, including descriptive `alt` texts for images and suitable `aria-*` tags.
- Structure content and document flow clearly to maximize machine readability, page indexability, and technical SEO performance.
```

## การกำหนดมาตรฐาน Typographic Scale และ Typography Hierarchy ของโปรเจกต์

```
## Typography Scale & Hierarchy Standardization

- Enforce uniform typography and font sizing rules across all components and pages within the project.
- Strictly adhere to the project's standard typography scale and utility class hierarchy:
  - h1: `text-3xl font-bold tracking-tight md:text-4xl lg:text-5xl`
  - h2: `text-2xl font-semibold tracking-tight md:text-3xl`
  - h3: `text-xl font-medium tracking-tight md:text-2xl`
  - Body: `text-base leading-relaxed text-muted-foreground`
- Avoid applying arbitrary font sizes, ad-hoc text utilities, or custom font styling overrides outside this design system.
- Maintain consistent font weight, line height (`leading`), and letter spacing (`tracking`) across all viewport breakpoints.
```

## การจัดโครงสร้าง Frontend Architecture และการแยกส่วนประกอบของโค้ด (Separation of Concerns)

```
## Frontend Architecture & Code Structure

- Centralize all static configuration values, environment constants, and reusable magic values strictly inside the `constants/` directory.
- Modularize UI components into dedicated files, even if a component is rendered only once on a single page, avoiding inline component clutter.
- Maintain a structured `components/` directory by logically categorizing elements into subdirectories (e.g., `ui/` for low-level design primitives, `shared/` for cross-cutting layout elements, and feature-specific folders).
- Refactor repetitive logic or utility functions executed two or more times into dedicated helper modules inside `utils/`, custom React/Vue compositions inside `hooks/`, or appropriate abstraction layers.
- Separate TypeScript interfaces, type aliases, and schema definitions into the `types/` directory rather than co-locating them in presentation or logic files.
- Enforce Separation of Concerns by isolating data-fetching and complex business logic into custom hooks. However, simple UI-only local state (such as modal toggles, dropdown states, or tab indexing) should remain inside presentational components.
```

## การจัดการ State การแสดงผลของ UI ตามสถานการณ์ (Loading, Error, Empty State)

```
## State-Driven UI & Scenario Handling

- Explicitly manage and render state-driven UI for all asynchronous operations and data-fetching views, including Loading, Error, Empty, and Success states.
- Choose contextual and appropriate state representations tailored to the feature context (e.g., skeleton loaders or micro-spinners for Loading, actionable error banners for Error, and clear illustrations with call-to-action triggers for Empty states).
- Ensure smooth transitions between operational states without causing sudden layout shifts, unhandled UI flashes, or flickering content.
- Provide clear feedback and recovery pathways in Error states, such as retry buttons or user-friendly fallback messaging, avoiding raw technical exception leaks.
- Design Empty states to guide the user constructively by explaining the absence of data and suggesting actionable next steps where applicable.
```
