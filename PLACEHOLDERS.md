# Placeholders

All of them are filled in.

```
grep -rn '{{' .
```

---

## Filled in

| Value | Used |
| --- | --- |
| Site URL | `https://onepercentapp.xyz` |
| Operator and responsible person | Luis Krüsselmann |
| Postal address | `c/o Impressumservice Dein-Impressum, Stettiner Str. 41, 35410 Hungen` — a service address. `Germany` is printed on the line below it in the address blocks, so it is not part of the value. Used in `/imprint` ×2, `/privacy` and `/terms`. |
| Imprint email | `info@onepercentapp.xyz` — **a second mailbox that has to exist and be read.** It is the § 5 DDG contact and is not the same address as support. |
| Phone | `0157 9234 1658`, operated by the imprint service. Linked as `tel:+4915792341658` so it dials from a phone. |
| Support and privacy email | `support@onepercentapp.xyz` — **this mailbox has to exist and be read.** Apple checks the address behind the support URL, and the privacy policy names it as the route for GDPR requests. |
| Response time | two business days |
| Last updated | 2026-07-28 |
| Copyright year | 2026 |
| Price | €4.99 per month, €29.99 per year |
| Trial | 3 days |
| Screenshots | `screenshot-1.png`, `screenshot-2.png`, `screenshot-3.png`, live on the front page |

## Removed rather than filled

The `{{PHONE}}` row was on this list while there was no number to publish. There
is one now, so the imprint has a phone row again — German case law wants a second
fast route beside email, and that risk is no longer being carried.

- **`{{OG_IMAGE}}`** — there is no `og.png` in this folder, and pointing link
  previews at a file that 404s is worse than having no preview image. The
  `og:image` and `twitter:image` tags are gone and `twitter:card` is now
  `summary`. To bring them back: drop a 1200 × 630 PNG in this folder, add the
  four tags to each page, and set the card back to `summary_large_image`.
- **`{{VAT_ID}}`, `{{REGISTER_COURT}}`, `{{REGISTER_NUMBER}}`** — the whole
  "Register" and "VAT identification number" sections are gone. A natural
  person is not entered in a commercial register, and § 27a UStG only requires
  a VAT ID to be shown if one has been issued. **If you do have a VAT ID, it
  must go back in** — that one is not optional.
- **`{{SUPERVISORY_AUTHORITY}}`** — the sentence in `/privacy` was
  rewritten so it does not name one. It points at the reader's own authority
  under Article 77 GDPR instead. Naming the authority for your state is still
  better practice; say which state and it can be put back.

---

## App Store badge

Done. `/` shows Apple's official black badge, linking to
`https://apps.apple.com/app/id6795155258`.

`6795155258` is the app's Apple ID from App Store Connect → App Information. It
is not the bundle identifier (`com.onepercentapp.ios`), and the link needs the
number. No country code in the URL: `apps.apple.com` routes each visitor to
their own storefront on its own.

Two rules that came with the asset and still apply if it is ever touched:
`app-store-badge.svg` is served from this folder and Apple's hosted copy is
never hotlinked — it would be the only external request this site makes — and
the badge may not be recoloured, which is why `.badge-link` changes nothing
about it but its opacity.

The link resolves only once the app is public. Before that it opens the App
Store on a page that does not exist yet, which is expected and needs no change
on release day.

---

## Keeping the legal pages true

The privacy policy, the terms and the deletion page describe what the app
actually does. If any of these change in the app, they change here too, and
`Last updated` moves with them:

- what the optional account copies to the backend,
- which processors are involved (Supabase, OpenAI, Resend),
- the sign-in methods on offer (Sign in with Apple, email and password),
- the price and the trial length.
