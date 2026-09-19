# WC3 DotA Storefront

React and TypeScript storefront prototype with 3D previews, BattleTag checkout, and admin order tools.

**Public portfolio source:** [quanleok/wc3-dota-portfolio](https://github.com/quanleok/wc3-dota-portfolio)

## Project overview

**Stack:** React, TypeScript, Vite and Stripe.

**Implemented work:** Product browsing, 3D previews, checkout flows, and admin order interfaces.

**Status and limits:** Order records and BattleTag examples are fictional demo fixtures. The storefront relies on an external asset workspace, which is not included here. It is not a production-readiness claim. Use only assets you own or have permission to use; this source does not grant third-party asset rights.

## Development documentation

A Warcraft III Reforged storefront for DotA custom-map cosmetics.

The app repo is kept separate from the large DotA map and asset workspace:

- app/site repo: `<project-root>`
- DotA asset/map workspace: `../dota`
- local site asset payload: `../dota/site-assets/w3dotashop-public`

The prototype models an entitlement flow:

- player chooses a skin or future digital item
- checkout captures the player BattleTag
- admin or host bot reads the order payload
- an external host-bot or map integration would grant the matching entitlement

## What is in this repo

- storefront interfaces for cosmetic products
- BattleTag-first checkout form
- admin login surface for partner / bot-hosting operations
- Stripe Checkout Session endpoint scaffold
- admin auth/session endpoints scaffold
- demo fallback mode for local development before live secrets are connected

## Environment variables

### Frontend

```bash
VITE_DEMO_MODE=true
VITE_ASSET_BASE_URL=
VITE_LOCAL_ASSET_ROOT=../dota/site-assets/w3dotashop-public
```

`VITE_DEMO_MODE=true` keeps local checkout and admin flows usable before live backend secrets exist.
`VITE_ASSET_BASE_URL` can point production builds at an external asset host. Local dev and preview use `VITE_LOCAL_ASSET_ROOT` to serve the moved models, audio, generated images, and WC3 textures without storing them inside the app repo.

### Backend / API

```bash
STRIPE_SECRET_KEY=
PUBLIC_BASE_URL=http://localhost:5173

ADMIN_USERNAME=
ADMIN_PASSWORD=
ADMIN_SESSION_SECRET=

BOT_ORDERS_API_URL=
BOT_ORDERS_API_TOKEN=

EVOLINK_API_BASE_URL=https://api.evolink.ai/v1
EVOLINK_API_KEY=
EVOLINK_SUNO_MODEL=suno-v5-beta

STRIPE_PRICE_SF_BONE_EMPEROR=
STRIPE_PRICE_PUDGE_PLAGUE_TITAN=
STRIPE_PRICE_INVOKER_DARK_STAR=
STRIPE_PRICE_ZEUS_STORM_KING=
STRIPE_PRICE_ORACLE_STAR_PRIEST=
```

## Local development

```bash
npm ci
cp .env.example .env.local
npm run dev
```

In demo mode:

- checkout stores a fake local order if Stripe is not configured
- admin can use the demo credentials shown in the UI

Run `npm run lint` and `npm run build` for source checks. The external asset workspace is not bundled: previews and media need separately supplied assets, and the build can report a missing external UI-sheet image. API routes require a compatible server runtime; the Vite development server alone is not a complete live backend.

## Audio asset generation

Portal sound effects can be generated as review candidates with Evolink Suno v5:

```bash
npm run assets:audio:suno
npm run assets:audio:suno -- --run gate-open-rumble
npm run assets:audio:suno -- --run --all --install
```

Dry run is the default. Put `EVOLINK_API_KEY` in `.env.local` before using `--run`; do not commit it. Generated candidates are written outside the app repo under `../dota/site-assets/w3dotashop-public/audio/portal/suno-v5`. `--install` copies the selected candidate into the canonical `/audio/portal` filenames.

## Integration requirements

Live integrations require further setup and validation:

1. Set the Stripe secret and per-product Stripe price IDs.
2. Set real admin credentials and an admin session secret.
3. Point `BOT_ORDERS_API_URL` at the host-bot / entitlement service.
4. Disable demo mode in production.

These settings alone do not establish production readiness. The production admin API refuses to use the development signing fallback when its session secret is missing.

## Notes

- This repo does not directly grant in-game skins. It prepares and authenticates the purchase flow.
- The actual entitlement push still belongs to the host bot / backend partner service.

## Public source boundary

This repository includes storefront code, synthetic order fixtures, asset filename
manifests and a standalone SpriteForge HTML utility. The utility ZIP excludes
macOS metadata. Developer-specific macOS launcher downloads are not included.
Private assets, customer records, account recovery files and live credentials
must stay outside Git. The visible demo admin login only controls synthetic
browser data; live admin APIs require their own server environment credentials.
