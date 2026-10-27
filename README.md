# The Thought Archive — Live v12

A Node.js (20+) website + backend in one service. No build system or npm dependencies.

## Run locally

`npm start` then visit `http://localhost:3000`.

## Deploy using GitHub + Render (iPad compatible)

1. Create a new GitHub repository (private is fine). Upload **package.json**, **server.js**, **render.yaml**, and **public/index.html**, preserving the `public` folder.
2. In Render, create a **Web Service** from that repository. Runtime **Node**, build command `npm install`, start command `npm start`, health path `/api/health`.
3. Choose an available service plan. Render assigns an HTTPS address. Open it and check `/api/health`.
4. Optionally set `OPENALEX_API_KEY` in Render Environment (obtain a key through OpenAlex). Restart/redeploy. The research provider may reject unauthenticated calls or apply quotas.
5. Test the home page: Save → automatic browser reload → saved tile stays on top; I'm Ready For Something New → previous unsaved IDs retired → new 30 if available.

### Existing Netlify bookmarks

Browser localStorage is scoped to each website origin. The Render URL **cannot automatically read bookmarks from your Netlify URL**. To keep your existing saved items, use the same hostname via a Netlify frontend pointing to Render API, or export/import browser data. This package preserves legacy key `thoughtArchiveSaved` and embeds old entries, but only if served on the **same origin**.

### Technical limits

- Live data comes from Gutendex's Project Gutenberg catalog and OpenAlex scholarly metadata.
- Gutenberg text is fetched and a paragraph extracted directly; text availability, rights, and exact section locators vary.
- Research text is a short excerpt of the indexed abstract, **not a verified scientific conclusion** or a substitute for reading the study. Abstract access and reuse permissions vary.
- Not every Gutenberg title has a usable plaintext edition; this does **not** ingest the entire archive.
- A request may return fewer than 30 results; upstream services can fail or rate-limit.
- Saved and retired IDs live in the visitor's browser; no login, cloud sync, or server-side persistent user database.
- Repeated ideas are excluded using locally retired IDs sent with each request. Very long histories exceed practical URL limits; production needs a server-side database for strict lifetime non-repetition.
- OpenAlex may require a free API key or charge for some usage. Check its current terms.

## API

`GET /api/health`

`GET /api/discover?count=30&seed=1&exclude=pg-123,oa-W123`

The discovery endpoint returns `items`, `errors`, `returned`, `exhausted`. No fabricated quotations or model-generated passages.
