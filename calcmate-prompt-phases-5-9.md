# CalcMate: Claude prompt for Phases 5–9

How to use this file:

1. Open a Claude Code session with the CalcMate source code repository attached.
2. Copy everything inside the box below and paste it as your first message.

Phases 1–4 (bugs, logo, calculator improvements, design) are already done, so this prompt only covers Phases 5–9. Claude checks the code first and skips anything that already exists.

---

````text
You are working on the source code of CalcMate (https://calcmate.in), an Indian
financial calculator website with about 80 calculators. Check the stack from
the repository before changing anything (it appears to be React/Next.js with
Tailwind, hosted on Netlify).

Earlier work (Phases 1–4: bug fixes, new logo, shared calculator features,
design changes) is already done in the code but not yet live. Your job is
Phases 5, 6, 7, 8 and 9 below. Build on top of the branch that has the
Phase 1–4 work; if it's not obvious which branch that is, ask me first.

==========================================================================
STEP 0: CHECK WHAT ALREADY EXISTS, AND SKIP IT
==========================================================================
Before writing any code:
1. Read the repository and find what already exists for each item in
   Phases 5–8: pages/routes, components, rates and ratings data files,
   scripts and tests. The Phase 1–4 changes are in the code but NOT yet
   deployed, so the live site (https://calcmate.in) is out of date. Treat
   the code on your branch as the truth, and don't report items as
   missing just because the live site doesn't show them.
2. Find the shared components from the earlier work: shareable URL inputs
   (query string), CSV / print download, save & compare (localStorage),
   the growth chart, the formatter for ₹ values in Indian format, and the
   FY/AY constant. Reuse them. If one is missing, build a simple shared
   version once and reuse it. Don't copy code onto each page.
3. Give me a short table first: item | status (done / partly done / not
   started) | where in the code. Then:
   - DONE: skip it. Don't rebuild or restyle it. Only fix it if you find a
     real bug, and tell me what the bug was.
   - PARTLY DONE: finish only the missing parts.
   - NOT STARTED: build it.
4. If something in the code doesn't match what this prompt describes,
   tell me instead of guessing.

General rules for every phase:
- Keep the site's current clean style. Don't add gradient headline text,
  emoji in buttons, a badge on every link, or a different colour for
  every icon. Calculators stay server-rendered and visible at the top of
  the page, with no "Loading calculator..." spinner.
- Never invent rates, ratings, limits or tax numbers. Use official sources
  only, store "last verified" dates, and list anything you couldn't verify.
- Every new page must work at 375px width with no sideways scrolling.
  Wide tables become stacked cards on phones.
- Every new page needs: one H1, a title of 50–60 characters, a meta
  description, a canonical URL, breadcrumbs, FAQ, related-calculator
  links, and an entry in the sitemap and the right hub page.
- Finish each phase, run the build, lint and tests, check it in a browser
  at 375px and 1366px, and commit each phase separately with a clear
  message before moving to the next.

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
   best-paying bank for this balance". Use the site's existing shared components
   for shareable URL inputs, CSV download and save/compare (see STEP 0).

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
      breakdown, the existing shareable link / save / CSV components, and links to the
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
PHASE 8: EXTRA IMPROVEMENTS (skip any that already exist)
==========================================================================
1. ONE RATES "SOURCE OF TRUTH". FD, savings, loan rates, Post Office
   rates and credit ratings all live in clearly separated data files with
   the same fields (source URL, effective date, last verified date). Every
   page, ranking and tool reads from them. No rate is typed directly into
   page text, titles or FAQ answers; generate those sentences from the data.
2. RATE CHANGE MARKERS. When a rate is updated, keep the previous value
   and date. Show a small "↑ 0.25% since <date>" or "↓ ..." next to rates
   that changed in the last 60 days, on comparison and ranking pages.
3. REAL RETURN AFTER TAX AND INFLATION. On the FD comparison, FD
   suggestion tool and savings rates page, add an optional column/line:
   post-tax rate and "real return" after inflation. Take the inflation
   number from the site's existing Inflation calculator data (official
   CPI), with its date. Keep it simple and explained in one line.
4. TAX-SAVER FD AND SENIOR CITIZEN VIEWS. On /highest-fd-interest-rates,
   add a "5-year tax-saver FD (80C, old regime)" tab and make sure the
   senior citizen tab also shows SCSS and POMIS rates for comparison.
5. METHODOLOGY PAGE. Update the existing /bank-rates-methodology page to
   explain, in plain words: where every rate and rating comes from, how
   often it's checked, how rankings are ordered (exact amount and tenure),
   what the "may be outdated" marker means, how the DICGC split is worked
   out, and that CalcMate doesn't take payment from banks for rankings
   (only if that's true; ask me). Link to it from every rates page.
6. STRUCTURED DATA. On ranking and comparison pages, add ItemList JSON-LD
   for the table rows, and set dateModified (and the sitemap lastmod) from
   the newest "last verified" date in the data, not the build date.
   Validate with validator.schema.org.
7. MONTHLY DATA CHECK. Add a scheduled GitHub Action (monthly) or a
   Netlify scheduled function, whichever fits the repo, that runs the
   stale-rates and stale-ratings scripts and opens a GitHub issue listing
   what is out of date. It only reports; it never changes rates by itself.
   Ask me before adding any secret or token.
8. INTERNAL LINKS. Link these together: FD suggestion → FD comparison →
   highest FD rates → each bank's page → savings rates → savings
   calculator; insurance cover suggestion ↔ term/health/HLV calculators.
   Add the new tools to the homepage category cards and to the "big
   questions" section, e.g. "I have ₹10 lakh: where should I put it in FD?"
   opening /fd-suggestion with ₹10 lakh filled in.
9. ACCESSIBLE TABLES. Sortable table headers must be buttons with
   aria-sort; filters must have labels; the "Highest/Lowest" toggle must
   work by keyboard.

==========================================================================
PHASE 9: BACKEND NEEDS REVIEW + NETLIFY FORM ON THE CONTACT PAGE
==========================================================================

A) WHAT DOES THE SITE NEED BEHIND THE SCENES? (report first, then build)
   Before building anything here, give me a short written review:
   1. For each feature in Phases 5–9, does it need anything beyond static
      pages and data files: a database, server functions, scheduled jobs,
      email sending, file storage, environment variables/secrets, or a
      paid service? Write a table: feature | what it needs | why | free
      option on Netlify | cost if any.
   2. Prefer the simplest option that works:
      - Rates and ratings: data files in the repo (no database needed).
      - Contact form: Netlify Forms (no database needed).
      - Monthly stale-data check: GitHub Action or Netlify scheduled
        function.
      - Save & compare: the visitor's browser (localStorage), no accounts.
   3. Only suggest a database (e.g. Netlify Blobs, Supabase or Neon) if a
      feature really needs one, e.g. "email me when FD rates change" alerts
      or storing user accounts. Explain what it would add and what it would
      cost, then STOP and ask me before adding any database, paid service,
      API key, secret or new environment variable. Don't create any
      accounts.
   4. List any settings I must change myself in the Netlify dashboard
      (e.g. form notifications, environment variables, scheduled
      functions), as step-by-step instructions.

B) NETLIFY FORM ON THE CONTACT PAGE (/contact)
   1. Replace or upgrade the current contact page form with a Netlify
      Form named "contact":
      - Fields: name, email (required, validated), topic (dropdown:
        "Calculator gives a wrong result", "Rate or rating is outdated",
        "Suggest a calculator", "Advertising / partnership", "Other"),
        the page it's about (optional URL), message (required,
        max 2,000 characters).
      - Spam protection: Netlify honeypot field (netlify-honeypot) and,
        if it doesn't hurt the design, Netlify's reCAPTCHA option.
      - Submit without leaving the page, then show a clear success
        message ("Thanks, we usually reply within 2 working days"; ask me
        for the right wording) and a clear error message if it fails.
      - Keyboard-accessible, labelled fields, works at 375px.
   2. Make sure Netlify can actually detect the form. With Next.js on
      Netlify, forms rendered by React aren't detected automatically: add
      a static HTML copy of the form (e.g. public/__forms.html with the
      same name and fields, as Netlify's Next.js docs describe) and submit
      to that path with a URL-encoded POST including "form-name". Check
      the current Netlify docs for the version of the Next.js runtime this
      repo uses. If the site isn't Next.js, use the right method for its
      framework.
   3. Add "Report a wrong rate" and "Report a problem with this
      calculator" links on rates pages, ranking pages and calculator pages.
      They open /contact with the topic and page URL already filled in
      (query string).
   4. Update the Privacy Policy page: say what the contact form collects,
      that it's stored by Netlify, and what it's used for.
   5. Test it: build locally, check that the form appears in the static
      HTML, and give me the steps to test it on a Netlify deploy preview
      and to turn on email notifications for new submissions (Netlify
      dashboard → Forms → Form notifications).

==========================================================================
WHEN YOU FINISH EACH PHASE
==========================================================================
- Run the build, lint and tests, and report the exact results.
- Give me a short list of what changed and any item you skipped, with the
  reason (including "already done").
- Phase 5: give the bank-by-bank verification table (bank | rates found |
  source URL | verified yes / could not load).
- Phase 6: show the ₹10 lakh worked example's output with the real rates
  data, and the life/health cover suggestion for one sample family.
- Phase 7: give the ratings table (bank/company | agency | instrument |
  rating | date | source URL | found yes / not available).
- Phase 8: list which items were already done and which you built.
- Phase 9: the backend-needs table first (and wait for my answer on
  anything that needs a database, paid service or secret), then the
  contact form test steps and the Netlify dashboard settings I need to
  change.
- Commit, and push to the branch you were told to use.
````
