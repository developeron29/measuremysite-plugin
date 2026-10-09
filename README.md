# measuremy.site for Claude Code

Cookieless web analytics your AI agent can read. This plugin connects Claude Code to the measuremy.site MCP server and adds:

- `measuremysite:add-analytics`: adds the tracker to your project (Next.js, Astro, Vite, SvelteKit, Hugo, WordPress, plain HTML...), tracks signups, and answers questions about your traffic. Claude picks it up when you ask to "add analytics".
- `/measuremysite:make-public` and `/measuremysite:make-private`: share your dashboard with a public link, or turn it off.

## Install

```
/plugin marketplace add developeron29/measuremysite-plugin
/plugin install measuremysite@measuremysite
```

Then run `/mcp`, pick `measuremysite` and sign in. A browser page opens where you enter your email and domain; no API key is pasted anywhere. 30-day free trial, no card; plans from $9/month. Showing the optional "Analytics by measuremy.site" badge on your site adds 50% to your monthly pageviews.

Docs: https://measuremy.site/docs/mcp
