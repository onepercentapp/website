# 1% — marketing website

Static HTML and CSS for the iOS app **1%** (bundle `com.onepercent.app`).

No build step. No framework. No npm. No JavaScript. No webfont, no CDN, no
iframe, no analytics, no cookies. Every byte is served from this folder.

```
index.html            hero, four sections, FAQ
privacy.html          privacy policy
terms.html            terms of use / EULA
support.html          contact, cancel, restore, delete, crisis line
delete-account.html   account deletion path (App Store guideline 5.1.1(v))
imprint.html          Impressum (§ 5 DDG)
styles.css            the only stylesheet
robots.txt
sitemap.xml
PLACEHOLDERS.md       every {{PLACEHOLDER}} and what belongs in it
README.md             this file
```

---

## Fill the placeholders first

Every unknown fact is a `{{PLACEHOLDER}}`. Find them:

```
grep -rn '{{' .
```

`PLACEHOLDERS.md` lists each one, what belongs there and which are optional.
The important ones are `{{SITE_URL}}`, `{{COMPANY_NAME}}`,
`{{COMPANY_ADDRESS}}`, `{{RESPONSIBLE_PERSON}}`, `{{SUPPORT_EMAIL}}`,
`{{PRIVACY_EMAIL}}`, `{{LAST_UPDATED}}` and `{{YEAR}}`.

A find-and-replace across all files is the right approach — the same value is
used in visible text, in meta tags and in `sitemap.xml`.

macOS / BSD `sed`, one placeholder at a time:

```
LC_ALL=C sed -i '' 's|{{SITE_URL}}|https://onepercent.app|g' *.html *.txt *.xml
LC_ALL=C sed -i '' 's|{{SUPPORT_EMAIL}}|hello@onepercent.app|g' *.html
```

GNU `sed` (Linux, CI) drops the `''`:

```
sed -i 's|{{SITE_URL}}|https://onepercent.app|g' *.html *.txt *.xml
```

Then confirm nothing is left:

```
grep -rn '{{' .        # only PLACEHOLDERS.md and README.md should match
```

---

## Look at it locally

Any static server works. Opening `index.html` straight from the filesystem
also works, since there is nothing to load.

```
python3 -m http.server 8000
```

Then `http://localhost:8000`.

Worth checking before you ship:

- 375px wide, which is the layout target
- dark mode — macOS System Settings, Appearance, or the browser's device
  emulation
- reduced motion on: macOS System Settings, Accessibility, Display, Reduce
  motion. The dot grid should be fully drawn immediately and nothing should
  move.
- tab through every page with the keyboard; the skip link should appear first
  and every link should show a focus ring

---

## Deploy

The folder **is** the site. Drop it anywhere that serves static files.

`README.md` and `PLACEHOLDERS.md` are for you, not for visitors. Neither is
linked from any page or listed in `sitemap.xml`, but both are readable if
someone guesses the URL — and `README.md` quotes the banned-word list below in
full, which is the only place those words exist in this folder. If you would
rather they were not reachable at all, leave the two `.md` files out of the
deploy.

### GitHub Pages

```
git init
git add .
git commit -m "1% marketing site"
git branch -M main
git remote add origin git@github.com:USER/REPO.git
git push -u origin main
```

Repository → Settings → Pages → Source: *Deploy from a branch* → `main` / `root`.
Live in a minute or two at `https://USER.github.io/REPO/`.

If it serves from a subpath rather than a domain root, set `{{SITE_URL}}` to
that full subpath — e.g. `https://user.github.io/repo`. The internal links are
all relative, so they work either way; only the canonical, Open Graph and
sitemap URLs care.

### Netlify

Drag the folder onto the Netlify dashboard. Or, connected to a repository:
build command empty, publish directory `.`.

### Cloudflare Pages, Vercel, S3, nginx

Same story: no build command, output directory is this folder.

---

## Things to keep true when editing

**Zero JavaScript.** The FAQ is `<details>`. The theme is
`prefers-color-scheme`. The dot animation is CSS. If a change seems to need a
script, it does not belong here.

**No external requests.** No webfont, no CDN, no hosted image, no embed. The
favicon is an inline SVG data URI. The App Store badge, when it exists, is
downloaded from Apple and served from this folder — never hotlinked. The only
outbound things on the whole site are ordinary links a person clicks: Apple's
EULA, Apple's refund page, and `findahelpline.com`.

**One `<h1>` per page.** Sections use `<h2>`.

**The dot grid is the only graphic**, and the only animation. 7 × 5, 35 dots,
34 filled and one open outline for today. Filled dots fade in 50ms apart via
`--i` on each element; `prefers-reduced-motion: reduce` renders the final state
with no animation at all.

**Design tokens live at the top of `styles.css`** and come from the app's
`tailwind.config.js` and `src/lib/theme-colors.ts`. One exception, marked in a
comment: light-mode `--ink-secondary` is `#6E6E73` here rather than the app's
`#8A8A8E`, because `#8A8A8E` on `#F5F5F4` measures 3.15:1 and fails WCAG AA for
body text. `#6E6E73` measures 4.65:1. Do not put secondary-coloured text on
`--surface-2` in light mode; that pairing is 4.37:1.

**Type sizes are the app's**, verbatim, except the hero display which scales
48–56px. Keeping the rest at 32 / 22 / 17 / 13 is what makes the site feel like
the app.

**The header and footer are copied into each page** by hand. There is no
templating. If you change one, change all six.

---

## Language rules

The App Store classifies apps by what they say they do, and wording that
implies a medical benefit can pull the app under EU MDR as a regulated device
as well as get the listing rejected. These words must not appear anywhere on
the site, including in alt text, meta descriptions and the FAQ:

> therapy, therapeutic, treatment, treat, cure, heal, relieve, alleviate,
> diagnose, diagnosis, symptom, disorder, anxiety, depression, depressed,
> mental illness, clinical, clinically proven, patient, disease, condition,
> trauma, PTSD, ADHD, OCD, evidence-based, "helps with", "works for", "proven
> to reduce"

Write "feel more at ease around people", not the medical phrasing.

Also: no exclamation marks, no motivational filler, no fake urgency, no user
or step counters, no invented reviews or ratings, and no claim of a result the
app cannot guarantee.

This disclaimer is in the footer of every page and once more in the privacy
policy — keep it there:

> 1% is a general self-improvement app and is not a substitute for
> professional advice or care.

The AI disclosure is not optional either. EU AI Act Article 50 requires it to
be obvious that the steps are machine-generated. It appears on the home page,
in the privacy policy, in the terms and in every footer.
