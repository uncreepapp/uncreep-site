# Uncreep — static site

Pure static HTML/CSS/JS. No build step, no dependencies.

```
index.html    Landing page (waitlist form + founding-member CTA)
app.html      Live demo app (scope-creep checker)
vercel.json   Vercel config (clean URLs: /app works as /app.html)
```

## 1. Replace the three placeholders

Find-and-replace these tokens across `index.html` and `app.html` before deploying:

| Token | Replace with | Where to get it |
|---|---|---|
| `FORMSPREE_ID` | Your Formspree form ID (e.g. `xabcdefg`) | formspree.io → New form → copy the ID from the endpoint URL `https://formspree.io/f/<ID>` |
| `STRIPE_PAYMENT_LINK` | Your Stripe Payment Link URL (e.g. `https://buy.stripe.com/...`) | Stripe Dashboard → Payment Links → New → one-time $29 product |
| `DOMAIN_PLACEHOLDER` | Your production domain (e.g. `uncreep.app`) | Used in the canonical + og:url tags in `index.html` |

Quick one-liner (macOS, run inside this folder):

```sh
sed -i '' -e 's/FORMSPREE_ID/xabcdefg/g' \
          -e 's|STRIPE_PAYMENT_LINK|https://buy.stripe.com/your_link|g' \
          -e 's/DOMAIN_PLACEHOLDER/uncreep.app/g' index.html app.html
```

## 2. Deploy to Vercel

**Option A — dashboard drag-and-drop (no CLI):**
1. Go to [vercel.com/new](https://vercel.com/new) and sign in.
2. Drag this whole `uncreep-site` folder onto the upload area (or "Deploy" → "Browse" and select it).
3. Framework preset: **Other**. No build command, no output directory. Deploy.

**Option B — CLI:**
```sh
cd uncreep-site
npx vercel          # first deploy (answers: no build settings needed)
npx vercel --prod   # promote to production
```

## 3. Add a custom domain

1. Vercel Dashboard → your project → **Settings → Domains**.
2. Enter your domain (e.g. `uncreep.app`) → **Add**.
3. Follow the DNS instructions Vercel shows: either point nameservers to Vercel, or add an `A` record (`76.76.21.21`) for the apex and a `CNAME` (`cname.vercel-dns.com`) for `www`.
4. HTTPS certificates are issued automatically once DNS propagates.

## Updating the site

Edit the HTML files and redeploy (drag-drop again, or `npx vercel --prod`). There is no state on the server — everything is static.
