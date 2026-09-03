# certsage-legal

Public legal pages for the **CertSage** mobile app (`com.certsage.app`), served as a
static site from GitHub Pages.

| File | Published at | Purpose |
|---|---|---|
| `index.html` | `/` | Landing page linking both documents |
| `privacy.html` | `/privacy.html` | Privacy policy — the URL Play Console needs |
| `delete-account.html` | `/delete-account.html` | Data deletion request — also required by Play |
| `style.css` | — | Shared styles. No JS, no webfonts, no CDN. |
| `favicon.svg` | `/favicon.svg` | The CertSage mark, themed for light and dark tabs |

## Facts these pages assert

Both documents are filled in and ready to publish. The details they state:

- **Controller:** Van Manh Le, individual developer, Australia
- **Contact:** support.certsage@integolutions.com
- **Data residency:** Supabase `ap-southeast-1` (Singapore)
- **Sub-processors:** Supabase, Anthropic, OpenAI (TTS only), RevenueCat, Google Play

If any of these change, the policy has to change with them.

## Publish

Push to `master`, then enable Pages once:

    gh api -X POST repos/BryanLe2811/certsage-legal/pages \
      -f 'source[branch]=master' -f 'source[path]=/'

Or: repo Settings -> Pages -> Source: Deploy from a branch -> `master` / `/ (root)`.

First build takes 1-2 minutes. After that every push republishes automatically.

## Custom domain

Served at `legal.certsage.app`. A subdomain, deliberately -- the apex
`certsage.app` stays free for a marketing site later, and the Play Console URL
must never have to move once submitted.

DNS at the registrar:

    Type   Name    Value                    TTL
    CNAME  legal   bryanle2811.github.io.   3600

Then Settings -> Pages -> Custom domain -> `legal.certsage.app` -> Save, and tick
**Enforce HTTPS** once it becomes available.

Order matters: add the DNS record FIRST, wait for it to resolve, then set the
domain in GitHub. `.app` is on the HSTS preload list, so browsers refuse plain
HTTP outright -- until Let's Encrypt issues the certificate the site does not
load at all, and there is no click-through warning. Load the URL in a real
browser before pasting it into Play Console.

Saving a custom domain makes GitHub commit a `CNAME` file to this repo, so
`git pull` before your next local edit.

## Where these URLs go

1. **Play Console -> Store listing** -> Privacy policy URL
2. **Play Console -> App content -> Data safety** -> same privacy policy URL, and the
   deletion URL under "Provide a way for users to request account deletion"
3. **In the app** -> Settings screen, Privacy Policy row (needs `url_launcher`)

Use the same URL in all three. Changing it later means updating all three.

## Keeping it accurate

The policy describes what the app **actually does today**. It states explicitly that
CertSage collects no location, no advertising ID, no microphone or camera data, no
analytics, and no push tokens. Update this page *before* shipping any of those --
a Data safety declaration that disagrees with the policy is a common review rejection.
