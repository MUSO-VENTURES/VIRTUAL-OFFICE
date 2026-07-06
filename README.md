# VIRTUAL-OFFICE

Internal MUSO VENTURES tools, hosted as static web apps. `index.html` at the root is a landing page linking out to each app.

## Apps

- **`/muso-contacts/`** — New Contact Assistant. Offline-capable PWA for logging new contacts, scanning business cards, and drafting follow-ups. See its own README for details.

## Hosting with real access control (not GitHub Pages)

GitHub Pages only supports private-repo hosting on paid plans (Pro/Team/Enterprise) — on a Free account, any Pages site is public regardless of the repo's visibility. Since this repo should only be visible to Shane and invited collaborators, use **Cloudflare Pages + Cloudflare Access** instead. Both are free for this use case (Access is free for up to 50 users) and give you real per-person login, not just a shared password:

1. **Deploy the site**
   - Go to the Cloudflare dashboard → Workers & Pages → Create → Pages → Connect to Git.
   - Authorize Cloudflare's GitHub App and select the `VIRTUAL-OFFICE` repo (keep it private on GitHub — Cloudflare can still pull from it).
   - Build settings: none needed — this is a static site. Leave the build command blank and set the output directory to `/` (repo root).
   - Deploy. You'll get a URL like `virtual-office.pages.dev`.

2. **Lock it down with Cloudflare Access**
   - In the Cloudflare dashboard → Zero Trust → Access → Applications → Add an application → Self-hosted.
   - Point it at your `*.pages.dev` domain (or a custom domain if you add one later).
   - Create a policy: Action = Allow, Include = "Emails" and list your own email (and later, any collaborator's email) — or use "Emails ending in" if everyone shares a company domain.
   - Save. Now anyone hitting the URL is prompted to verify their email via a one-time code before they see anything — no password to share or leak.

3. **Adding a collaborator later**: Zero Trust → Access → Applications → your app → Policies → add their email. Done — no redeploy needed.

4. **Every future `git push` to `VIRTUAL-OFFICE`** auto-deploys through Cloudflare Pages, so updates (new apps, edits to `muso-contacts/`) go live automatically, still behind the same access gate.

### Why not just password-protect it with client-side JavaScript?

A password check written in the page's own JavaScript isn't real security — anyone can view the page source or the network tab and read the data/app straight past the prompt. Cloudflare Access sits in front of the server and blocks the request before any of the site's files are ever sent, which is what actually restricts access.

## Data note

`muso-contacts/seed-contacts.json` contains real MUSO_Contacts.xlsx data (business names, phone numbers, emails). That's fine as long as this repo and its deployed site stay access-gated as above — don't flip this to a plain public GitHub Pages site without stripping that file first.
