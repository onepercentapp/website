# Placeholders

Every fact the site does not know yet is written as `{{NAME}}`. Search the
folder for `{{` and you will find all of them.

```
grep -rn '{{' .
```

Replace each one everywhere it appears — the same value is used in the visible
text, in the `<meta>` tags and in `sitemap.xml`, so a find-and-replace across
all files is the right move. Nothing is generated; these are literal strings in
literal files.

---

## Required before launch

| Placeholder | What goes there |
| --- | --- |
| `{{SITE_URL}}` | Public origin of this site, no trailing slash, e.g. `https://onepercent.app`. Used in `<link rel="canonical">`, all Open Graph and Twitter URLs, `sitemap.xml` and `robots.txt`. Must be the exact scheme and host you actually serve on, otherwise canonical tags point at the wrong place. |
| `{{COMPANY_NAME}}` | Legal name of the operator, exactly as registered — e.g. `Jane Doe` for a sole trader, or `Onepercent GmbH`. Appears in the imprint, the privacy controller block, the terms and every footer copyright line. |
| `{{COMPANY_ADDRESS}}` | Full postal address, street and number, postcode and town, one line or with `<br>`. A P.O. box is not enough under § 5 DDG. The word `Germany` is already printed on the next line. |
| `{{RESPONSIBLE_PERSON}}` | Natural person who represents the operator and is responsible for the content under § 18 (2) MStV. First and last name. |
| `{{SUPPORT_EMAIL}}` | Address a person actually reads, e.g. `hello@onepercent.app`. This is the address Apple checks behind the support URL. |
| `{{PRIVACY_EMAIL}}` | Address for GDPR requests. May be the same as `{{SUPPORT_EMAIL}}` — if so, put the same value in both. |
| `{{SUPPORT_RESPONSE_TIME}}` | How fast you actually answer, written as a phrase that follows "We reply within" — e.g. `two business days`. Promise something you can keep; it is quoted on three pages. |
| `{{LAST_UPDATED}}` | Date in ISO form, `YYYY-MM-DD`. Shown on every legal page and used as `<lastmod>` in `sitemap.xml`, which requires exactly that format. Update it whenever you change a legal page. |
| `{{YEAR}}` | Four-digit year for the footer copyright line, e.g. `2026`. |
| `{{SUPERVISORY_AUTHORITY}}` | The data protection supervisory authority for your German state, written out — e.g. `the Bayerisches Landesamt für Datenschutzaufsicht`. The sentence around it reads "Ours is …", so include the article. |

## Required once the app is on the App Store

| Placeholder | What goes there |
| --- | --- |
| `{{APP_STORE_URL}}` | Full App Store product URL, e.g. `https://apps.apple.com/app/id0000000000`. Only referenced inside the commented-out badge block in `index.html` right now — see "App Store badge" below. |
| `{{PRICE_MONTHLY}}` | Monthly price with currency as shown in your home storefront, e.g. `€4.99`. |
| `{{PRICE_YEARLY}}` | Yearly price with currency, e.g. `€39.99`. |
| `{{TRIAL_LENGTH}}` | Free trial length as a phrase, e.g. `7-day` — the sentence reads "after a {{TRIAL_LENGTH}} free trial". If there is no trial, delete the trial sentences in `index.html` and `terms.html` rather than leaving a value here. |

## Optional

| Placeholder | What goes there |
| --- | --- |
| `{{OG_IMAGE}}` | Absolute URL to a 1200 × 630 PNG or JPG for link previews, served **from this site's own origin**, e.g. `https://onepercent.app/og.png`. Crawlers fetch it; the page itself never does, so this does not break the no-external-requests rule. If you have no image, delete the four `og:image` / `twitter:image` lines from each page and change `twitter:card` to `summary`. |
| `{{PHONE}}` | Phone number in the imprint. German case law wants a second fast contact route beside email; a phone number is the usual one. If you would rather not publish a number, replace the whole `Phone` row with a contact form route and say so, or delete the row and accept the risk. |
| `{{VAT_ID}}` | VAT identification number under § 27a UStG, e.g. `DE123456789`. If none has been issued, delete the whole "VAT identification number" section from `imprint.html`. |
| `{{REGISTER_COURT}}` | Register court, e.g. `Amtsgericht München`. Delete the "Register" section if the operator is not entered in a commercial register. |
| `{{REGISTER_NUMBER}}` | Register number, e.g. `HRB 123456`. Same as above. |
| `{{SCREENSHOT_1}}` `{{SCREENSHOT_2}}` `{{SCREENSHOT_3}}` | Paths to three same-origin app screenshots, e.g. `shots/step.png`. The markup that uses them sits inside an HTML comment in `index.html` and is switched off on purpose — the dot grid is meant to be the only graphic. Uncomment it only if you decide otherwise, and write real alt text while you are in there. |

---

## App Store badge

`index.html` currently renders a plain "Coming to the App Store" pill instead of
a badge, so there is no dead link while the app is unreleased.

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

## After replacing

Check nothing was missed:

```
grep -rn '{{' .        # should print only this file and README.md
```
