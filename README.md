# C Zombie Coder – deploy on Vercel

Files: `index.html` (the game) and `api/lb.js` (shared leaderboard backend).

## Setup
1. Push this folder to GitHub, then **Add New → Project** on vercel.com and deploy it.
2. Project → **Storage** → add **Upstash Redis** (Marketplace) and connect it to the project. Env vars are added automatically (`KV_REST_API_*` or `UPSTASH_REDIS_REST_*`).
3. **Redeploy**, then open `/api/lb?level=1`. You should see `{"rows":[]}`.

## How accounts work
- No login. A name is claimed by a secret token stored on the student's device; only its hash is saved.
- Names are unique (case-insensitive). A taken name is refused.
- Students can rename themselves (✏️). Their scores move to the new name.
- Scores can only rise, and only as fast as real play allows, so one-shot fake scores are rejected.
- If a student clears the browser data, their name stays locked until you free it (below).

## Teacher tools
Set an `ADMIN_KEY` env var and redeploy, then:
```
curl -X DELETE -H "x-admin-key: KEY" "https://YOUR-SITE.vercel.app/api/lb?name=Riya"     # remove one student, frees the name
curl -X DELETE -H "x-admin-key: KEY" "https://YOUR-SITE.vercel.app/api/lb?level=all"     # clear all scores (names stay reserved)
```
