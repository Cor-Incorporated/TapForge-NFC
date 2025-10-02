# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

TapForge NFC is a landing page for an NFC card printing service built with Astro 5.x in SSR mode. The site is deployed to Cloudflare Pages and integrates with Stripe for payment processing.

## Development Commands

```bash
# Install dependencies
npm install

# Start development server (accessible on network via --host flag)
npm run dev

# Build for production (outputs to dist/)
npm run build

# Preview production build locally
npm run preview
```

The dev server runs on `http://localhost:4321`.

## Architecture

### SSR Configuration

- **Output mode**: `server` (SSR enabled in astro.config.mjs)
- **Adapter**: Cloudflare Pages (`@astrojs/cloudflare`)
- **Prerendering**: Home page (`index.astro`) uses `export const prerender = true` for static generation, while API routes use `export const prerender = false` for server-side execution

### Page Structure

The main page (`src/pages/index.astro`) is composed of sections in this order:
1. HeroSection
2. ServicesCarousel (Swiper-based)
3. ServiceHighlights
4. ClientsSection
5. CaseStudiesSection
6. TestimonialsSection
7. PricingSection
8. FaqSection
9. CTA section (inline in index.astro)

### Data-Driven Content

All content is managed through JSON files in `src/data/`:
- `global_settings.json` - Site metadata, navigation, theme color
- `home.json` - Page titles and descriptions
- `pricing.json` - Price plans with Stripe Price IDs
- `services.json` - Service carousel items
- `faq.json` - FAQ accordion items
- `testimonials.json` - Customer testimonials
- `case_studies.json` - Case study cards
- `clients.json` - Client logos

To modify content, edit the relevant JSON file rather than the components.

### Stripe Integration

The `/api/create-checkout` endpoint creates Stripe Checkout sessions:

```javascript
// Client-side usage
const response = await fetch("/api/create-checkout", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    priceId: "price_10set", // From pricing.json
    quantity: 1
  })
});
const { url } = await response.json();
window.location.href = url;
```

**Environment variables required:**
- `STRIPE_SECRET_KEY` - Backend API key
- `PUBLIC_STRIPE_PUBLISHABLE_KEY` - Frontend key

The API gracefully handles missing keys in development with informative error messages.

### Image Optimization

Images are optimized at build time using Astro's compile service for Cloudflare compatibility:
- **Service**: `compile` (configured in astro.config.mjs)
- **Static images**: Use Astro's `<Image>` component with `format="avif"` for optimal compression
- **Dynamic images**: Use standard `<img>` tags with lazy loading for content from JSON data
- **Hero background**: Optimized AVIF format with eager loading

### Styling

- **Framework**: Tailwind CSS 4.x (configured via `@tailwindcss/vite` plugin)
- **Theme colors**:
  - Primary blue: `#1E40AF` (used in hero, CTA)
  - Accent green: `#10B981` (used in buttons)
- **Fonts**: Inter and Inter Display (local fonts configured in astro.config.mjs experimental fonts feature)

### Component Pattern

Components receive props from JSON data imports in `index.astro`. They follow this pattern:
- Props: `title`, `content`, data array (e.g., `pricing`, `faq`, `services`)
- Styling: Tailwind utility classes
- Interactivity: Minimal JavaScript (e.g., Swiper initialization, accordion toggles)

### Deployment

Cloudflare Pages configuration:
- **Build command**: `npm run build`
- **Output directory**: `dist`
- Set environment variables in Cloudflare dashboard (same as .env file)

## Key Implementation Notes

- Pages that need SSR must set `export const prerender = false` in frontmatter
- The site uses Japanese language content throughout
- Stripe Price IDs in `pricing.json` must match actual Stripe dashboard products
- Navigation is anchor-based (`/#pricing`, `/#faq`, etc.)

## Error Handling Best Practices

### API Routes
- All error messages should be in Japanese for consistency
- Validate all inputs (priceId, quantity) before processing
- Return structured error responses with `{ error: string }` format
- Use proper HTTP status codes (400 for validation, 500 for server errors)
- Log errors to console for debugging while providing user-friendly messages

### Client-side
- Store original button text before showing loading state
- Check for `data.error` in API responses before proceeding
- Restore button state (text and disabled attribute) on error
- Use `instanceof Error` checks for proper error handling
- Provide Japanese error messages in alert dialogs
