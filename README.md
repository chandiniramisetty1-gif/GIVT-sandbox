# GIVT Sandbox

**Gamified · Individualized · Verified Talent**
Seven-agent educational workforce platform — Kennesaw State University · Healthcare Informatics MVP · 2026.

A single-page React application (Vite) implementing the seven GIVT agents:

1. **Translator** — résumé ↔ desired job description capability-gap analysis + JD-language résumé translation.
2. **Talent** — employer profiling (company confirmation, AI + web-search profile, use cases, talent demand).
3. **Curriculum** — maps current courses to use cases, proposes future curriculum.
4. **Advisor** — three learning pathways, directed-study syllabi, professor supervision, token exchange ledger.
5. **Reputation** — on-résumé skill verification by stakeholders, scoring, leaderboard.
6. **Generator** — forward-looking curriculum modules + downloadable curriculum recommendation document (GAN loop seed).
7. **Discriminator** — compliance critique, standard curriculum guideline, Generator↔Discriminator equilibrium loop.

Plus a dashboard **account system** (Create Account, 500-token reward, Hedera address, upload/write profile, 250-word profile generation).

---

## Quick start (VS Code / local)

Requires **Node.js 18+** (Node 20 recommended).

```bash
npm install
npm run dev
```

Open the printed URL (default http://localhost:5173).

### Build for production

```bash
npm run build      # outputs to dist/
npm run preview    # serve the production build locally
```

---

## Run on Replit

1. Create a new Repl → **Import from GitHub** (or upload this folder / the provided ZIP).
2. Replit reads `.replit` and `replit.nix` automatically. Click **Run**.
   - It runs `npm run dev` on port 5173, exposed on port 80 via the webview.
3. First run installs dependencies automatically. If not, open the Shell and run `npm install`.

> If the webview shows a "host not allowed" message, it's already handled by
> `server.allowedHosts: true` in `vite.config.js`. Just reload the webview.

---

## Push to GitHub

```bash
git init
git add .
git commit -m "GIVT Sandbox initial commit"
git branch -M main
git remote add origin https://github.com/<you>/givt-sandbox.git
git push -u origin main
```

`node_modules/` and `dist/` are already in `.gitignore`.

---

## Project structure

```
givt-sandbox/
├── index.html            # Vite entry HTML
├── package.json          # scripts + dependencies (React 18 + Vite 5)
├── vite.config.js        # dev/preview server config (Replit-friendly)
├── .replit               # Replit run/deploy config
├── replit.nix            # Replit Node 20 toolchain
├── .gitignore
├── README.md
├── context.md            # full spec to regenerate this app in a new AI session
└── src/
    ├── main.jsx          # React DOM root
    ├── index.css         # minimal global styles
    └── App.jsx           # the entire GIVT Sandbox app (single-file component)
```

The whole application lives in **`src/App.jsx`** (default export `App`). It uses only
React + hooks and inline styles — no UI framework — and loads Google Fonts and the
`mammoth` / `pdf.js` parsers from a CDN at runtime.

---

## A note on AI features (live API calls)

Two features call the Anthropic Messages API directly from the browser:

- **Talent** → "Generate company profile" (uses web search)
- **Account** → "Update profile · generate 250-word profile"

In the Claude artifact environment these calls are proxied automatically. In a
standalone deployment (local / Replit / GitHub Pages) a direct browser call to
`api.anthropic.com` will fail (CORS + no key), and the app **gracefully falls back**
to built-in heuristic / template output — everything else works fully offline.

To enable live AI in production, add a small server-side proxy that injects your
`ANTHROPIC_API_KEY` and forward the request to `https://api.anthropic.com/v1/messages`,
then point the two `fetch(...)` calls in `src/App.jsx` at your proxy URL.

---

## Regenerating this app from scratch

Open a new AI session and provide **`context.md`** (included here). It contains the
complete specification — design tokens, constants, every agent's behavior, scoring
formulas, the account system, and the GAN convergence targets — needed to rebuild the
exact same interface. For a byte-exact copy, just reuse `src/App.jsx`.
