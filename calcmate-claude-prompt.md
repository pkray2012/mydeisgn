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

An audit of the live site found the problems listed below. Work in 7 phases,
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
PHASE 5: SAVINGS ACCOUNT INTEREST CALCULATOR + ALL-BANK RATE COMPARISON
==========================================================================

Build two new pages in the same style as the existing calculators, and
link them from the homepage, the Investment & Savings hub, /banks, the
sitemap and the related-calculator lists.

A) /savings-account-interest-calculator
   Inputs:
   - Bank (dropdown of every bank in the rates data below, plus "Custom rate")
   - Balance: either one average balance, or a simple month-by-month
     balance table (12 rows) for people whose balance changes
   - Period: months or years
   - Interest credit frequency: quarterly (default) or monthly. Pre-fill it
     from the chosen bank's data when known.
   - Age: below 60 / 60 and above (for the tax deduction)
   - Tax regime: New / Old
   Calculation rules (check them against RBI and Income Tax rules before
   coding, and write them in the page's "Formula" and "Key assumptions"
   sections):
   - Interest is calculated on the daily closing balance and credited at
     the chosen frequency. Compounding happens only at each credit.
   - SLAB RATES: many banks pay different rates on different parts of the
     balance (e.g. 2.70% up to ₹1 lakh, 3% on the part above ₹1 lakh).
     Support both styles, and store which one each bank uses:
     "tiered" (each part of the balance gets its own slab rate) and
     "whole balance" (the whole balance gets the slab it falls into).
   - Tax: Old regime: Section 80TTA deduction up to ₹10,000 (below 60) or
     Section 80TTB up to ₹50,000 (60+, which also covers FD interest).
     New regime: no 80TTA/80TTB. Show taxable interest. No TDS applies to
     savings account interest; say so.
   Outputs: total interest, effective annual yield, interest per credit
   period (table), taxable interest, and "how much more you'd earn at the
   best-paying bank for this balance". Use the shared components from
   Phase 3 (shareable URL inputs, CSV download, save/compare).

B) /savings-account-interest-rates (comparison page)
   - One table of ALL banks in the data: bank, bank type (public, private,
     small finance, payments bank), the rate for the balance entered, the
     highest rate and the balance it needs, minimum balance, credit
     frequency, "last verified" date, and a link to the bank's official
     rate page.
   - A balance box at the top (default ₹1,00,000). Sort by the rate earned
     on that balance. Filters for bank type and "no minimum balance".
   - Each row has a "Calculate" link that opens the calculator with that
     bank and balance filled in.
   - Title: "Savings Account Interest Rates Today (<Month Year>): All Banks
     Compared | CalcMate". Put the month in automatically from the data's
     newest verified date, never from the build date.
   - Also show each bank's savings rate on its existing /banks/<bank> page,
     using the same data.

C) RATES DATA: THIS IS THE IMPORTANT PART. DO NOT INVENT RATES.
   - Store the rates in ONE data file (JSON or TS) next to the existing
     bank/FD rate data. For each bank: slabs [{from, to, rate}], slab
     style, credit frequency, minimum balance, source URL (the bank's
     official interest-rate page or PDF), "effective from" date, and
     "last verified" date.
   - Cover every bank in the current /banks directory: SBI, PNB, Bank of
     Baroda, Bank of India, Bank of Maharashtra, Canara, Central Bank of
     India, Indian Bank, Indian Overseas Bank, Punjab & Sind, UCO, Union
     Bank, IDBI, HDFC, ICICI, Axis, Kotak, IDFC First, IndusInd, Yes, RBL,
     Federal, South Indian, Karnataka, City Union, CSB, DCB, Dhanlaxmi,
     Tamilnad Mercantile, Bandhan, AU SFB, Equitas SFB, Jana SFB, Suryoday
     SFB, Ujjivan SFB, Utkarsh SFB. Add India Post Payments Bank and the
     Post Office Savings Account (4% p.a., from nsiindia.gov.in /
     indiapost.gov.in) as their own rows.
   - Get each bank's current rates from that bank's OFFICIAL website or
     rate PDF. Many banks moved to new .bank.in addresses (e.g. a
     <name>.bank.in domain), so try both the old and the new address.
     Don't use rates from blogs, aggregator sites or your own memory.
   - If a bank's page can't be loaded or read, don't guess. Keep its
     existing value if the site has one, with its old "last verified"
     date; otherwise leave the bank out of the table and list it for me.
     At the end, give me a table: bank | rates found | source URL |
     verified (yes / could not load).
   - On both pages, show "Rates as of <date>" and a short note: "Rates
     change often. Check with the bank before opening an account." If a
     bank's "last verified" date is older than 45 days, show a small
     "may be outdated" marker on its row.
   - Add a small script (e.g. `npm run check:savings-rates`) that lists
     banks whose data is older than 45 days, so the data can be updated
     regularly. Don't make the website itself fetch bank sites while
     people are using it.
   - Add unit tests for the calculation: tiered vs whole-balance slabs,
     quarterly vs monthly credit, the 80TTA/80TTB limits, and a balance
     exactly on a slab boundary.

==========================================================================
PHASE 6: "WHAT SHOULD I DO?" SUGGESTION TOOLS (FD BY AMOUNT + INSURANCE COVER)
==========================================================================

Both tools give options worked out from the user's numbers, not product
sales. Show this line on both: "These are calculated options, not
financial advice." Never rank insurers or show affiliate products.

A) FD SUGGESTION BY AMOUNT: /fd-suggestion (title: "Where Should I Put My
   FD? Best FD Options for Your Amount")
   Inputs:
   - Amount to invest (e.g. ₹7,50,000)
   - How long: a tenure (months/years) or "I need the money on <date>"
   - Age: below 60 / 60+ (senior rates)
   - Payout: at maturity / monthly / quarterly interest
   - Tax: regime, and slab rate or annual income (for post-tax return)
   - Bank types to include: public / private / small finance / Post Office
     (small finance banks are on by default, with a one-line note on what
     they are)
   - "Might I need some money early?" yes/no
   Use the same official FD rates data as the FD Comparison page (same
   rules as Phase 5 part C: official sources only, "last verified" dates,
   never invented). Use the rate for the user's amount AND tenure (rates
   change with tenure, and amounts of ₹3 crore and above are "bulk" with
   different rates).
   Output, in this order:
   1. Top 5 options for this amount and tenure: bank, rate, maturity
      amount, interest, post-tax interest, payout per month/quarter if
      chosen, and "last verified" date. Each opens the FD calculator
      already filled in.
   2. DEPOSIT INSURANCE CHECK (DICGC): deposits are insured up to ₹5 lakh
      per depositor per bank (principal + interest, all accounts in that
      bank added together). Check this limit and its rules on
      dicgc.org.in before coding. If the amount plus interest exceeds
      ₹5 lakh, suggest a split across 2–3 high-rate banks so each stays
      within the cover, and show the rate difference vs putting it all in
      one bank. Also add this check to the existing FD calculator and the
      FD Comparison page as a small note.
   3. FD LADDER option (if the "need money early" answer is yes, or the
      amount is large): split into 3–5 FDs maturing at different times
      (e.g. 1, 2, 3 years). Show the blended rate and when each part
      becomes available.
   4. TAX: TDS is deducted when FD interest from one bank in a year
      crosses the threshold (check the current limits for regular and
      senior depositors on incometax.gov.in; don't use remembered
      numbers). Mention Form 15G/15H when the user's income is below
      taxable. In the old regime, mention the 5-year tax-saver FD (80C).
   5. ALTERNATIVES WORTH COMPARING for the same amount and tenure, using
      the site's existing calculators: Post Office Time Deposit, and for
      seniors SCSS and POMIS (with their deposit limits). Link to each.
   6. A one-sentence answer at the top, above the details, like the other
      calculators have.

   WORKED EXAMPLE TO BUILD AND TEST AGAINST: "I have ₹10 lakh"
   Inputs: ₹10,00,000, 2 years, age 35, interest at maturity, new regime,
   30% slab, all bank types, no early need. The page should show:
   - Top answer: "For ₹10 lakh over 2 years, the highest rates are at
     <Bank A> (<x>%) and <Bank B> (<y>%). Split it as ₹4.5 lakh + ₹4.5 lakh
     + ₹1 lakh across <Bank A>, <Bank B> and <Bank C> to stay fully
     insured, and you'd get about ₹<maturity> back (₹<post-tax> after tax)."
     The bank names, rates and amounts come from the rates data. Never
     hard-code or invent them.
   - Why it splits: ₹10 lakh in one bank leaves ₹5 lakh+ uninsured. Each
     part must stay at or under ₹5 lakh INCLUDING the interest it will
     earn by maturity, so ₹5,00,000 in one bank isn't enough room. Work
     out the largest principal per bank that stays ≤ ₹5 lakh at maturity
     for that bank's rate and tenure, and fill the highest-rate banks first.
   - A table comparing: all in one bank vs the suggested split vs a
     1/2/3-year ladder: total maturity, post-tax interest, amount insured,
     and when the money becomes available.
   - TDS note: interest per bank per year compared with the TDS
     threshold, so the user sees which bank will deduct TDS.
   - The same example with age 65 should switch to senior rates and also
     show SCSS for comparison.
   Put this example (with mocked rates in the test, not real ones) in the
   unit tests.

B) INSURANCE COVER SUGGESTION: improve the existing
   /term-insurance-calculator, /insurance-needs-calculator,
   /human-life-value-calculator and /health-insurance-calculator, and add
   one simple entry page /how-much-insurance-do-i-need that asks a few
   questions and gives BOTH numbers:
   1. LIFE (TERM) COVER
      Inputs: age, annual income, monthly household expenses, years until
      the youngest dependant is independent / years to retirement,
      outstanding loans, future goals (child education, marriage) in
      today's money, existing life cover, existing savings/investments,
      expected inflation and return.
      Recommended cover = present value of the expenses the family needs
      until those years + loans + goals (inflated) − existing cover −
      savings. Round up to the nearest ₹25 lakh. Show the breakdown so the
      user can see where the number comes from. Show "10–15× annual
      income" only as a simple check next to it, not as the answer.
      Suggest the policy tenure (cover until the age the user retires or
      dependants become independent).
   2. HEALTH COVER
      Inputs: city (metro / tier-2 / tier-3), family members and ages,
      existing employer cover, any parents to cover separately.
      Give a suggested base cover plus a super top-up amount, with the
      reason (city hospital costs, family size, age, employer cover ending
      when the job changes). Keep the suggestion amounts in ONE
      clearly-commented config file so they can be updated easily, and
      say in the page's assumptions what they are based on.
      Show the Section 80D limits (check the current limits first).
   3. Don't estimate or show premiums unless the site already has a
      sourced premium dataset. If it doesn't, say "Premiums vary by
      insurer and health; get quotes" and link to irdai.gov.in for buying
      tips.
   4. Results: one clear summary card ("You may need about ₹1.5 Cr life
      cover for 25 years and ₹10 L health + ₹25 L super top-up"), the
      breakdown, the Phase 3 shareable link / save / CSV, and links to the
      detailed calculators.

Add unit tests for: the DICGC split (exactly ₹5 lakh, just above it,
interest pushing it over), the ladder amounts adding up to the input, and
the term-cover formula with zero/negative gaps (cover can't go below 0).

==========================================================================
PHASE 7: HIGHEST / LOWEST RATE RANKINGS + BANK AND FINANCE COMPANY RATINGS
==========================================================================

A) HIGHEST AND LOWEST RATES
   Using the same verified rates data (official sources only, "last
   verified" dates, never invented):
   1. On the FD Comparison, Savings Account Rates and loan pages, add a
      sort toggle "Highest first / Lowest first", plus two small
      summary boxes at the top: "Highest: <bank> <rate>" and "Lowest:
      <bank> <rate>" for the amount and tenure the user picked.
   2. New ranking pages, each with a table, the "as of <date>" line, and a
      "Calculate" button on each row:
      - /highest-fd-interest-rates (general and senior citizen tabs; filter
        by tenure: 1 yr, 2 yr, 3 yr, 5 yr; filter by bank type)
      - /highest-savings-account-interest-rates
      - /lowest-home-loan-interest-rates
      - /lowest-personal-loan-interest-rates
      - /lowest-car-loan-interest-rates
      - /lowest-education-loan-interest-rates and /lowest-gold-loan-interest-rates
        only if the site already has those rates in its data
      For deposits, "highest" is good; for loans, "lowest" is good. Label
      them that way and colour them the same way. For loans, show the
      "starting from" rate AND say it depends on credit score, income and
      employer, and that processing fees aren't included. Show processing
      fees where the data has them.
   3. Rank on the rate for the exact tenure and amount shown. Small
      finance banks and company FDs are shown with a clear label (see B)
      so the top of the list is never misleading.

B) BANK AND FINANCE COMPANY RATINGS (factual credit ratings only)
   1. Add a "Rating" column to the FD comparison, savings rates, the
      ranking pages, the FD suggestion tool (Phase 6) and each /banks/<bank>
      page. It shows the CREDIT RATING given by a SEBI-registered rating
      agency, e.g. "CRISIL AAA/Stable", "ICRA AA+/Stable", "CARE A+".
      - For company FDs / NBFCs (e.g. Bajaj Finance, Shriram Finance,
        Mahindra Finance, and any other finance companies you add): use the
        rating of their FIXED DEPOSIT programme (FD ratings use a different
        scale, e.g. "AAA (FD)" or "MAAA"; show it exactly as the agency
        writes it).
      - For banks: use the bank's long-term issuer or bond rating, and say
        which instrument it is. Banks' deposits usually have no separate
        rating, so don't make one up.
   2. Store ratings in one data file: entity, agency, instrument, rating,
      outlook, date of the rating action, and the URL of the agency's
      rating rationale/press release. Get them ONLY from the agencies'
      official sites: CRISIL (crisilratings.com), ICRA (icra.in), CARE
      (careratings.com), India Ratings (indiaratings.co.in), Acuité
      (acuite.in), Brickwork (brickworkratings.com), Infomerics
      (infomerics.com). If you can't find or load a rating, leave the cell
      as "Not available" and list it for me. Don't use blogs, news or memory.
   3. DO NOT create CalcMate's own star ratings, scores, "safest bank"
      labels or rankings of banks by safety. Only show the agencies'
      ratings, with the agency name and date, and link to the source.
   4. Add a short "What do these ratings mean?" box (AAA = highest
      safety, then AA, A, BBB; anything below BBB is not investment grade;
      outlook Stable/Positive/Negative/Watch) and the note: "Credit
      ratings are opinions of the rating agency, not a guarantee."
   5. Make the safety differences clear, using facts:
      - Bank FDs (including small finance banks) are covered by DICGC
        deposit insurance up to ₹5 lakh per depositor per bank.
      - Company / NBFC FDs are NOT covered by DICGC. Show a clear label
        "Not covered by deposit insurance" on every company FD row, and
        don't let the FD suggestion tool (Phase 6) put company FDs in the
        "insured split".
      - Optionally mark the banks RBI lists as D-SIBs (domestic
        systemically important banks), using RBI's latest list from
        rbi.org.in, with its date.
   6. Add a filter "Minimum rating" (e.g. AAA only / AA and above / all)
      on the FD comparison, ranking pages and the FD suggestion tool. On
      the FD suggestion tool, default it to "AA and above" for company FDs.
   7. Add a check to the stale-data script from Phase 5
      (`npm run check:savings-rates`, or a new `check:ratings`) that lists
      ratings older than 12 months.

==========================================================================
WHEN YOU FINISH EACH PHASE
==========================================================================
- Run the build, lint and tests, and report the exact results.
- Give me a short list of what changed and any item you skipped, with the
  reason.
- For Phase 1, give a before/after table for items 1–5 (page, what it
  showed, what it shows now).
- For Phase 5, give the bank-by-bank verification table from part C.
- For Phase 6, show the ₹10 lakh worked example's output with the real
  rates data, and the life/health cover suggestion for one sample family.
- For Phase 7, give a table: bank/company | agency | instrument | rating |
  date | source URL | found (yes / not available).
- Commit, and push to the branch you were told to use.
````

---

## Audit notes (for the owner, not part of the prompt)

- I tested 88 CalcMate pages: every calculator plus the hub pages, at 1366px and 375px.
- Some pages showed "₹" followed by "₹25 Lakh" (SWP, RD, SCSS, Step-Up SIP, Sukanya, Post Office MIS). Those are false alarms: an input's ₹ prefix followed by preset buttons. They aren't in the prompt.
- No page showed NaN, undefined or negative money values. Every page has exactly one H1, a canonical URL, a meta description of normal length, and structured data (WebPage, WebApplication, BreadcrumbList, FAQPage).
- I couldn't test financialcalculator.io's working calculators, because its bot protection blocks automated browsers from loading its scripts. The ideas taken from it come from its page layout and its own description of its features.
