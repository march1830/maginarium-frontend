# Maginarium Frontend

A minimal Vue 3 + Vite + TypeScript starter for the Maginarium game UI, preconfigured for deployment on Vercel.

## Prerequisites
- Node.js 18+ (recommended LTS)
- npm 9+ (or pnpm/yarn if you prefer)

## Getting Started
1. Install dependencies:
   ```bash
   npm install
   ```
2. Start the dev server:
   ```bash
   npm run dev
   ```
   Vite will print the local URL (usually http://localhost:5173).

3. Type-check (optional during dev):
   ```bash
   npm run type-check
   ```

## Build
Create an optimized production build in `dist/`:
```bash
npm run build
```

Preview the production build locally:
```bash
npm run preview
```

## Project Structure
- `src/` — application source (Vue components, styles, entry)
- `public/` — static assets served at the app root (e.g., `/favicon.svg`)
- `index.html` — HTML entry
- `vite.config.ts` — Vite configuration
- `tsconfig*.json` — TypeScript configurations
- `vercel.json` — Vercel deployment configuration

## Deployment on Vercel
This project is ready for zero-config deployment on Vercel.

- Import the repo in Vercel and select the root directory.
- Framework preset: Vite (auto-detected by `framework: "vite"`).
- Build command: `npm run build`
- Output directory: `dist`

Vercel rewrites are configured to serve SPA routes via `index.html`.

### Environment Variables
- Add any required variables in Vercel Project Settings → Environment Variables.
- Locally, create an `.env` file (ignored by Git). Vite exposes variables starting with `VITE_` to the client.

## Aliases
Use `@/` to import from `src/` thanks to the path alias defined in `tsconfig.json`.

## Next Steps
- Replace the placeholder components with real Maginarium UI.
- Add a router (`vue-router`) if you need multiple pages.
- Add state management (e.g., `pinia`) as the game grows.
- Set up testing (e.g., Vitest + Vue Test Utils) when ready.