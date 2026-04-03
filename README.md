# HACCS Website

Official website for HACCS (Hispanic & Latine Association of Computing College Students) at NJIT.

## Tech stack

- React + TypeScript + Vite
- Chakra UI (primary UI system in current pages)
- Tailwind CSS + shadcn/ui (present in codebase, lightly used by live pages)
- React Router (client-side routes)

## Local development

```bash
npm install
npm run dev
```

App runs on `http://localhost:8080`.

Other commands:

```bash
npm run build
npm run preview
npm run lint
```

## How to read the architecture

Use this order when onboarding so the structure is easy to follow.

1. Start at `src/main.tsx`
   - Bootstraps React and imports global CSS (`src/index.css`).

2. Move to `src/App.tsx`
   - Global providers are initialized here (ChakraProvider, QueryClientProvider, tooltip/toast providers).
   - Route map is defined here:
     - `/` -> `Home`
     - `/about` -> `AboutUs`
     - `/gallery` -> `Gallery`
     - `/achievements` -> `Achievements`
     - `/events` -> `Events`
     - `/sponsors` -> `Sponsors`
     - `*` -> `NotFound`

3. Understand shared layout in `src/components/layout/Layout.tsx`
   - Every main page is wrapped in `Layout`.
   - `Layout` composes:
     - `Navbar`
     - page content (`children`)
     - `Footer`
     - `DecorativeCirclesChakra` background accents

4. Read each route in `src/pages/*`
   - `Home.tsx`: hero + mission + social links.
   - `AboutUs.tsx`: org description + event images + E-board cards.
   - `Gallery.tsx`: static image grid + social links.
   - `Achievements.tsx`: achievements text + image grid.
   - `Events.tsx`: embedded Google Calendar + hardcoded past events.
   - `Sponsors.tsx`: sponsor tier cards + sponsor CTA panel.
   - `NotFound.tsx`: fallback route (Tailwind-styled standalone page).

5. Check reusable content components
   - `src/components/EBoardMemberChakra.tsx`: card used by About page.
   - `src/components/layout/Navbar.tsx`, `Footer.tsx`: global nav/footer behavior and external links.

6. Review design system and styling
   - Chakra tokens are defined in `src/App.tsx` under `defineConfig` (`haccs.navy`, `haccs.coral`, fonts, etc.).
   - Tailwind/theme tokens are in:
     - `tailwind.config.ts`
     - `src/index.css`
   - This is a mixed Chakra + Tailwind setup.

7. Review tooling/config
   - `vite.config.ts`: Vite config + `@` alias to `src`.
   - `components.json`: shadcn/ui generator config.
   - `public/_redirects`: SPA redirect rules for static hosts.

## Things to know before editing

- Most page content is currently hardcoded in TSX files (events, board members, sponsor tiers, social links).
- There are two UI systems in the repo:
  - Live route pages use Chakra.
  - Large `src/components/ui/*` set from shadcn exists but is mostly unused by pages.
- `QueryClientProvider` is mounted, but current pages do not fetch data through React Query yet.
- Some legacy/alternate components appear unused in current routing (for example: `DecorativeCircles.tsx`, `HeroNetworkCanvas.tsx`, `EBoardMember.tsx`).
- `Navbar` has a mobile menu icon but no open/close menu behavior implemented yet.
- A few files contain mojibake characters caused by encoding mismatch (for example hamburger icon, right-arrow symbol, copyright symbol, and bullet dot); clean these if touching those lines.
- `NotFound.tsx` is styled differently from other pages (Tailwind classes, not `Layout`).

## Where to make common changes

- Add a new page route: `src/App.tsx`
- Update top nav links: `src/components/layout/Navbar.tsx`
- Update footer links/socials: `src/components/layout/Footer.tsx`
- Update board members: `src/pages/AboutUs.tsx`
- Update past events list/calendar URLs: `src/pages/Events.tsx`
- Update sponsors: `src/pages/Sponsors.tsx`
- Adjust theme colors/fonts:
  - Chakra tokens in `src/App.tsx`
  - Tailwind tokens in `src/index.css` + `tailwind.config.ts`

## Future enhancement plan

### Phase 1: Stability and maintainability

- Normalize text encoding across all source files to UTF-8.
- Decide on one primary UI system (Chakra-only or Chakra + shadcn split with clear boundaries).
- Extract repeated external links (Discord/Instagram/LinkedIn/email) into a shared config module.
- Move hardcoded arrays (events, members, sponsors, gallery) into dedicated data files (`src/data/*`).
- Add basic tests:
  - route smoke tests
  - render tests for navbar/footer
  - data-driven page section tests

### Phase 2: Dynamic content

- Add a lightweight CMS or data backend (for events/gallery/sponsors/eboard).
- Use React Query for remote data fetching + caching.
- Add loading and error states on data-driven sections.
- Add admin update workflow (protected content management path).

### Phase 3: UX and accessibility

- Implement real mobile nav drawer/menu behavior.
- Improve accessibility audit coverage:
  - focus states
  - keyboard nav
  - landmarks/headings
  - image alt quality
- Add SEO improvements per page:
  - unique title/meta tags
  - social preview metadata
  - structured data where relevant

### Phase 4: Performance and operations

- Optimize large images (compression + responsive sizes).
- Add CI pipeline for lint/build/test checks.
- Add deployment preview workflow for pull requests.
- Add observability:
  - client error tracking
  - lightweight analytics
  - uptime checks

## Suggested directory map

```text
src/
  main.tsx                # bootstrap
  App.tsx                 # providers + route table + Chakra theme tokens
  index.css               # global Tailwind theme variables
  pages/                  # route-level pages
  components/
    layout/               # Navbar, Footer, Layout shell
    ui/                   # shadcn/ui component library
  assets/                 # local images and logos
  hooks/                  # shared hooks
  lib/                    # shared utilities
public/
  _redirects              # SPA redirects for static hosting
```
