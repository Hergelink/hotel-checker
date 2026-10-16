# Titcotour Hotel Check — Vercel Version

A lightweight internal site: paste hotel name, hotel URL, Google Maps link, or Google Search URL, then request an AI review assessment backed by web search. No database and no build step.

## Deploy using GitHub and Vercel

1. Sign in to [GitHub](https://github.com) and create a **private** repository (for example `titcotour-hotel-check`).
2. Click **Add file → Upload files** in GitHub. From this extracted folder, upload `index.html`, `vercel.json`, `.gitignore`, `README.md`, AND the `api` directory (or create `api/check.js` with that exact path). Commit.
3. Sign in to [Vercel](https://vercel.com/new). Choose **Add New → Project → Import Git Repository**, connect GitHub, and select the repository. Framework preset: **Other**. No build command, no output directory; the entrypoint is `index.html` at repository root.
4. Before deploying, configure **Environment Variables** in the Vercel project:
   - `OPENAI_API_KEY` — API key from https://platform.openai.com/api-keys
   - `STAFF_PASSWORD` — a unique password, at least 12 characters
   - `OPENAI_MODEL` — optional; defaults to `gpt-5.4`
   Set them for Production (and Preview only if needed).
5. Click **Deploy**, visit the generated `*.vercel.app` URL, sign in and test with a hotel name including a city.
6. If variables are added after initial deployment, **Redeploy** from Vercel's deployments page.

**IMPORTANT:** Vercel's free Hobby plan is restricted to non-commercial uses. For Titcotour corporate/internal use you need an eligible paid Vercel plan, or adapt the code to a provider that permits business use on a free tier.

## Security

- API key and staff password live in Vercel Environment Variables, not files in the repository or browser. Do not create `config.php`.
- A signed, HttpOnly, Secure, SameSite=Strict cookie expires after 8 hours. Do not share your password publicly.
- For sensitive production operation, enable WAF/rate limiting/access controls; password guessing and repeated searches can incur cost. This small prototype does not have account-specific access controls or persistent rate limiting.
- Restrict OpenAI API spend using your project-level usage controls, and monitor billing.
- The app does not fetch submitted URLs itself: they are untrusted identification hints passed to AI web research.
- AI may not access all review websites or recent comments; the results are not a safety guarantee. Always review linked evidence.

## Project files

- `index.html`: Website UI
- `api/check.js`: Serverless OpenAI API call and staff authentication
- `vercel.json`: Function runtime limit

## Troubleshooting

- **Setup required**: Missing Environment Variables, or `STAFF_PASSWORD` is too short; redeploy after editing variables.
- **Login fails**: Check password and that production variables were set correctly.
- **Research timed out**: Try again, or check provider runtime limits and OpenAI dashboard usage.
- **OpenAI error**: Check API key, billing, model availability, and web-search API access.
- **404 on /api/check**: Make sure `api/check.js` exists directly under the root `api` folder.
