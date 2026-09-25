# eyecarecatoosa

A **total redesign** of **eyecarecatoosa.com** (Eyecare of Catoosa Hills — Catoosa, OK; Brett
Howell, O.D., Kyle E. Craig, O.D. and Phillip C. Ford, O.D.) — 156 pages — built by the
`site-reforge` pipeline and hardened for public hosting.

**Preview: https://sgencms.github.io/eyecarecatoosa/**

This is a pure static site with no build step, no dependencies and no backend. Serve the
folder, or use the preview link above.

> This is an **unofficial development copy** published for build review. It is not operated
> by, affiliated with, or endorsed by Eyecare of Catoosa Hills. The real site is
> https://www.eyecarecatoosa.com/.

## What this is, and what it is not

This is **not** a pixel-faithful clone. It is a commissioned redesign:

| | |
| --- | --- |
| **Content, imagery, branding, contact details, URLs** | All from `eyecarecatoosa.com`. No page copy was written: every sentence of body text comes from the practice's live site, much of it the eye-health library copy the practice republished. The redesign adds only interface text: menu-group labels, section labels such as "Related eye care" and "On this page", button labels such as "Get directions", and descriptive alt text for the site's photographs. **No image was generated**, apart from the favicon and touch icon, which the build cut from the logo's eye mark and set white on the logo's navy. Every other image was published on the practice's site: the practice's own photographs, plus stock and product photographs its site used from its web platform's shared clip-art library (people, eyewear and exam-instrument photos, and Transitions product shots). |
| **Navigation model and page anatomy** | Modelled on `eyetrendsclearlake.com`: a top bar, a Services mega-menu grouped into care clusters, an Eyewear mega-menu with four featured categories, and a persistent appointment call to action. **No content or asset came from that site.** |
| **Visual design** | New. It is the "Hillside" system, built on the practice's own navy `#112744` (measured from its logo) with a warm paper ground, a clay accent for actions, arch-framed photographs and contour lines taken from the name *Catoosa Hills*. |
| **Platform** | Removed: no WordPress, no EyeCarePro theme, no Beaver Builder, no Gravity Forms, no trackers. |

Because this is a redesign rather than a clone, matching the old design pixel for pixel is not
a goal.

## Verification

These figures were measured by the pipeline, not judged by eye, and filled in from the
measurement files. The first six rows come from the handoff build's audit. The last three were
re-read from this tree as published.

| Check | Result |
| --- | --- |
| Content recall vs the live source, every rebuilt page | **99.53% mean; 155 of 156 pages at 100%; the other one, an archive listing, is below the floor and explained in the handoff documentation; floor 95%** |
| Pages mapped | **156 / 163** at their original URLs; the 7 not rebuilt are explained below |
| Claims traced to the live site (`sr-fabrication`) | **SOURCED: 620 claims across 158 files, 0 untraced** |
| Platform decontamination | **CLEAN: 0 findings across 166 files** |
| Responsive + a11y sweep, 390 / 768 / 1024 / 1440 px | **0 blocker · 0 major** across 632 page × width sweeps. Also 715 minor, all “Text clipped by overflow:hidden”: 648 are screen-reader-only text, clipped by design; 67 are the phone layout's three-line link-card teasers (the full text stays in the page); and 21 nit, all “Heading level skipped”: each is an article heading the live page itself sets at h3 or deeper directly under its H1. |
| Gate (`sr-gate.mjs`) | 23 PASS · 6 FAIL · 0 UNPROVEN. The verdict is NOT-READY, and the handoff zip was packaged with a recorded override. Each red is explained below |
| Preview hardening, re-read from the shipped bytes | **158 / 158 pages** |
| Reference audit: every local `href` / `src` / `srcset` / `url()` resolved against its own page | **27,195 checked; 0 escape the site root, 0 missing, and 0 root-relative outside `404.html`, whose 157 references are absolute under `/eyecarecatoosa/` by design (GitHub Pages serves it at any depth)** |
| Rendering at this preview's subpath, every page at 1440 and 390 px | **314 page loads (157 pages × 1440 / 390 px): 0 requests ≥ 400, 0 broken images, 0 console errors, 0 requests outside `/eyecarecatoosa/`, 0 horizontal overflow at 390** |

**Why the red gate checks are red:**

- **C03, C04, C06 — defects of the live site.** Its pages link to 16 URLs that answer 404
  (none of them is in its sitemap), one of its pages (`/slideshow/1802-2/`) has no text, and two of
  its pages have no `<title>`. The
  rebuild does not invent content for them: missing pages stay missing, and the empty and untitled
  pages keep their URLs, get a title from their own heading (or the practice's name) and are marked
  `noindex`.
- **C16, C17 — 7 source URLs deliberately not rebuilt**, by change-control decision;
  the handoff documentation lists each one with its reason, and explains the one page whose
  content recall is below the floor.
- **C22, pixel parity.** This check measures fidelity to the design the brief asked to
  *replace*. The drift it reports is the deliverable.

## Hardening applied to this public copy

`PROVENANCE.json` counts every one of these changes.

1. **`noindex, nofollow, noarchive, nosnippet` on every page.** This is the control that
   actually keeps the pages out of search results. A `robots.txt` with `Disallow: /` is also
   shipped, but it has **no effect** here: crawlers read `robots.txt` only at the host root
   (`sgencms.github.io/robots.txt`, which this project site cannot provide), not under
   `/eyecarecatoosa/`. GitHub Pages cannot send an `X-Robots-Tag` header either. That leaves
   the non-HTML files with no index control of their own, such as the practice's fee-schedule
   PDF, images and `search-index.json`. On this site they are linked only from pages marked
   `nofollow`. The repository itself is public, and github.com shows its files like any public
   repository's.
2. `og:url` points at this preview and `og:description` / `twitter:description` carry the
   disclosure. `og:image`, `twitter:image` and the old theme's `og:featured_image` are removed.
   `noindex` does not stop link-unfurl crawlers, so without this a pasted link would render a card
   indistinguishable from the practice's own.
3. **JSON-LD removed.** It asserted the practice's identity, address, telephone and opening
   hours.
4. **All five practice forms made inert, with or without JavaScript** (appointment request,
   contact, patient registration, satisfaction survey, vendors).
   - Each form gets `action=""`, `onsubmit="return false"` and `data-preview="inert"`.
   - Each **submit button is replaced by a disabled `type="button"`**. With no submit control
     and several text fields there is no Enter-key submission either, so the forms cannot post
     even when scripting is off.
   - A visible notice at the top of each form card says so and gives the practice's phone number.

   The patient registration form collects health information, and on a public URL nobody should
   believe they submitted it. Site search still works.
5. `<meta name="referrer" content="no-referrer">`, so outbound clicks don't reveal this URL to
   third parties.
6. **No on-page disclosure banner**, by the org's standing rule since 2026-09-24. The disclosure
   is carried by `noindex`, by `og:description` / `twitter:description` (a pasted link unfurls as
   the disclosure), by the notice on each practice form, and by this README.
7. `sitemap.xml` and `llms.txt` are not shipped, because both advertise the practice's real
   URLs and invite crawlers. The Netlify-only `_headers` / `_redirects` are not shipped either,
   since GitHub Pages ignores them.
8. **The map embed uses no API key.** The live site embeds its map, on `/` and `/location/practice-location-1/`, with a
   Google Maps API key. The redesign also shows the map on `/hours-location/`, which on the live site has none. A key pushed to a public GitHub repository is flagged by secret
   scanning to Google and to its owner. All 3 map pages use Google's keyless embed of the
   same address instead, and a screenshot check confirms the pin lands in exactly the same place.
9. `<link rel="canonical">` is **kept** pointing at the practice's real page. That is correct for a
   duplicate, and deliberately different from `og:url`.

## Known limits

- **The five practice forms do not submit.** This is deliberate (see 4 above). In the handoff build
  they are complete but unwired, waiting for the practice to point them at a HIPAA-eligible
  endpoint.
- **3 pages load a third-party frame** when viewed: maps.google.com on `/hours-location/`, `/`, `/location/practice-location-1/`. Measured in a browser, with the frame's own requests included, viewing those pages contacts 7 Google hosts (`fonts.googleapis.com`, `fonts.gstatic.com`, `maps.google.com`, `maps.googleapis.com`, `maps.gstatic.com`, `places.googleapis.com` and `www.google.com`), including 21 POST requests to Google's map services over the 6 measured loads. The frame has `referrerpolicy="no-referrer"`, so it is sent no Referer, but a frame can still read the origin of the page that embeds it. No other page contacts any third party: fonts, images, scripts and styles are all self-hosted. Outbound links (the practice's Facebook page, Google Maps directions) are ordinary links, and `no-referrer` keeps this URL out of those requests.
- **33 pages link to the EyeGlass Guide** (`ecp.eyeglassguide.com`), as the live site does: 32 articles end with a link inviting the reader to visit the EyeGlass Guide, and `/eyeglasses-contacts/eyeglass-guide/` links to its "interactive on-line tool". That host did not answer when this preview was built (the connection timed out), so those links lead nowhere. They are kept as found, for the practice to decide.
- **12 images were refused (HTTP 403)**: 11 by the live site's image CDN (`da4e1j5r7gw87.cloudfront.net`) and 1 by the old web vendor's server (`www.eyecarepro.net`, a background image in its theme stylesheet), among them the current doctor portraits and the optical-showroom photographs. The pipeline does not retry past a refusal. Where the live site placed them, the rebuild shows other images the live site published (the older doctor portraits the CDN still serves among them), or none. The practice can supply the originals.
- **The live site contradicts itself in places, and has dead links.** The rebuild settles two
  things:
  - Every hours table (and the home page's hours line) shows the Monday–Thursday 12–1 lunch
    closure, which the live site states only on `/hours-location/`; its home page and location page
    list the same hours without it. The practice should confirm the closure.
  - The live pages link to 16 URLs that answer 404, eight of them the service links on
    `/eye-care-services/`. Each link now goes to the practice's own page on the same subject where
    one exists, and otherwise keeps its words without the link. The footer's "Sitemap" link is left
    out.

  The rest are shown where the live site shows them: four spellings of the practice's name and an
  unfilled template placeholder in the privacy notice. All of them are listed for the practice to
  settle in the handoff `CHANGE-LOG.md`.
- **Commit metadata is public**, including the committer email.

## Licence / ownership

This repository is an unaffiliated development artifact and asserts no rights over any of its
content.
- The practice's own writing, its own photographs and the Eyecare of Catoosa Hills name and logo
  belong to the practice.
- The practice's site also republished third-party material, and this copy carries it as found:
  the eye-health library articles and diagrams, eyewear and lens-treatment illustrations,
  frame-brand logo collages, stock photographs from its web platform's shared clip-art library, and
  Transitions product photographs. The owners' rights remain theirs; the trademarks among them (Transitions,
  Ray-Ban, Oakley, Gucci and others) belong to their owners.
- The typefaces are Young Serif and Figtree (both SIL Open Font License 1.1), self-hosted.
