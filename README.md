# Jay Wix Store — Light

A minimal e-commerce storefront built with [Jay Framework](https://github.com/nicenemo/jay) and Wix Stores. Features product listing, product detail pages with variants, and a shopping cart.

## Prerequisites

- Node.js >= 20
- A Wix site with Wix Stores installed

## 1. Setup & Dev Server

```bash
# Install dependencies
npm install

# Copy credential templates
cp config/.wix.yaml.example config/.wix.yaml
# Edit config/.wix.yaml with your Wix API key, site ID, and client ID

# Validate setup
npm run setup

# Start development server
npm run dev
```

The dev server starts at `http://localhost:5173` with hot reload.

## 2. AI Designer (Aiditor)

The project includes the Jay AI designer for editing pages visually with AI assistance.

```bash
npm run dev
```

Then open `http://localhost:5173/aiditor` in your browser.

The aiditor reads the project's contracts and page templates, letting you modify layouts and add components using natural language.

## 3. Deploy to Wix BaaS

### First-time setup

```bash
# Create a Wix headless site (generates wix.config.json)
npm create @wix/new@latest init

# Authenticate with Wix CLI
wix login

# Create a data collection named "jay-backend-files" in the Wix dashboard
# Required fields: path (text), content (text), fileType (text),
#                  sizeBytes (number), category (text), version (text)

# Validate everything is configured
npm run setup
```

### Deploy

```bash
npm run build:production
npm run deploy
```

This bundles a 2.5 MB `entry.mjs`, uploads page data to a Wix data collection, and deploys the server + frontend to Wix BaaS + CDN.

## 4. Deploy to Self-Hosted Server

```bash
# Build
npm run build:production

# Serve with the built-in production server
npm run serve
```

Or use the fetch handler directly in any Node.js server:

```typescript
import { createJayFetchHandler } from '@jay-framework/jay-fetch-handler';

const handler = createJayFetchHandler({
  backendDir: './build/v0.18.0/backend',
  staticBaseUrl: '/',
  frontendDir: './build/v0.18.0/frontend',
});

// Use with any HTTP server framework
```

## Project Structure

```
src/
├── pages/
│   ├── page.jay-html                    # Homepage
│   ├── products/
│   │   ├── page.jay-html                # Product listing
│   │   ├── [slug]/page.jay-html         # Product detail (dynamic)
│   │   └── ceramic-flower-vase/         # Product detail (static override)
│   │       └── page.jay-html
│   └── cart/
│       └── page.jay-html                # Shopping cart
└── styles/
    └── atelier-theme.css                # Theme styles
```

Pages use `.jay-html` templates with headless component bindings — no JavaScript needed for data fetching, SSR, or client hydration. The framework handles all of that through contracts and plugins.

## Credentials

| File | Purpose |
|------|---------|
| `config/.wix.yaml` | Wix SDK access (API key, site ID) — used at build time and runtime |
| `wix.config.json` | Wix BaaS deployment target (app ID, site ID) — used at deploy time |

These can point to different sites in a headless architecture. See the [deployment guide](https://github.com/nicenemo/jay) for details.
