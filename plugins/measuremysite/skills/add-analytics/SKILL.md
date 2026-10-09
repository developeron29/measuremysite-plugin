---
name: add-analytics
description: Adds measuremy.site cookieless web analytics to a website project (Next.js, Astro, Vite/React/Vue, SvelteKit, Hugo, WordPress, plain HTML and others), tracks signups and other conversions, and answers questions about the site's traffic. Use when the user asks to add analytics or visitor tracking, replace Google Analytics, track signups or clicks, or asks how their site's traffic, a launch or a campaign is doing.
---

# Add measuremy.site analytics

measuremy.site is one script tag under 1 KB: no cookies, no npm package. Its MCP server, `measuremysite`, gives you the site's snippet and its traffic. The free plan covers 5,000 pageviews a month.

## 1. Make sure the MCP server is connected

Look for the `measuremysite` MCP tools (`get_setup`, `get_overview`, ...).

- **Tools available:** go to step 2.
- **Server listed but not signed in:** tell the user to run `/mcp`, pick `measuremysite` and sign in. A browser page opens where they enter their email and domain. Nothing secret passes through you.
- **No server at all** (skill used without the plugin): run `claude mcp add --transport http measuremysite https://a.measuremy.site/mcp`, then ask the user to restart (`claude -c` keeps the conversation) and sign in.

## 2. Get the snippet

Call `get_setup`. It returns the site's `snippet` (with its public `data-key`), `where` it goes for each framework, CSP advice, and whether the domain is verified. Use the snippet exactly as returned; don't invent a key.

## 3. Add it to the project

Find the file that renders `<head>` for every page and add the snippet once:

| Project | File |
|---|---|
| Next.js App Router | `app/layout.tsx`, inside `<head>` |
| Next.js Pages Router | `pages/_document.tsx`, inside `<Head>` |
| Vite / React / Vue / Svelte SPA | `index.html` at the project root |
| SvelteKit | `src/app.html`, before `%sveltekit.head%` |
| Astro | base layout, e.g. `src/layouts/Layout.astro`, as `<script is:inline defer ...>` |
| Hugo | head partial under the project's own `layouts/` (don't edit the theme) |
| Other | see `where` in the `get_setup` result |

For WordPress, Webflow, Shopify and other site builders, the tag goes in the platform's admin, not in code: give the user the snippet and the steps from `https://measuremy.site/docs/install`.

Client-side navigation is tracked automatically. If the site sets a Content-Security-Policy, add `https://a.measuremy.site` to `script-src` and `connect-src`.

## 4. Track the conversion that matters

Find where signup (or purchase, or the key action) succeeds and add:

```js
window.measuremysite && measuremysite.track("signup", { plan: "free" });
```

For a plain link, button or form, an attribute is enough: `data-measuremysite-event="signup_click"`. Use short snake_case names and never put emails or names in props.

## 5. Verify

Visits from `localhost` aren't counted. After the user deploys, the first visit to the live domain verifies it. Call `get_setup` again: `verified` turns true and `next_step` says what is left.

Skip `get_setup`'s `slash_commands`: this plugin already provides `/measuremysite:make-public` and `/measuremysite:make-private`.

## Answering questions about traffic

Start with `get_overview`. Use `find_traffic_spikes` and `explain_spike` for "what happened", `query_visits` for breakdowns by country, browser, source, page or event, `get_funnel` for drop-off, and `get_whats_working` with `get_growth_actions` before suggesting marketing. For anything the user will publish, call `create_tracked_link` first and measure it later with `get_campaign_results`.
