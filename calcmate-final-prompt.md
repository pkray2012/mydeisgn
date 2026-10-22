# CalcMate: final prompt (decisions + indexing + SEO/GEO + finish + deploy zip)

Paste the whole box into the CalcMate session.

````text
OPEN ITEMS REVIEWED. MY DECISIONS:
- Coloured icon tiles (blog call-out, contact picker): YES, remove the
  tiles, keep the icons plain (brand blue/grey).
- Bank of India list symbol: keep as is for now.
- Titles over 60 chars: YES, shorten them.
- 404 page: YES, title "Page not found | CalcMate".
- HTML sitemap page: list indexable pages only.
- Remove the unused CENTRAL_RATES.rbiRepoRate = 6.50 constant.
Everything else in open-items.md: I'll send files/PDFs later; don't
wait for them.

Work through the steps below in order without stopping, unless something
needs a paid service, a secret, or a decision only I can make. Collect
any questions for me in a "Decisions for you" list at the end.

==========================================================================
STEP A: FINISH PR #3
==========================================================================
1. Apply the decisions above that belong in PR #3's code.
2. Logo optimisation: SVGO (safe preset, keep viewBox) on every logo
   SVG; 2× web copies of raster logos (small copies, ~48px tall, for the
   72×24 list box/menu/rankings; ~2× header size for bank pages);
   originals untouched in assets/logo-originals/. Report the logo folder
   size before/after, the bytes the Banks panel loads when opened, and a
   2× zoom crop to prove nothing got blurry.
3. Re-run lint, typecheck, unit tests, static build and browser tests.
4. I authorise you to MERGE PR #3 into main once everything passes
   (normal merge commit).

==========================================================================
STEP B: FINISH THE EDUCATION / BIKE BRANCH
==========================================================================
1. Merge main into claude/education-bike-loan-rates and resolve the
   loan-rates conflicts.
2. Apply the approved fixes: the Calculate column cut off at 1366; fee
   text per scheme, or shortened with the full text one tap away; Canara
   10.40% as the open rate plus a women's concession note; TMB labelled
   as a credit-score band.
3. Re-run every check, open the PR (tag table, verification table,
   changed lender pages, blocked lenders, logo status), and I authorise
   you to MERGE it into main once all checks pass.

==========================================================================
STEP C: FINAL AUDIT ON MAIN (subagents in parallel)
==========================================================================
Audit the merged main with parallel subagents: calculation accuracy,
data and sources, tax and rules, SEO, mobile/desktop and accessibility
(375 and 1366, axe), forms/privacy/security (CSP report-only, HSTS,
both Netlify forms in __forms.html), and performance. Verify every
blocker/high yourself; fix confirmed ones on a branch off main with a PR
you may merge after checks pass. Ask me before anything that changes a
feature or the design (the decisions above are already approved).

C1. GOOGLE INDEXING (Search Console shows only 14 valid breadcrumb
    items so far). Check that:
1. EVERY indexable page has a visible breadcrumb AND valid BreadcrumbList
   JSON-LD (Home > Category hub > Page), with absolute https://calcmate.in
   URLs that match the canonical and the visible breadcrumb exactly; no
   breadcrumb on the homepage; validate a sample with validator.schema.org.
2. Every page's canonical, og:url, sitemap URL and internal links use
   https://calcmate.in (no http, no www, no trailing slash, no .html).
3. The sitemap lists all indexable pages with real lastmod dates, and no
   noindex/redirected pages; robots.txt points to it.
4. Every indexable page is linked from at least one hub/menu/footer link
   (no orphan pages), and important pages are max 2 clicks from home.
Report counts: indexable pages, pages with BreadcrumbList, orphans.

C2. SEO GUIDELINES (fix gaps; no design changes; keep the phone
    homepage under the 9,000px budget):
1. One H1, a title of 50–60 chars with the main keyword first, a meta
   description of 140–160 chars, a self-referencing https canonical,
   lang="en-IN".
2. The right schema per page type: WebApplication/SoftwareApplication
   for calculators; FAQPage only where the FAQ is visible on the page;
   ItemList for rankings; Article (author, datePublished, dateModified,
   image) for guides; Organization (logo, sameAs) site-wide. All valid,
   no fake ratings or reviews.
3. Internal linking: every calculator links to its hub, 3–6 related
   calculators and 1–3 guides; hubs link to all their calculators;
   descriptive anchor text.
4. Core Web Vitals on mobile: LCP < 2.5s, CLS < 0.1, INP < 200ms for the
   homepage, one calculator, /banks and one ranking page (report the
   Lighthouse numbers).
5. Images: width/height set, alt text on meaningful images, lazy-loaded
   below the fold.

C3. GEO GUIDELINES (AI search: ChatGPT, Perplexity, Google AI Overviews,
    Copilot):
6. Answer first: every calculator and ranking page opens with a 1–2
   sentence direct answer with the key number (the existing answer box),
   plus a short "Key takeaways" list (3–4 bullets) where it helps. Plain
   words, numbers with units and dates (e.g. "as of 21 Oct 2026").
7. Quotable facts: rates, limits and rules in real HTML tables/lists
   (not images, not only in JavaScript), each with its source and
   "last verified" date next to it.
8. Trust (E-E-A-T): an About page saying who runs CalcMate and why; the
   methodology page; "Last updated" and "Sources" on every calculator
   and rate page; an author/reviewer line on guides. Ask me for names
   and credentials; don't invent any.
9. Consistent names: the official bank/scheme names the first time
   (e.g. "Public Provident Fund (PPF)", "Sukanya Samriddhi Yojana (SSY)"),
   then the short form.
10. FAQs written as the real questions people ask, each answered in 2–3
    sentences that stand alone.
11. robots.txt: allow Googlebot, Bingbot and the AI search crawlers
    (OAI-SearchBot, ChatGPT-User, PerplexityBot, ClaudeBot,
    Google-Extended, Applebot-Extended). List them for me so I can decide
    about training-only bots like GPTBot.
12. Add /llms.txt: a short plain-text guide to the site (what CalcMate
    is, main sections with links, how rates are verified, contact).
13. Bing: add IndexNow support (key file + a post-deploy ping of changed
    URLs) if it fits the static Netlify setup; tell me if it needs a
    secret.
Report a table: guideline | pages passing | pages fixed | left for me.

C4. BRAND SEARCH: someone searching "CalcMate" or "calcmate.in" on
    Google must find calcmate.in (other things are called CalcMate, e.g.
    an iOS notes-calculator app and calcmate.org, so make our brand
    signals strong and consistent):
1. Homepage: title starts with the brand, e.g. "CalcMate: Free
   Financial Calculators for India" (≤60 chars); H1 or first line names
   "CalcMate"; meta description mentions "CalcMate".
2. WebSite structured data on the homepage: name "CalcMate",
   alternateName ["CalcMate.in", "Calc Mate"], url https://calcmate.in/
   (this is what Google uses for the site name in results), plus a
   SearchAction for the site search only if /search?q= really works.
3. Organization structured data (site-wide, one @id): name "CalcMate",
   alternateName "CalcMate.in", url, logo (square PNG ≥112×112, the
   official logo), sameAs = our real social profiles (Facebook; ask me
   for any others: X/Twitter, LinkedIn, Instagram, YouTube). Never list
   profiles that don't exist.
4. Every page title ends with " | CalcMate" (or the brand appears
   consistently); og:site_name "CalcMate"; the same logo everywhere.
5. Favicon: a square icon at least 48×48 (multiples of 48: 48, 96,
   144) linked in the head, so Google can show it in results; the
   favicon file must be crawlable (not blocked by robots.txt).
6. About page: says clearly "CalcMate (calcmate.in) is ...", who runs
   it, and links to the social profiles; link it from the footer.
7. Report what you changed, and list the off-site steps for me (below).

End STEP C with a READY TO DEPLOY / NOT READY verdict.

==========================================================================
STEP D: NETLIFY DEPLOY FILE (only if READY)
==========================================================================
1. From the final main: npm run build:static. Check that out/ contains
   index.html, _redirects (removed-page 301s ABOVE the /* 404 rule),
   _headers, __forms.html, 404.html, robots.txt, sitemap.xml and
   llms.txt, and that every page is built for https://calcmate.in.
2. Zip the CONTENTS of out/ (index.html at the top level of the zip) as
   calcmate-netlify-drop.zip, and send it to me.
3. Give me the exact deploy steps for my EXISTING site (Netlify -> my
   site -> Deploys -> drag-and-drop box at the bottom; NOT
   app.netlify.com/drop), the post-deploy checklist (both forms;
   curl -I on a removed bank page and /about.html = 301;
   /no-such-page = 404; security headers; a few calculators; the menu and
   Banks panel on phone and desktop) and how to roll back.
4. Tell me whether this Netlify site is linked to GitHub. If it is, say
   so: then I should NOT drag-and-drop, because the merge to main
   deploys it (I'll lock auto-publishing first if I want to check before
   it goes live).
5. Give me the Search Console + Bing steps for after the deploy:
   resubmit the sitemap; the top 15 URLs to "Request indexing" (spread
   over a few days); import into Bing Webmaster Tools.

Final report: what changed in each step, the indexing counts, the
SEO/GEO table, the verdict, the zip, and the "Decisions for you" list.
````
