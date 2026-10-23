# SHOP×GRAM — Founder's Pocket (v8 MOBILE / PWA)

Mobile-first rebuild of **SHOP×GRAM Founder's Desk v7 PRO** (desktop) as an installable
Progressive Web App for phones in portrait orientation. Single-file game
(`index.html`, inline CSS + vanilla JS), no build step, no frameworks, **no AI of any
kind** — every image, text, decision and event is procedural, deterministic or
rule-based (seeded canvas art, templates, hand-written copy).

## What's in this folder

| File | Purpose |
|---|---|
| `index.html` | The whole game (HTML + CSS + JS, one file) |
| `manifest.json` | PWA manifest (name, colors, portrait, icons) |
| `service-worker.js` | Offline app-shell cache + system notifications + notification clicks |
| `icons/` | `icon-192.png`, `icon-512.png`, `maskable-512.png`, `apple-touch-icon.png` (procedurally drawn, matching the paper/ink brand) |
| `README.md` | This file |

## How to run / install on a phone

The service worker (offline + notifications + installability) requires **HTTPS or
localhost** — opening `index.html` straight from storage runs the game but disables
the PWA layer.

- **Easiest:** drop the folder on any static host (GitHub Pages, Netlify Drop,
  Cloudflare Pages, Vercel…). Open the URL on your phone → browser menu →
  *Install app / Add to Home Screen*.
- **Local testing:** `npx serve .` or `python3 -m http.server 8000` inside this
  folder, then visit `http://localhost:8000` (desktop) or your machine's LAN IP
  over an HTTPS tunnel (e.g. `npx localtunnel`, Cloudflare tunnel) for a real phone.
- Inside the game: **MORE → INSTALL APP** explains the per-browser steps.

Saves live in `localStorage` on the device and write automatically every 5 s.

## Controls (mobile layout)

- **Bottom tabs:** STORE · BUY (wholesale) · ORDERS · BANK · MORE.
- **MORE sheet:** Stats, Creators, Global, Rivals, Franchise, Upgrades, Market,
  Staff, Settings (+ install & export shortcuts).
- **Top bar:** balance (tap → Stats), economy chip, day/clock, speed ⏸/1×/3×/5×,
  notification bell with unread badge.
- **Ticker strip** under the top bar = thin marquee; tap it (or the bell) for the
  scrollable notification feed.
- Every dialog is a **bottom sheet** (drag the handle down, tap the backdrop, or ✕
  to dismiss). Nothing is hover-dependent; all touch targets ≥ 44 px.
- Safe-area insets (notch / home indicator) are respected via `env(safe-area-inset-*)`.

## Notifications

- In-app: toasts + bell badge + persistent event feed (tapping an entry jumps to the
  relevant screen).
- System (lock-screen) push: **Settings → SYSTEM NOTIFICATIONS → ENABLE**, or the
  one-time banner that appears in Orders after your first order. Permission is only
  ever requested from those user actions. Delivered through the service worker
  (`registration.showNotification`); clicking one reopens the game.
- Per-category switches in Settings: Orders · Complaints · Staff & payroll ·
  Stock & supply · Money · Growth. Covered events include new order, order expiring,
  delivered, lost, complaint opened/escalated/fraud, payroll due/paid, staff quit /
  poached, low/out of stock, VIP supplier drop, campaign ended, daily taxes, loan
  auto-pay made/missed, credit-score change & freeze, R&D complete, franchise income,
  economy phase change, milestones, new rivals.
- If permission is denied or unsupported, the game silently falls back to in-app
  toasts + badges.

## Feature parity with the desktop v7 PRO

Products & lifecycle stages, procedural post art, pricing/market-fit math, ad &
creator campaigns with fatigue/frequency, wholesale market with drifting prices and
rush supplier, VIP supplier drops + personal R&D catalog, warehouse capacity &
inventory-manager automation, orders with carriers/delays/loss/escrow receipts,
complaints & trust penalties, customer reviews, staff roles with wages/inflation/
poaching, upgrades, bank loans with daily auto-pay & credit score, stock market with
live P&L, R&D lab, competitors & market share, economy cycles, global markets with
branches (tap-able canvas world map + custom branch building), franchises, prestige
& founder points, comprehensive JSON export (download / share / copy), localStorage
save-resume.

## Desktop → mobile change list (summary)

1. Removed the `min-width:1280px` body rule and the "DESKTOP ONLY" gate; added
   `width=device-width, initial-scale=1, viewport-fit=cover`.
2. Sidebar nav → 5-tab bottom bar + "More" bottom sheet; right rail → collapsible
   "Stats" cards.
3. Desktop modals → bottom sheets with drag-to-dismiss; tables → stacked cards.
4. Leaflet world map (CDN script + tile server) → dependency-free canvas map with
   tap hit-testing; market/branch management also fully list-driven.
5. `window.confirm/prompt` dialogs → confirmation sheets & stepper sheets.
6. New notification system (toasts, badges, feed, per-category toggles, Notification
   API via service worker, permission-on-user-action, graceful fallback).
7. Ticker → 24 px marquee strip that doubles as a shortcut to the notification feed.
8. Touch ergonomics: 44 px minimum targets, 16 px inputs (no iOS zoom), tap states
   instead of hover, safe-area padding, `overscroll`/double-tap-zoom control.
9. Performance for phones: batched rAF renders, 4 Hz live ticks instead of per-frame
   DOM writes, smaller cached canvas art (480 px), lazy `loading` images, reduced
   motion support.
10. Save format `v8` (own key) with migration-safe merge; prestige key namespaced.
    Deleting a listing now returns its stock to the warehouse.

## Maintenance notes

- Bump `CACHE` in `service-worker.js` when shipping updates so installed clients
  refresh the app shell.
- The Google Fonts link is the only network dependency; offline play falls back to
  system fonts automatically.
