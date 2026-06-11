# Frontend Reuse Plan — Phase E (Subagent 5)

> Strategy for reusing components, design tokens, and patterns from the old frontend.

## Components Reused Now (Phase 1)

| Old Component | New Form | Notes |
|--------------|----------|-------|
| HSL color system (globals.css) | Preserved in new `globals.css` | Same CSS variable approach |
| Tajawal font | Preserved | Arabic-first typography |
| Animation keyframes | Preserved | fade-in, slide-in, scale-in, shimmer |
| shadcn/ui patterns | Recreated via shadcn CLI | Same Radix-based primitives |
| Tailwind config structure | Adapted | HSL tokens, container, borderRadius |

## Components Converted to Puck Blocks (Phase 1)

| Old Component | Puck Block | Changes |
|--------------|-----------|---------|
| HeroSection (14KB) | HeroBlock | Simplified props, Puck-managed |
| StatsSection (4KB) | StatsBlock | Configurable via Puck editor |
| CollegesSection (8KB) | CollegesGridBlock | Dynamic data from collection |
| NewsSection (9KB) | NewsGridBlock | Dynamic data from collection |
| ContactSection (17KB) | CTASection | Generic CTA with configurable props |

## Components Deferred (Phase 2)

| Component | Reason |
|-----------|--------|
| EventsSection | Events collection deferred |
| CampusLifeSection | Campus Life deferred |
| PartnersSection | Partners deferred |
| ProjectsSection | Projects deferred |
| CentersSection | Centers deferred |
| OffersSection | Offers deferred |
| BlogPageContent | Blog deferred |
| AdmissionSection | Complex academic workflows |
| SmartChat | AI integration deferred |
| Dashboard components | Refine replacement deferred |
| RouteGuard | Auth system changes |

## Components Requiring Refactor

| Component | Issue | Action |
|-----------|-------|--------|
| Header (33KB) | Very large, tightly coupled to Frappe API | Rebuild as smaller layout component |
| Footer (11KB) | Coupled to old API patterns | Rebuild using Payload globals |
| All detail pages | Fetch from Frappe API | Rebuild using Payload Local API |

## Design Tokens to Preserve

```css
/* These exact CSS variables should be migrated to new project */
:root {
  --background, --foreground
  --primary, --primary-foreground
  --secondary, --secondary-foreground
  --muted, --muted-foreground
  --accent, --accent-foreground
  --destructive, --destructive-foreground
  --border, --input, --ring
  --card, --card-foreground
  --radius
  --font-tajawal
}
```

## Tailwind Configuration to Preserve

- HSL color tokens via CSS variables
- Custom container (center, 2rem padding, 1400px max)
- Custom border radius (lg, md, sm via --radius)
- Tajawal font family definitions
- Custom animations and keyframes
- `darkMode: ["class"]`
- `tailwindcss-animate` plugin
