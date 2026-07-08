# Humanly — Hosting & Sign-in Setup

This folder contains everything needed to host **Humanly** on GitHub Pages
and get sign-in (magic link + Google) working.

Files:
- `index.html` — the whole app (this is your site)
- `.nojekyll` — tells GitHub Pages not to process the files (leave it, don't delete)
- `404.html` — sends any stray link back to the app
- `README.md` — this file

---

## Part 1 · Put the site on GitHub Pages

You've used GitHub Pages before, so this will feel familiar.

1. Go to https://github.com and click **New repository** (the green button, or the **+** top-right).
2. Name it something like `humanly` and set it to **Public**. (Pages needs Public on the free tier.)
   - Don't add a README from GitHub — you already have one here.
3. On the new empty repo page, click **uploading an existing file**.
4. Drag **all four files** from this folder into the upload box:
   `index.html`, `.nojekyll`, `404.html`, `README.md`.
   - ⚠️ Make sure `.nojekyll` uploads too. If your file browser hides it (the leading dot),
     you may need to enable "show hidden files," or just don't worry — the site still works
     without it in most cases, it just helps avoid odd caching.
5. Click **Commit changes**.
6. Go to the repo's **Settings** tab → **Pages** (left sidebar).
7. Under **Build and deployment → Source**, choose **Deploy from a branch**.
8. Set **Branch** to `main` and folder to `/ (root)`, then **Save**.
9. Wait ~1–2 minutes. Refresh the Pages settings screen — it will show:
   **"Your site is live at https://YOUR-USERNAME.github.io/humanly/"**

👉 **Copy that URL.** You need it for Part 2. It will look like:
`https://skye-example.github.io/humanly/`

Test it: open the URL. The app should load. (Sign-in won't work yet — that's Part 2.)

---

## Part 2 · Tell Supabase to trust your new URL

Your database is already live (project: **Humanly**,
`qaminbhfgkkjlbwlatvw.supabase.co`). It just needs to know your site's address.

1. Go to https://supabase.com/dashboard and open the **Humanly** project.
2. Left sidebar → **Authentication** → **URL Configuration**.
3. **Site URL** — paste your GitHub Pages URL, including the trailing slash:
   `https://YOUR-USERNAME.github.io/humanly/`
4. **Redirect URLs** — click **Add URL** and add BOTH of these:
   - `https://YOUR-USERNAME.github.io/humanly/`
   - `https://YOUR-USERNAME.github.io/humanly/**`
   (The `**` wildcard covers any sub-path. Add both to be safe.)
5. Click **Save**.

✅ **Magic-link email sign-in now works.** Open your site, go to sign in,
enter your email, click the link it sends you. (See note on email limits below.)

---

## Part 3 · Enable Google sign-in (optional — do Part 2 first)

This is the step that fixes the *"provider is not enabled"* error.
It has two halves: create a Google credential, then paste it into Supabase.

### 3a · Create the Google OAuth credential

1. Go to https://console.cloud.google.com
2. Top bar → project dropdown → **New Project** (name it "Humanly"), then select it.
3. Left menu → **APIs & Services** → **OAuth consent screen**.
   - User type: **External** → **Create**.
   - App name: `Humanly`. Add your email where required. Save through the steps.
     (You can leave scopes default and skip test users for now.)
4. Left menu → **APIs & Services** → **Credentials**.
5. **+ Create Credentials** → **OAuth client ID**.
6. Application type: **Web application**.
7. Under **Authorized redirect URIs**, click **Add URI** and paste your Supabase
   callback URL (Supabase shows you this exact string on the Google provider page —
   it looks like):
   `https://qaminbhfgkkjlbwlatvw.supabase.co/auth/v1/callback`
8. Click **Create**. Google shows you a **Client ID** and **Client Secret** — keep this tab open.

### 3b · Paste it into Supabase

1. Supabase dashboard → **Authentication** → **Providers** → **Google**.
2. Toggle **Enable Sign in with Google** ON.
3. Paste the **Client ID** and **Client Secret** from Google.
4. **Save.**

✅ **Google sign-in now works.**

---

## Notes & gotchas

- **Free Supabase email is rate-limited** (a handful of magic-link emails per hour).
  Fine for testing. For real users later, connect a custom email sender
  (Resend or Postmark) under Authentication → Emails → SMTP.
- **After you change the app**, re-export the HTML, rename it to `index.html`,
  and re-upload it to the repo (GitHub → your repo → `index.html` → the pencil/upload).
  Pages redeploys automatically in ~1 minute.
- **The URL must match exactly** in Supabase. If your repo is named `humanly`,
  the path is `/humanly/` — the trailing slash matters. If sign-in bounces you
  to a blank page, it's almost always a Redirect URL mismatch here.
- **Your Supabase keys** (project URL + publishable/anon key) are already baked
  into `index.html`. The anon key is safe to expose publicly — that's what it's for.
  Your data is protected by Row Level Security, not by hiding the key.

---

## Quick reference — your project

| Thing | Value |
|---|---|
| Supabase project | Humanly |
| Project ref | `qaminbhfgkkjlbwlatvw` |
| Supabase URL | `https://qaminbhfgkkjlbwlatvw.supabase.co` |
| Google callback (for step 3a) | `https://qaminbhfgkkjlbwlatvw.supabase.co/auth/v1/callback` |
| Your site URL (fill in after Part 1) | `https://__________.github.io/humanly/` |
