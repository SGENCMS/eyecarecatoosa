# PROVENANCE

This tree is a **redesign** of `https://www.eyecarecatoosa.com/`, which was then modified for
public hosting. Because it was modified, **this tree is not the handoff deliverable** and must not
be cited as one. Machine-readable counts are in `PROVENANCE.json`. Every deviation is listed below.

## Capture

| | |
| --- | --- |
| Content source | `https://www.eyecarecatoosa.com/`: 163 pages, found through the sitemap and a same-origin BFS crawl. robots.txt was honoured, no host refused a page, and the crawl was not truncated. 16 further URLs, linked from its pages (none of them in its sitemap), answer 404 on the live site. |
| Structure source | `https://eyetrendsclearlake.com/`: **navigation model and page anatomy only**, crawled in full (43 pages). No content, image, asset or word from that site appears here. |
| Captured | 2026-09-25 |
| Method | The `site-reforge` pipeline (crawl → extract → assets → capture → tokens → motion → plan → build → SEO → rebase), plus a zero-dependency Chrome DevTools Protocol bridge for the browser stages |
| Platform of origin | WordPress with the EyeCarePro theme, Beaver Builder and Gravity Forms. All of it has been removed. |
| Design baseline | Computed style measured in a real browser at 390 / 768 / 1024 / 1440 px, not read from source CSS |

## What the redesign changed, and what it preserved

| | |
| --- | --- |
| **Preserved** | Every rebuilt page's text (99.53% mean; 155 of 156 pages at 100%; the other one, an archive listing, is below the floor and explained in the handoff documentation; floor 95%). URLs are unchanged. Titles, meta descriptions and canonicals are unchanged too, with 2 untitled source pages given a title (`/category/our-doctors/` and `/slideshow/1802-2/`); one archive retitled “What’s New”, its posts’ own label for it, in place of WordPress’s default “Uncategorized”; 1 empty meta description filled (`/`); and 3 missing canonicals filled with the page's own address. So are all five forms (every field, label and option) and the contact details. The hours are unchanged, except that every hours table (and the home page's hours line) shows the Monday–Thursday lunch closure, which the live site states only on `/hours-location/` (see the README's *Known limits*). |
| **Written** | No page copy. All body text comes from the live site. The only new text is interface text: menu-group labels, section labels ("Related eye care", "On this page", "In this section", "Page tools"), button labels ("Get directions") and descriptive alt text for the site's photographs. Any fact a label touches is from the live site. |
| **Added** | 11 sections and pages, by change-control decision: Site search page; Host 404 page; home: eye care services grid; home: optical band; home: insurance band; home: testimonial; Doctor band; Related care cards; Request-an-appointment band before the footer; Page tools panel; Get directions link. The redesign also adds the live site's map to `/hours-location/` (recorded as an IMPROVE of that page's visit section). |
| **Removed** | The 507 change-control rows are 420 PRESERVE, 62 IMPROVE, 14 REMOVE, 11 ADD. Every REMOVE row belongs to the 7 source URLs that are not rebuilt (each is listed with its reason in the handoff documentation); no other section was removed or replaced. Platform plumbing is gone: the EyeCarePro / WordPress runtimes, the voice-search widget, the seasonal snow effect, the "Powered by eyecarepro" credit and the WordPress login link. |
| **Generated** | Nothing but the favicon and touch icon, which the build cut from the logo's eye mark and set white on the logo's navy. Every other image was published on the practice's site. |

## Deviations applied for public hosting

The "Rendered?" column says whether the change can affect what is painted on the page.

| # | Change | Pages | Rendered? |
| --- | --- | --- | --- |
| 1 | `robots` set to `noindex, nofollow, noarchive, nosnippet`. **This is the effective index control**; see #9. | 158 | No |
| 2 | `<meta name="referrer" content="no-referrer">` inserted | 158 | No |
| 3 | `<title>` **not** prefixed, by the org's standing rule. `og:description`, `noindex` and the form notices carry the disclosure instead. | — | — |
| 4 | `og:url` repointed at this preview; `og:description` and `twitter:description` replaced with the disclosure. `og:image` / `twitter:image` removed, and so is the old theme's non-standard `og:featured_image` (3 pages), so no image-bearing meta remains. | 158 | No |
| 5 | `schema.org` JSON-LD removed (470 blocks). It asserted the practice's identity, address, telephone and opening hours. | 158 | No |
| 6 | Every `<form>` marked `data-preview="inert"` (301 forms, most of them the site-search boxes, which still work). The five practice forms also get `action=""` and `onsubmit="return false"`, and each **submit button becomes a disabled `type="button"`**, so they cannot post with JavaScript off either. | 301 forms | **Yes** (button shown disabled) |
| 7 | A visible notice at the top of each practice form's card — "This form is disabled in this preview", then the practice's phone number from the build's sourced config — in place of the handoff build's "Online submission is not available yet" line | 5 | **Yes** |
| 8 | **No rendered disclosure banner** (the org's standing rule since 2026-09-24). The verifier fails any page that carries a banner element or its wording. The preview stylesheet, `styles/preview.css`, is linked on every page for the form notices and disabled buttons (#6, #7). | 158 | No |
| 9 | `robots.txt` replaced with `Disallow: /`. **It has no effect here**, because crawlers read only `https://sgencms.github.io/robots.txt` (the host root) and never a project subpath. GitHub Pages cannot send `X-Robots-Tag`, so non-HTML files (PDF, images, JSON, the markdown) have no index control. The file is kept only in case this tree is ever served from a domain root. | — | No |
| 10 | `sitemap.xml` and `llms.txt` not shipped, because both advertise the practice's real URLs. `_headers` and `_redirects` are Netlify-only and GitHub Pages ignores them. | — | No |
| 11 | `404.html` references made absolute under `/eyecarecatoosa/`. GitHub Pages answers a missing path at any depth with it, so it is the one page that cannot use relative references. | 1 | Yes |
| 12 | `.nojekyll` added | — | No |
| 13 | The map embed on `/hours-location/`, `/` and `/location/practice-location-1/` switched from the Maps Embed API (which carries a Google API key) to Google's keyless embed of the same sourced address, with `referrerpolicy="no-referrer"`. See *The map key*. | 3 | **Yes** |
| 14 | Line endings normalised to LF (158 pages). Every CR sat inside the inline path-resolution script the `sr-rebase` stage adds; no page text had any. The repository's `.gitattributes` would make git do this on commit anyway, and an HTML parser treats both forms the same. Doing it here means the bytes verified are the bytes published. | 158 | No |

### The map key

The live site embeds its map as `maps/embed/v1/place?key=AIza…&q=place_id:…`. The key is public on
the live site, and Maps Embed keys are designed to be shipped to browsers. It is still left out of
this repository: a Google API key pushed to a public GitHub repository is picked up by GitHub secret
scanning and reported to Google, which notifies the key's owner, and publishing this preview should
not raise a credential-leak alert against a third party.

The keyless embed shows "Eyecare of Catoosa Hills, 650 S. Cherokee St., Suite A, Catoosa, OK 74015". Screenshotted next to the keyed embed, both drop the pin
labelled *Eyecare of Catoosa* at exactly the same spot. Every part of that address string comes from
the sourced `PRACTICE` config. The handoff build keeps the live site's keyed embed URL on every map page, including any the redesign added.

## Deliberately NOT changed

| | Why |
| --- | --- |
| `<link rel="canonical">` → the practice's own URL, on every page | Correct for a duplicate, and deliberately different from `og:url`, which drives unfurl cards |
| The live site's other internal contradictions (the name's four spellings, a placeholder in the privacy notice) | These are the practice's own words, so choosing between them is the practice's call. All of them, and the lunch closure the rebuild does show everywhere (see *Preserved*), are listed in the handoff `CHANGE-LOG.md`. |
| `site.css`, `tokens.css`, `motion.css`, `scripts/site.js` and every other non-HTML file except the named preview additions | Byte-identical to the handoff build; `preview-verify.mjs` compares them. The notice and disabled-button styles live only in `preview.css`. |

## Not published here

These are **not** in this repository:

- The `audit/` tree: the raw capture of the practice's site, computed-style captures,
  screenshots and reports. It is bulky, it carries absolute build paths from the capture machine,
  and a preview has no use for a full raw copy of the practice's site.
- `src/`, the generator and tools, and `docs/`, the handoff documentation.
- `assets/source/`, the original downloads.

All three belong to the handoff package.

## Verification after modification

Every figure below was re-read by `src/tools/preview-verify.mjs` (in the handoff build) from this
tree and from a browser rendering of it. None was taken from the tool that wrote them.

- **Hardening**: 158/158 pages carry exactly one robots meta reading `noindex, nofollow, noarchive, nosnippet`. On every page:
  - the referrer is set;
  - there is no JSON-LD;
  - there is no disclosure banner (its element and its wording are both absent), and the preview stylesheet is linked;
  - the canonical is still the practice's own;
  - `og:url` is this preview and `og:description` is the disclosure;
  - there is no image-bearing meta (`og:image`, `twitter:image`, `og:featured_image`, `image_src`);
  - there are no CR bytes;
  - there is exactly one `<h1>`.

  Other hardening results:
  - 3 iframes carry the permitted referrer policy.
  - 301/301 forms are inert.
  - All 5 practice forms have `action=""`, `onsubmit="return false"`, an empty `data-endpoint` and **no submit control** (5/5), each with its notice (5), and none keeps the handoff build's offline notice ("Online submission is not available yet", `data-form-offline`).
  - No title carries a prefix, and none of the 316 `og:title` / `twitter:title` values is empty.
- **Pages not rebuilt**: every shipped text file was searched for the addresses and titles of the 7 source URLs that were not rebuilt (13 markers). There were 0 hits.
- **Byte identity**: 75 non-HTML files are byte-identical to the handoff build. The only others are the named preview additions: `.gitattributes`, `.gitignore`, `.nojekyll`, `PROVENANCE.json`, `PROVENANCE.md`, `README.md`, `robots.txt`, `styles/preview.css`.
- **Secrets**: 171 text files, this one included, were scanned for credential-shaped strings (Google API keys, GitHub, OpenAI, Slack and AWS tokens, private keys) and for build-machine paths. There were 0 hits.
- **Reference audit**: 27,195 local references (22,532 href, 1,454 src, 296 action, 2,910 srcset, 3 css url()) were resolved against the file that carries each one.
  - 0 escape the site root, 0 point at a missing file, and 0 are root-relative.
  - The exception is `404.html`, whose 157 references are all absolute under `/eyecarecatoosa/` by design.
- **Rendering at the preview's subpath** (the live GitHub Pages site):
  - Every page was loaded in headless Chrome at 1440 and 390 px (314 loads), with lazy images forced to load.
  - The result was 0 responses ≥ 400, 0 broken images, 0 console errors, 0 requests outside the prefix, 0 horizontal overflow at 390, and no banner on any load (each page rendered at least 200 characters of text for that absence to be read from).
  - Third parties: every child target (the map frames included) was attached and its network watched. Off-site requests came only from inside the map frame on the 3 map pages, to `fonts.googleapis.com`, `fonts.gstatic.com`, `maps.google.com`, `maps.googleapis.com`, `maps.gstatic.com`, `places.googleapis.com` and `www.google.com` (21 of them POST). The page itself contacted no third party, and no other page contacted any.
  - Chrome cancelled no request.
- **404 at depth**: `no-such-page/`, `a/b/c/d/no-such-page`, `eye-care-services/nope/` each returned 404 and rendered the styled page with no banner, 0 failed subresources and 0 broken images.
- **Search**: `/search/?q=dry eye` returned 10 results at the subpath, all inside `/eyecarecatoosa/`. The first one opens (HTTP 200).
- **Practice forms, JavaScript on**: on the appointment and registration forms a submit was cancelled, the page did not navigate, 0 requests were sent, and the notice was present.
- **Practice forms, JavaScript off**: on the same two forms, Enter in a text field and a click that landed on the button (hit-tested) produced 0 and 0 non-GET requests, and the page stayed put.
  - Positive control: the same button was re-armed through the DevTools protocol, with no page script, and clicked again. Each form's click was **caught** submitting (POST /eyecarecatoosa/contact-us/appointment-request-form/; POST /eyecarecatoosa/contact-us/patient-registration-form/), which proves the test can see a submission.
  - Every non-GET request was failed locally, so none left the machine. The other three forms carry the same markup, checked byte by byte above.
- **Nothing covers a control**: hit-tested at each control's centre, the focused skip link is topmost at 390 px and 1440 px, and the open mobile drawer's close button is topmost at 390 px.
- **Tree**: `sitemap.xml`, `llms.txt`, `_headers`, `_redirects`, `audit/` and `src/` are absent. `.nojekyll` is present. `robots.txt` reads `Disallow: /`, which has no effect at this subpath (deviation #9).
- **After this file was written**, `preview-docs.mjs` re-ran the static checks (tree, byte identity, secrets, hardening, references) over the finished tree, including README.md, PROVENANCE.md and PROVENANCE.json. It would have refused to finish unless they passed.
