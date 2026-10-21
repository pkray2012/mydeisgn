# CalcMate: fix list and Claude prompt

How to use this file:

1. Open a new Claude Code session with the CalcMate source code repository attached.
2. Upload your logo to that session. An SVG is best; otherwise use a square PNG of at least 512×512.
3. Copy everything inside the box below and paste it as your first message.

The prompt includes everything found in the audit of https://calcmate.in (October 2026). The other session doesn't need this conversation.

---

````text
You are working on the source code of CalcMate (https://calcmate.in), an Indian
financial calculator website with about 80 calculators. It appears to be built
with React/Next.js and Tailwind; the class names include ink-*, brand-* and
bg-brand-50. Confirm the stack from the repository before changing anything.

An audit of the live site found the problems listed below. Work in 4 phases,
in order. Finish each phase, run the project's build, lint and tests, and
check it in a browser at 375px (mobile) and 1366px (desktop) widths. Commit
each phase separately with a clear message, then move to the next phase.
Don't change the site's overall visual style: it's clean and simple, and the
owner likes it. Don't add gradient headline text, emoji in buttons, a badge
on every link, or a different colour for every icon.

If anything below doesn't match what you find in the code, tell me instead
of guessing.

==========================================================================
PHASE 1: BUGS (fix these first)
==========================================================================

1. INCOME TAX CALCULATOR GIVES TWO DIFFERENT ANSWERS (/income-tax-calculator)
   With the default inputs (gross ₹12,00,000; 80C ₹1,50,000; 80D ₹25,000):
   - The green summary box at the top says: "...compared to ₹1,63,800 under
     the Old Regime, saving ₹1,63,800 annually."
   - The result cards and the comparison table below say: Old Regime tax
     ₹1,11,800, and "You save ₹1,11,800".
   The summary box seems to ignore the itemised deductions. ₹1,63,800 is
   exactly the old-regime tax with only the ₹50,000 standard deduction.
   Fix: compute the summary box, the result cards, the comparison table and
   the sticky bottom bar from ONE shared calculation result, so they can't
   disagree. Check that the summary also updates live when inputs change.
   Look for the same "summary sentence computed separately" pattern on every
   other calculator that has a green summary box, and fix it the same way
   wherever it exists.

2. RUPEE SIGN SHOWN TWICE ("₹₹")
   Seen on the live site:
   - /income-tax-calculator: "(₹₹0/mo)", "(₹₹9,317/mo)",
     "₹₹50,000 std + ₹₹1,75,000 itemized"
   - /hra-calculator: "₹₹2,40,000", "₹₹60,000"
   - /rent-vs-buy-calculator: "₹₹64,00,000", "₹₹16,00,000", "₹₹4,80,000",
     "₹₹1,22,98,644"
   - /salary-calculator: "₹₹10,24,752"
   Cause: a string adds "₹" in front of a value that the currency formatter
   already returns with "₹". Fix it in the shared formatter or in those
   templates. Then search the whole codebase for the same pattern (a
   template string with "₹${" or "₹{" around a formatted currency value)
   and fix every occurrence. Add a unit test that the formatter output
   never starts with "₹₹".

3. TAX YEAR IS OUT OF DATE
   It is now October 2026, so the current year is FY 2026-27 (AY 2027-28).
   These pages still say FY 2025-26 / FY 2025–26:
   /income-tax-calculator, /salary-calculator, /freelancer-tax-calculator,
   /self-employed-tax-calculator, /salary-tds-calculator. The last one shows
   both FY 2025-26 and FY 2026-27, which is inconsistent.
   Fix:
   a) Put the current FY/AY in ONE constant and use it everywhere, so it
      never needs to be changed on each page again.
   b) Check the FY 2026-27 slabs, standard deduction, 87A rebate limit,
      marginal relief, surcharge and cess against official sources
      (incometax.gov.in, the Finance Act 2026 / Budget 2026 documents).
      DO NOT guess tax numbers. If you can't confirm them, keep the existing
      numbers, change only the label to the year they actually belong to,
      and list for me what still needs confirming.
   c) Also check the page titles, meta descriptions, FAQ text and JSON-LD on
      those pages for old year references.

4. SLIDER AND TEXT BOX SHOW DIFFERENT VALUES
   On these pages the text box shows one number and the slider sits on a
   different one, because the slider's step doesn't line up with the
   default value:
   - /loan-prepayment-calculator: text 20,00,000, slider 20,10,000 (step 50,000)
   - /emi-with-down-payment-calculator: text 8,00,000, slider 8,05,000 (step 10,000)
   - /iphone-18-pro-emi-calculator: text 1,64,900, slider 1,65,000 (step 1,000)
   - /iphone-18-pro-max-emi-calculator: text 1,79,900, slider 1,80,000 (step 1,000)
   - /post-office-mis-calculator: text 9,00,000, slider 8,76,000 (step 25,000)
   - /scss-calculator: text 15,00,000, slider 15,01,000 (step 50,000)
   - /sukanya-samriddhi-calculator: text 1,50,000, slider 1,49,250 (step 1,000)
   Fix: the text box is the source of truth. Check which value the
   calculation actually uses and make sure it's the text box value. Make
   the slider's step and min line up with typical values (for example,
   min 0 or a round number). Also make sure that typing a value that isn't
   on a step doesn't move the stored value; the slider may display the
   nearest position. Fix this in the shared slider/input component, not
   page by page.

5. 48 OF 88 PAGES SCROLL SIDEWAYS ON PHONES (tested at 375px width)
   The page is wider than the screen (scrollWidth was 396–615px against
   375px), so the whole page wobbles left and right on mobile. There are
   two causes:
   a) The sticky bottom result bar (classes "fixed inset-x-0 bottom-0 z-30 ...",
      inner "mx-auto flex h-16 max-w-5xl items-center justify-between gap-4",
      right badge "shrink-0 text-right" > "inline-block rounded-lg bg-brand-50
      ...") is too wide. For example, "Saves ₹1,11,800 compared to Old Regime"
      runs off the right edge. Fix: allow the text to shrink and wrap
      (min-w-0 on the flex children, remove shrink-0 from the long badge or
      shorten the text on mobile, e.g. "Saves ₹1,11,800").
      Pages: advance-tax, capital-gains-tax, car-loan-emi, epf-vs-nps,
      freelancer-tax, gratuity, health-insurance, hra, human-life-value,
      income-tax, insurance-needs, kvp, life-insurance, loan-interest,
      ltcg-tax, nps-annuity, nps, nps-vs-mutual-fund, nsc, pension, pf,
      post-office-mis, post-office-schemes-comparison, ppf, rent-vs-buy,
      retirement, salary, salary-tds, scss (and probably others).
   b) The formula "variables" rows in the Formula section (row:
      "flex items-center gap-2 rounded-lg bg-ink-50/60 px-3 py-1.5 border",
      description: dd "text-xs sm:text-sm text-ink-700 truncate"). truncate
      doesn't work without min-w-0 on the flex child, so long descriptions
      push the page wider. Fix: add min-w-0 to the dd and let the text wrap
      on mobile (replace truncate with break-words, or stack the term above
      its description below the sm breakpoint).
      Pages: 8th-pay-commission, atal-pension-yojana, da, eps-pension,
      gold-price, gst, inflation, leave-encashment, loan-prepayment,
      post-office-fd, post-office-rd (and probably others).
   After fixing, add a check (Playwright or similar) that loads every
   calculator page at 375px and fails if
   document.documentElement.scrollWidth > clientWidth.

6. SIP CARD PROMISES A CHART THAT DOESN'T EXIST
   The homepage and listing card for the SIP calculator say "See total
   wealth gain, year-by-year compounding growth chart", but /sip-calculator
   only has the invested-vs-returns donut and a year-by-year table that is
   hidden by default. Fix this by building the growth chart (Phase 3,
   item 1). Until then, change the card text. Check the other card
   descriptions for promises the page doesn't keep.

7. SEO: PAGE TITLES TOO LONG (over 65 characters, so Google cuts them off)
   advance-tax (78), capital-gains-tax (69), epf-vs-nps (71),
   freelancer-tax (71), health-insurance (77), insurance-needs (74),
   kvp (74), life-insurance (77), ltcg-tax (72), nps-annuity (78),
   nps-vs-mutual-fund (75), pension (67), post-office-schemes-comparison (76),
   rent-vs-buy (75), retirement (69), salary-tds (74), self-employed-tax (74),
   stcg-tax (76), tds (73).
   Fix: shorten each to 50–60 characters, keeping the main keyword first,
   e.g. "Advance Tax Calculator FY 2026-27 – Due Dates & Interest | CalcMate".
   Don't change URLs.

==========================================================================
PHASE 2: LOGO (I have uploaded the logo file in this session)
==========================================================================

Find the uploaded logo file and use it everywhere the brand appears. The
current header logo is a green rounded square with "₹" next to the word
"CalcMate".
1. Header: replace the current mark with the new logo. Keep the header
   height the same; on mobile the logo must fit next to the menu button.
   Use an <img>/<svg> with alt="CalcMate" and explicit width/height (no
   layout shift). If the logo already contains the word "CalcMate", don't
   also print the word next to it.
2. Footer: same logo, smaller.
3. Favicons, generated from the logo:
   favicon.ico (16+32+48), icon.svg (if the logo is SVG),
   apple-touch-icon.png 180×180 (solid background, no transparency),
   icon-192.png and icon-512.png, plus a maskable 512 icon with safe padding.
   Wire them up the way this framework expects (e.g. Next.js app/icon.*,
   app/apple-icon.*, or <link> tags) and update the web manifest
   (name, short_name, theme_color, background_color, icons).
4. Default Open Graph / Twitter share image (1200×630): logo + "Free
   Financial Calculators for India" on the brand background. Use it on
   every page that doesn't have its own image.
5. JSON-LD: add or update the Organization "logo" (an absolute URL to a
   PNG, at least 112×112) and "name": "CalcMate".
6. If the logo's main colour clearly differs from the current brand teal,
   ask me before changing brand colours. Don't change them on your own.
If no logo file was uploaded, stop this phase and ask me for it.

==========================================================================
PHASE 3: CALCULATOR IMPROVEMENTS
==========================================================================

Build each improvement as a reusable component used by all the relevant
calculators, not a copy on each page. Keep everything client-side, with no
sign-up.

1. Year-by-year growth chart (stacked area or line: amount invested vs
   value) on SIP, Step-Up SIP, Lumpsum, FD, RD, PPF, NPS, EPF, Sukanya
   Samriddhi, Retirement and Compound Interest. Use the same green/amber
   colours as the existing donut. Show values in the Indian format
   (₹12.5 L, ₹1.2 Cr). Must work on mobile and when JavaScript is slow (the
   numbers and table are still in the HTML).
2. Download the schedule: "Download CSV" and "Print / Save PDF" buttons for
   every amortisation schedule and year-by-year table.
3. Shareable links: put the inputs in the URL query string
   (e.g. ?amount=5000000&rate=8.5&years=20), so people can share or bookmark
   a result. The page must still work, and still render the default values
   in the HTML, when there's no query string. Keep the canonical URL without
   the query.
4. Save and compare (no login): a "Save this scenario" button that stores
   inputs and results in localStorage, plus a small "Compare saved" panel
   showing 2–3 saved scenarios side by side. Wrap all localStorage access
   in try/catch.
5. Home Loan EMI calculator:
   - Optional prepayments inside the same calculator: a one-time lump sum
     in month N, and/or an extra amount every month or year, with "reduce
     tenure" vs "reduce EMI". Show interest saved and months saved.
   - Optional under-construction mode: a pre-EMI / moratorium period in
     months, with the pre-EMI interest shown and added to the total cost.
6. Fill in calculators from homepage links: support the URL parameters
   from item 3 so the homepage "question" cards (Phase 4) can open a
   calculator with values already filled in.

==========================================================================
PHASE 4: DESIGN IMPROVEMENTS (keep the current clean style)
==========================================================================

1. Homepage calculator list: the homepage currently shows all ~80
   calculators as large cards, which makes a very long scroll. Replace this
   with about 8 category cards (EMI, Loans, Product EMI, Investment &
   Savings, Tax & Salary, Post Office & Govt schemes, Insurance & Retirement,
   Credit Cards / Math). Each card has a title, one line of description,
   its 5–6 most popular calculators as plain links, and an "All <category> →"
   button to the existing hub page. Keep the existing featured EMI
   calculator at the top. The full list stays on /all-calculators.
2. "Big questions" section on the homepage: 4 cards with a real question
   and a link that opens the calculator already filled in (Phase 3,
   item 6), e.g.
   - "Can a ₹3,000 SIP started at 25 beat a ₹10,000 SIP started at 35?"
   - "If I pay ₹3,000 extra on my home loan EMI every month, how much do I
     save?"
   - "New or Old tax regime: which is better at ₹15 lakh salary?"
   - "Rent and invest, or buy a home in 2026?"
   Compute the numbers shown on the cards with the real calculator
   functions, not hard-coded text.
3. Live rates strip below the hero: 4–6 small tiles such as "Best FD rate:
   up to X%", "Home loan from X%", "PPF X%", "SSY X%", "SCSS X%", each
   linking to the matching calculator or /banks page. Take the numbers from
   the data the site already uses (bank directory / rate constants). Don't
   invent rates, and show an "as of <date>" label.
4. On calculator pages longer than 2 screens: a small "On this page" list
   of the H2 sections (sticky in a side column on desktop, collapsible at
   the top on mobile).
5. One trust line under the homepage search, in small grey text with
   simple icons: "Free · No sign-up · Formulas as per RBI and Income Tax
   rules · Download schedules". No large coloured feature cards.
6. Check tap targets on mobile: buttons, preset chips (1 yr, 2 yr...) and
   the Mo/Yr toggle should be at least 40px tall.

Don't copy from competitor sites: gradient-coloured headline text, purple
gradient panels, emoji inside buttons and pills, a coloured badge
("POPULAR", "SAVE LAKHS") on almost every link, multi-coloured icon tiles,
or dark full-width promo sections. Keep the calculator server-rendered and
visible at the top of the page. Don't hide it behind a "Loading
calculator..." spinner.

==========================================================================
WHEN YOU FINISH EACH PHASE
==========================================================================
- Run the build, lint and tests, and report the exact results.
- Give me a short list of what changed and any item you skipped, with the
  reason.
- For Phase 1, give a before/after table for items 1–5 (page, what it
  showed, what it shows now).
- Commit, and push to the branch you were told to use.
````

---

## Audit notes (for the owner, not part of the prompt)

- I tested 88 CalcMate pages: every calculator plus the hub pages, at 1366px and 375px.
- Some pages showed "₹" followed by "₹25 Lakh" (SWP, RD, SCSS, Step-Up SIP, Sukanya, Post Office MIS). Those are false alarms: an input's ₹ prefix followed by preset buttons. They aren't in the prompt.
- No page showed NaN, undefined or negative money values. Every page has exactly one H1, a canonical URL, a meta description of normal length, and structured data (WebPage, WebApplication, BreadcrumbList, FAQPage).
- I couldn't test financialcalculator.io's working calculators, because its bot protection blocks automated browsers from loading its scripts. The ideas taken from it come from its page layout and its own description of its features.
