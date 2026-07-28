# Placeholders

Almost all of them are filled in. Two are left, and one of those is a real
blocker.

```
grep -rn '{{' .
```

---

## Still open

| Placeholder | Where | What goes there |
| --- | --- | --- |
| `{{COMPANY_ADDRESS}}` | `imprint.html` ×2, `privacy.html`, `terms.html` | **Blocker.** Full postal address — street and number, postcode, town. The word `Germany` is already printed on the line below it, so leave that out. § 5 DDG requires a real address; a P.O. box is not enough. Until this is replaced the pages render the literal `{{COMPANY_ADDRESS}}`, which reads as a broken site. |
| `{{APP_STORE_URL}}` | `index.html` | Not a blocker — it only appears inside an HTML comment. See "App Store badge" below. |

## Filled in

| Value | Used |
| --- | --- |
| Site URL | `https://onepercentapp.xyz` |
| Operator and responsible person | Luis Krüsselmann |
| Support and privacy email | `support@onepercentapp.xyz` — **this mailbox has to exist and be read.** Apple checks the address behind the support URL, and the privacy policy names it as the route for GDPR requests. |
| Response time | two business days |
| Last updated | 2026-07-28 |
| Copyright year | 2026 |
| Price | €4.99 per month, €29.99 per year |
| Trial | 3 days |
| Screenshots | `screenshot-1.png`, `screenshot-2.png`, `screenshot-3.png`, live on the front page |

## Removed rather than filled

- **`{{OG_IMAGE}}`** — there is no `og.png` in this folder, and pointing link
  previews at a file that 404s is worse than having no preview image. The
  `og:image` and `twitter:image` tags are gone and `twitter:card` is now
  `summary`. To bring them back: drop a 1200 × 630 PNG in this folder, add the
  four tags to each page, and set the card back to `summary_large_image`.
- **`{{PHONE}}`** — the imprint no longer has a phone row. German case law
  wants a second fast contact route beside email, and a phone number is the
  usual one; email alone is the common practice for a solo developer and the
  risk that comes with it is accepted here. Add the row back if you would
  rather publish a number.
- **`{{VAT_ID}}`, `{{REGISTER_COURT}}`, `{{REGISTER_NUMBER}}`** — the whole
  "Register" and "VAT identification number" sections are gone. A natural
  person is not entered in a commercial register, and § 27a UStG only requires
  a VAT ID to be shown if one has been issued. **If you do have a VAT ID, it
  must go back in** — that one is not optional.
- **`{{SUPERVISORY_AUTHORITY}}`** — the sentence in `privacy.html` was
  rewritten so it does not name one. It points at the reader's own authority
  under Article 77 GDPR instead. Naming the authority for your state is still
  better practice; say which state and it can be put back.

---

## App Store badge

`index.html` renders a plain "Coming to the App Store" pill instead of a badge,
so there is no dead link while the app is unreleased.

When the app is live:

1. Download the official black "Download on the App Store" badge from Apple's
   Marketing Resources and Identity Guidelines.
2. Save it into this folder as `app-store-badge.svg`. Do not hotlink Apple's
   copy — the site makes no external requests, and that would be the only one.
3. In `index.html`, swap the `<span class="pill">…</span>` for the anchor in the
   comment directly above it, and fill in `{{APP_STORE_URL}}`.

The pill is sized to 156 × 52 CSS px, which is the standard render size of
Apple's 119.664 × 40 pt master asset, so the swap causes no layout shift.

---

## Keeping the legal pages true

The privacy policy, the terms and the deletion page describe what the app
actually does. If any of these change in the app, they change here too, and
`Last updated` moves with them:

- what the optional account copies to the backend,
- which processors are involved (Supabase, Google Gemini, Resend),
- the sign-in methods on offer (Sign in with Apple, email and password),
- the price and the trial length.
