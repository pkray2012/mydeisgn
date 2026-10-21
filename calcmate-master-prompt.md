# CalcMate: master prompt (all pending corrections, phase by phase)

Paste the whole box into the CalcMate session. It replaces the separate "homepage declutter", "menu" and "navigation + bank logos" prompts, so send only this one.

---

````text
MASTER PLAN: work through these phases IN ORDER, one at a time.

How to work (applies to every phase):
- Start from the latest main on a NEW branch (e.g. claude/polish-<date>).
- Before each phase, check what already exists and skip anything done
  (say "already done" in the report). Never redo finished work.
- After each phase: run lint, typecheck, unit tests, the static build and
  the browser tests; check changed pages at 375px and 1366px (no sideways
  scroll); commit the phase as its own commit with the phase name; push;
  give me a short report (what changed, what you skipped and why,
  screenshots where asked). Then go straight to the next phase unless
  the phase says to wait for me.
- Rules: keep the current clean style (same colours and fonts, no
  gradient text, no emoji, no badges on every link, no coloured icon
  tiles). Never invent rates, ratings, limits or tax figures; official
  sources only, with source URL and dates. No URL may break: any moved
  or merged page gets a 301 redirect in BOTH netlify.toml and
  public/_redirects, plus a test.
- Don't merge to main. At the end, open ONE pull request with all phases.

==========================================================================
PHASE 1: HOMEPAGE DECLUTTER
==========================================================================
1. "Browse calculators by category": 5–6 most popular calculators per
   category + "All <category> →". Equal-height cards, no big empty space.
   On phones, a collapsible list (closed by default) instead of tall
   cards. Use the same groups as the new menu (Phase 2).
2. "Big money questions": show 4 (₹10 lakh FD, home loan prepayment, tax
   regime, minimum due) + "More money questions →" (expandable section
   or its own page with the rest).
3. Mobile sticky result bar on the homepage: show it only while the
   visitor is inside the featured EMI calculator, not from the first
   screen.
4. One disclaimer only: remove the duplicate block above the footer on
   the homepage (keep the footer one).
5. Don't change the header, hero, search or rates strip.
Target: mobile homepage under ~9,000px tall (it's ~16,500px now).
Screenshots before/after at 375 and 1366.

==========================================================================
PHASE 2: MENU: MORE CATEGORIES, ONE CLEAN LIST EACH
==========================================================================
Today: 6 mega-menus with sub-headings; "Loans & EMI" has ~30 links and
runs off the screen. Competitors use ~11 top menus, each one clean list
of up to ~13 links. Do the same in our style.
1. Top menu (11): Loans | Credit Cards | Product EMI | Investments |
   FD & Savings | Post Office | Tax | Salary & Retirement | Insurance &
   Goals | Banks | More
2. Each dropdown (except Banks): ONE column, max 13 links, no
   sub-headings, short labels without "Calculator" (page titles keep it),
   no label wrapping, never taller than the screen, ending with
   "All <category> →". Every calculator appears in exactly one menu or in
   More. Suggested content:
   - Loans: EMI, EMI with down payment, EMI affordability, Home loan,
     Personal loan, Car loan, Bike loan, Education loan, Gold loan,
     Business loan, Loan against property, Loan prepayment, Loan
     eligibility
   - Credit Cards: Card EMI, Minimum due / interest, Payoff, Cash
     withdrawal, Forex markup, Rewards vs annual fee, Utilization, Zero
     cost EMI
   - Product EMI: Mobile, iPhone, iPhone 18, iPhone 18 Pro, iPhone 18 Pro
     Max, Laptop, TV, AC, Refrigerator, Washing machine
   - Investments: SIP, Step-up SIP, Lumpsum, ELSS, STP, SWP, Goal SIP,
     CAGR, XIRR, Compound interest, Simple interest, SIP vs lumpsum,
     Inflation
   - FD & Savings: FD, Where should I put my FD?, FD comparison, Highest
     FD rates, RD, Savings interest, Savings account rates, Highest
     savings rates
   - Post Office: PPF, Sukanya Samriddhi, SCSS, Monthly Income Scheme,
     NSC, KVP, Post Office RD, Post Office FD, Compare all schemes, Atal
     Pension Yojana
   - Tax: Income tax, Salary TDS, Advance tax, TDS, HRA, Self-employed,
     Freelancer, Capital gains, LTCG, STCG, GST
   - Salary & Retirement: Salary, DA, 8th Pay Commission, EPF, EPS
     pension, Gratuity, Leave encashment, Retirement, FIRE, Pension, NPS,
     NPS annuity (EPF vs NPS and NPS vs MF via "All →")
   - Insurance & Goals: How much insurance do I need?, Term, Health,
     Life, Human life value, Insurance needs, Child education, Home down
     payment, Rent vs buy, Debt payoff
   - More: Percentage, Average, Gold price, Guides, Money tips, How we
     verify rates, All calculators
3. BANKS menu (like competitors): a wide panel listing every bank and
   finance company we have a page or rates for, A–Z, in a 4-column grid
   (3 on smaller desktops), each with its logo (Phase 3) and short name
   (e.g. "SBI", "HDFC Bank", "AU SFB"). Above the grid, small filter
   tabs: All | Public | Private | Small finance | Finance companies |
   Post Office. Below: "All banks & rates →", "Compare FD rates →",
   "Highest FD rates →". Include the finance companies with FD rates or
   ratings in our data (Bajaj Finance, Shriram, Mahindra Finance, PNB
   Housing, LIC HFL, Sundaram, ICICI HFC) and Post Office / IPPB; link
   each to its existing page, or to the right rates page if it has no
   page. The panel must fit 1366×768 (scroll inside it, never the page).
4. Fit on one line at 1366px (logo + 11 items + search): tighten
   spacing/font slightly first; between 1024 and 1365px move the
   lowest-priority items (Product EMI, Insurance & Goals, Post Office)
   into More automatically. Never wrap to two lines.
5. Mobile menu: same groups as an accordion (one open at a time); Banks
   shows the logo list with the same filter tabs.
6. Build the menu, hub pages, breadcrumbs, /all-calculators and the
   homepage categories from ONE shared navigation data file, so they
   always match. New or renamed hubs get 301s, tests and sitemap entries.
7. Keyboard and screen readers: Esc closes, arrow keys move, focus
   visible, focus returns to the menu button.
Screenshots of every dropdown and the Banks panel at 1366, the header at
1024, and the mobile menu at 375.

==========================================================================
PHASE 3: BANK LOGOS EVERYWHERE A BANK IS NAMED
==========================================================================
All 36 logos exist (public/banks/logos, BankLogo component) but only
/banks and the bank pages use them. Add them (~20–24px, aligned, next to
the name) on: the homepage rates tiles and "Compare bank rates" tables;
the highest FD / highest savings / lowest home, personal and car loan
rate pages; FD comparison; savings account rates and the savings
calculator's bank list; the FD suggestion tool (top options, insured
split, ladder); the ratings and D-SIB sections; the Banks menu and the
mobile menu.
- Use BankLogo's fallback (initials in a neutral circle) for anything
  without a logo (finance companies, Post Office, IPPB). Don't download
  new logos without asking me; list which ones are missing.
- Check every logo file at small size: not cropped, blurry or squashed.
  Wide logos (e.g. Karnataka Bank, ICICI) get a wider box, not a
  squashed square. Fix in CSS; report any bad file.
- Decorative next to the name: alt="", lazy-loaded below the fold, fixed
  size (no layout shift), tables still readable at 375px.
- Add on /banks and the methodology page: "Bank names and logos are
  trademarks of their owners and are used only to identify the bank.
  CalcMate is not affiliated with or endorsed by any bank."
Screenshots: two ranking pages and the FD suggestion tool at 375 and
1366.

==========================================================================
PHASE 4: SMALL CONTENT FIXES
==========================================================================
1. Money Tips frequency: "every two weeks" everywhere it's mentioned
   (skip if already done).
2. NPS pages: add one line where the lump sum is taxed: "Tax on a lump
   sum above 60% of the corpus hasn't been clarified yet; this calculator
   treats it as taxable."
3. Loan fee text cut off mid-word at 160 characters (10 records): show
   the full text, or cut at a word and link to the source.
4. /banks FAQ: the 3 answers that are in the structured data but not on
   the page: show them on the page, or remove them from the JSON-LD.
5. "−₹0" formatting: show "₹0".
6. ⌘K shortcut hint: show "Ctrl K" on Windows/Linux and "⌘K" on Mac.
7. The 404 page has two robots tags: keep one (noindex).
8. robots.txt: remove the "Host:" line.

==========================================================================
PHASE 5: SEO CLEAN-UP FROM THE AUDIT
==========================================================================
1. /blog/credit-cards and /blog/product-emi are indexable but all their
   articles are noindex: make the hubs noindex too, or index the
   articles that are good enough. Tell me which you chose and why.
2. Sitemap lastmod: use each page's real last-changed date (data dates
   for rate pages, git date for content), not the build time.
3. Blog Article structured data: add an image (the og image is fine).
4. The 28 indexable bank calculator pages are near-duplicates: add a
   short, data-driven unique section to each (this bank's rate, rating,
   DICGC, how it compares with the average of all banks, last-verified
   date). No hand-typed rates. If a page still has too little unique
   content, make it noindex and tell me.
5. Fix heading jumps (h1 straight to h3) site-wide.

==========================================================================
PHASE 6: ACCESSIBILITY AND PERFORMANCE FROM THE AUDIT
==========================================================================
1. Mobile menu closes on Esc; tables that scroll sideways can be reached
   and scrolled by keyboard (tabindex="0" with a label on the scroll box).
2. The homepage result bar must not cover the rate cards.
3. Remove the 404 page's link grid from every page's data; stop dense
   link grids prefetching everything on load (prefetch on hover or
   visible only); only ship the FD data to pages that use it; reduce the
   /banks payload.
4. Lighthouse (mobile) before/after on the homepage, one calculator,
   /banks and one ranking page. Report the scores.

==========================================================================
PHASE 7: CALCULATION AND DATA NOTES FROM THE AUDIT
==========================================================================
1. FD under 6 months and EPF interest timing: check against the official
   rules and fix if wrong.
2. Very large inputs show odd numbers: set sensible max values with a
   clear message.
3. CSV downloads: guard against spreadsheet formulas (prefix cells that
   start with =, +, - or @).
4. YES Bank and other "lowest" loan rates that are realised rates (not
   card rates): label them clearly or use the card rate, per the source.
5. Kotak and CSB notes that cite a 5.25% repo rate: update them from the
   RBI's latest policy, or remove the repo mention.
6. UCO gold-loan rate from an old annexure: re-verify, or remove it.
7. Small savings rates with two different verified dates; PPF page
   citing Q2: make all small-savings pages use the current quarter's
   rates and one date.
8. HRA metro cities and the RBI credit card direction citation: check
   and correct.

==========================================================================
PHASE 8: SECURITY: CSP IN REPORT-ONLY MODE
==========================================================================
Add a Content-Security-Policy-Report-Only header (netlify.toml AND
public/_headers, plus the config test) that allows our own files,
Google AdSense, Google Analytics and Netlify Forms. Report-only means it
can't break anything. List exactly which domains it allows. I'll switch
it to enforce later.

==========================================================================
PHASE 9: PDF RATES (only if I've uploaded the files; otherwise skip)
==========================================================================
If PDFs for MOSPI CPI, India Post / NSI, IPPB, Bank of India, Bandhan,
IDBI, South Indian Bank, PNB Housing or Indian Bank loan rates are
uploaded in this session: add those rates from them (file name, the
"effective from" date on the document, today as last-verified), rebuild
the bank pages they now verify, and replace the "unverified" CPI label
once the series matches MOSPI. If none are uploaded, skip and list what's
still missing.

==========================================================================
PHASE 10: ICONS (small set, last)
==========================================================================
One icon set (keep the one the repo uses, else lucide-react; tree-shaken
SVG, no icon fonts or emoji). One icon per category and calculator,
kept in a single map. Add them ONLY to: homepage category cards and
calculator cards (hubs, /all-calculators, popular and related lists);
the mobile menu (and desktop menu if not crowded); action buttons
(download, print, share, save, copy) and key notes ("may be outdated",
"Insured up to ₹5 lakh", "Not covered by deposit insurance"). Nowhere
else. Brand blue or grey only; decorative icons aria-hidden; no layout
shift. Report bundle size before/after and show screenshots.

==========================================================================
PHASE 11: FINAL AUDIT WITH SUBAGENTS, THEN THE PR
==========================================================================
Use subagents in parallel, each with one area and read-only access:
calculation accuracy (every calculator, defaults and edge cases),
data and sources, tax and rules, SEO (titles, canonicals, sitemap vs
redirects vs noindex, broken links, one-hop 301s), mobile and
accessibility (375 and 1366, keyboard, contrast), forms/privacy/security,
and performance. Verify every blocker and high yourself, fix the
confirmed ones (ask me before anything that changes a feature or the
design), list mediums and lows. Re-run everything, then open ONE pull
request into main with all phases, a summary per phase, and a clear
verdict: READY TO DEPLOY or NOT READY (with remaining blockers). Include
the exact checks for me to run on the Netlify deploy preview (forms,
redirects, 404, headers, a few calculators, the menu and Banks panel on
phone and desktop).
````
