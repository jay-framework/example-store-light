# Jay Wix Store — Light

A minimal e-commerce storefront built with [Jay Framework](https://github.com/nicenemo/jay) and Wix Stores. Features product listing, product detail pages with variants, and a shopping cart.

## Prerequisites

- Node.js >= 20
- A Wix site with Wix Stores installed

## 1. Setup

```bash
npm install
npm create @wix/new@latest init
npm run setup
```

Here's what each command does:

1. **`npm create @wix/new@latest init`** — Creates a new headless Wix site, generating a `wix.config.json` with:
   - `siteId` — the Wix site identifier (also known as metasite ID in Wix)
   - `appId` — a client ID for headless API access

   This command is only required for Wix-hosted sites.

2. **`npm run setup`** — Creates `config/.wix.yaml`. If a valid `wix.config.json` exists, it reads the `siteId` and `clientId` from it.

   > **Note:** `appId` (in `wix.config.json`) and `clientId` (in `config/.wix.yaml`) are the same value. Both can also be generated manually from **Wix Dashboard → Site Settings → Headless Settings → Headless Client**.

3. **Generate an API key** — Go to **Wix Dashboard → Account Settings → API Keys**, create a new key, and paste it into `config/.wix.yaml` under `apiKeyStrategy.apiKey`.

4. **Run `npm run setup` again** to validate that all credentials are configured correctly.

### Why two credential files?

| File               | Purpose                                                                                                                             |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| `wix.config.json`  | Used for **Wix BaaS deployment** — only needed on the server where the application is deployed. Required only for Wix-hosted sites. |
| `config/.wix.yaml` | Used to **connect to Wix backend** services (Wix Data, Wix Stores, etc.) at build time and runtime.                                 |

Having two separate files allows deploying multiple versions of the business as different BaaS instances. By default, both point to the same site.

## 2. Dev Server

```bash
npm run dev
```

The dev server starts at `http://localhost:5173` with hot reload.

## 3. AI Designer (Aiditor)

The project includes the Jay AI designer for editing pages visually with AI assistance.

```bash
npm run dev
```

Then open `http://localhost:5173/aiditor` in your browser.

The aiditor reads the project's contracts and page templates, letting you modify layouts and add components using natural language.

## 4. Deploy to Wix BaaS

### First-time setup

```bash
# Authenticate with Wix CLI
wix login

# Create a data collection named "jay-backend-files" in the Wix dashboard
# Required fields: path (text), content (text), fileType (text),
#                  sizeBytes (number), category (text), version (text)
```

### Deploy

```bash
npm run build:production
npm run deploy
```

This bundles a 2.5 MB `entry.mjs`, uploads page data to a Wix data collection, and deploys the server + frontend to Wix BaaS + CDN.

## 5. Deploy to Self-Hosted Server

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

