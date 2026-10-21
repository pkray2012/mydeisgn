# CalcMate: next phases (A–F)

Paste the box below into the CalcMate session working on branch `claude/cool-pasteur-5p6jp3`. It replaces the earlier "redirects + newsletter" reply; that reply is now Phases A and D here.

---

````text
Continue on branch claude/cool-pasteur-5p6jp3. Work phase by phase in the
order below (A, B, C, D, E, F). For each phase: build it, run the build, lint,
unit tests and browser tests, check every changed page at 375px and
1366px, commit it as its own commit with the phase name, push, and give me
a short report before starting the next phase.

Rules for every phase (same as before):
- Never invent rates, fees, limits or rules. Use official sources only
  (the bank's own site or rate PDF, rbi.org.in, incometax.gov.in), store
  the source URL, effective date and "last verified" date, and list
  anything you couldn't verify.
- Keep the site's current clean style.
- Every new page gets: one H1, a title of 50–60 characters, a meta
  description, a canonical URL, breadcrumbs, FAQ, related links, and an
  entry in the sitemap and the right hub page.

==========================================================================
PHASE A: SAFETY REDIRECTS + CLEANUP (do this first)
==========================================================================
1. These 8 bank calculator pages were removed with no redirect and will
   404 (they're live today, noindex). Add permanent 301 redirects in both
   netlify.toml and public/_redirects, and extend the redirect test:
   /banks/au-small-finance-bank/car-loan-calculator      -> /car-loan-emi-calculator
   /banks/au-small-finance-bank/home-loan-calculator     -> /home-loan-emi-calculator
   /banks/equitas-small-finance-bank/car-loan-calculator -> /car-loan-emi-calculator
   /banks/kotak-mahindra-bank/car-loan-calculator        -> /car-loan-emi-calculator
   /banks/canara-bank/gold-loan-calculator               -> /gold-loan-emi-calculator
   /banks/punjab-national-bank/gold-loan-calculator      -> /gold-loan-emi-calculator
   /banks/union-bank-of-india/gold-loan-calculator       -> /gold-loan-emi-calculator
   /banks/ujjivan-small-finance-bank/personal-loan-calculator -> /personal-loan-emi-calculator
2. Make the redirects data-driven: generate the redirect list for removed
   bank calculator pages from one list in the code, so a page that comes
   back in Phase B automatically loses its redirect (a forced 301 would
   otherwise hide the rebuilt page). Add a test that no page in the
   sitemap is also redirected.
3. Remove the leftover typed rates and "verified" wording from the bank
   prose in src/data/banks/registry.ts (it's stripped at build time, but
   shouldn't stay in the source).
4. For Bank of Baroda, use ICRA's last formal rating action date
   (25 Feb 2026), not the 13 Jul 2026 update.

==========================================================================
PHASE B: RE-VERIFY AND REBUILD THE REMOVED BANK PAGES
==========================================================================
27 bank calculator pages were removed because their rates weren't
verified (79 -> 52 pages):
- RD calculators (13 banks, including SBI, HDFC, ICICI, Axis)
- Used-car loan (5, including SBI, HDFC, ICICI) and two-wheeler loan
  (HDFC)
- Car loan: AU SFB, Equitas SFB, Kotak; home loan: AU SFB; gold loan:
  Canara, PNB, Union Bank; personal loan: Ujjivan SFB
Get the exact old list from git (compare main with this branch) and:
1. For each page, try to verify the current rate from the bank's OWN
   website or rate PDF (try the old address and the new <name>.bank.in
   address). For gold loans, use only a rate the bank publishes; don't
   work it out from a benchmark plus spread.
2. If verified: add the rate to the verified data files (with source
   URL, effective date and last-verified date), bring the page back using
   the current bank-page template, and remove it from the redirect list.
   Pages for SBI, HDFC Bank, ICICI Bank and Axis Bank that come back go
   back into the sitemap and stay indexable, as before; keep the others
   noindex like the rest of the bank calculator pages.
3. If not verified: keep the redirect, and list it for me.
4. Where the verified data now has a rate for a bank and product that
   never had a page (e.g. RD or FD for more banks), suggest those pages in
   your report; don't build them yet.
5. The banks that couldn't be loaded before (Bank of India, Bandhan, IDBI,
   South Indian Bank, Indian Bank loan rates, PNB Housing FD rates, India
   Post, IPPB, MOSPI): try again. If they still fail, list them; I'll
   upload their PDFs.
6. Report: a table of page | verified (yes/no) | rate | source URL | now
   live or still redirected.

==========================================================================
PHASE C: 5 NEW CREDIT CARD CALCULATORS
==========================================================================
The site has only the Credit Card EMI calculator. Add these five, as
general calculators (not tied to one card or bank), in the Credit Cards
group of the Loan & EMI category and on its hub page:

1. /credit-card-interest-calculator: "What happens if I pay only the
   minimum due?"
   Inputs: statement balance, annual (or monthly) interest rate, minimum
   due % (and minimum ₹ amount), new spends per month, payment each month.
   Show: interest charged each month, GST on the interest, months to clear
   the balance, total interest + GST paid, and how losing the interest-free
   period on new purchases changes it. Month-by-month table, chart, CSV.
   Explain the minimum-due trap in plain words.
2. /credit-card-payoff-calculator: "How fast can I clear my card debt?"
   Two modes: (a) I can pay ₹X a month -> how many months and how much
   interest; (b) I want to be debt-free in N months -> how much per month.
   Support several cards at once with avalanche (highest rate first) vs
   snowball (smallest balance first), showing total interest for each
   method. Link to Credit Card EMI, Personal Loan EMI and the existing Debt
   Payoff calculator (compare: is converting to EMI or a personal loan
   cheaper?).
3. /credit-card-cash-withdrawal-calculator: "What does an ATM cash
   withdrawal on my credit card cost?"
   Inputs: amount, cash advance fee % and minimum ₹ fee, interest rate,
   days until repaid. Show: fee, interest from day one (no interest-free
   period on cash), GST on fee and interest, total cost and effective
   annual cost.
4. /credit-card-forex-markup-calculator: "What does spending abroad (or
   on foreign websites) cost on my card?"
   Inputs: amount in foreign currency, exchange rate (user enters it; don't
   fetch live rates), forex markup %, optional dynamic currency conversion
   markup. Show: markup, GST on markup, total in ₹ and the effective
   exchange rate; compare with a 0% markup card.
5. /credit-card-rewards-calculator: "Is this card worth its annual fee?"
   Inputs: monthly spend by category (online, groceries, fuel, travel,
   other), reward rate or points per ₹100 per category, value of one point
   in ₹, annual fee, fee-waiver spend threshold, and any caps. Show:
   yearly rewards value, whether the fee is waived, net yearly value, and
   the spend needed to break even.
Optional 6th, only if time allows: /credit-utilization-calculator (limit,
balances across cards, % used, and what payment brings it under 30%),
explaining that this is a common guideline, not a credit-bureau rule.

For all of them:
- No bank- or card-specific numbers unless verified from the bank's own
  site. Default inputs are typical example values, clearly marked
  "example; enter your card's figures from your statement or the bank's
  schedule of charges".
- GST at 18% on interest and fees: confirm the current rate first.
- Check RBI's Master Direction on credit cards (rbi.org.in) for rules
  that affect the maths or the text (e.g. how minimum due, late fees and
  interest work) and cite it in "Key assumptions". Don't guess rules.
- Each has the one-line answer box, key assumptions, formula, worked
  example, FAQ, shareable link, save & compare and CSV, like the other
  calculators.
- Unit tests for each calculation, including edge cases (payment smaller
  than the monthly interest, so the debt never clears: show a clear
  warning instead of an endless table).
- Add two homepage "big questions": "Paying only the minimum due on
  ₹50,000? Here's how long it takes" and "Is your credit card worth its
  annual fee?", opening the calculators already filled in.

==========================================================================
PHASE D: NEWSLETTER SIGN-UP WITH NETLIFY FORMS
==========================================================================
Same setup as the contact form:
1. A Netlify Form named "newsletter": one email field (required,
   validated), a consent checkbox ("Send me CalcMate's rate updates and
   new calculators. I can unsubscribe any time."), the Netlify honeypot
   field, and a hidden "source" field with the page it was sent from.
2. Add the static HTML copy of the form next to the contact form's copy
   so Netlify detects it, and submit with a URL-encoded POST including
   "form-name", like the contact form.
3. Place it in the site footer (all pages), in a small box at the end of
   the rates and ranking pages ("Get an email when FD rates change"), on
   the new credit card calculators, and on the blog. Small, in the
   current style, no pop-ups.
4. Success: "You're subscribed. Check your inbox for updates." Clear error
   message if it fails. Works at 375px and with the keyboard.
5. Privacy Policy: what's collected (email, page, date), that Netlify
   stores it, how to unsubscribe (reply "unsubscribe" or use the contact
   form), and that the list is never sold or shared.
6. Netlify Forms only COLLECTS emails; it can't send newsletters. Don't
   add an email-sending service, API key or paid tool. In your report,
   list free options to send the newsletter later (e.g. importing the
   Netlify Forms CSV into a free email tool) and what each needs.
7. Give me the Netlify dashboard steps: check the "newsletter" form
   appears, turn on email notifications, and export subscribers as CSV.

==========================================================================
PHASE E: FINAL PRE-LAUNCH CHECK (no new features)
==========================================================================
1. Run every test and a full static build. Crawl the built site and
   report: broken internal links, pages missing from the sitemap, pages in
   the sitemap that redirect or are noindex, duplicate titles or
   descriptions, and any page wider than 375px.
2. Check that every rate, rating and limit shown on the site comes from
   the data files (no typed numbers in page text).
3. Run npm run check:savings-rates and the ratings check; list anything
   stale.
4. Give me a short launch checklist: what to test on the Netlify deploy
   preview (calculators, both forms, redirects, a few bank pages), what
   to set in the Netlify dashboard, and what to watch in Google Search
   Console in the first two weeks after going live.

==========================================================================
PHASE F: FINAL AUDIT WITH SUBAGENTS (after A–E are all committed)
==========================================================================
I'll deploy after this, so be thorough. Use subagents (the Agent tool)
running in parallel, each with ONE area, a clear scope and read-only
access to the built site and code. Each returns findings with: severity
(blocker / high / medium / low), page or file, what's wrong, how to
reproduce, and the suggested fix. Run these audits:

1. CALCULATION ACCURACY: for every calculator (about 100), check the
   default example against an independent hand calculation of the
   formula, plus edge cases (0%, very large amounts, shortest and longest
   tenure, empty or invalid input). Check that the answer box, result
   cards, tables, charts and CSV all show the same numbers.
2. DATA AND SOURCES: every rate, rating, limit and tax figure shown comes
   from the data files, has a source URL and date, and matches its
   source; no typed numbers in page text; stale markers work; company FDs
   are always labelled "not covered by deposit insurance" and never in
   the insured split.
3. TAX AND RULES: TDS thresholds, 80C/80D/80TTA/80TTB, 87A, slabs, cess,
   GST on card charges, DICGC limit and RBI credit card rules match the
   current official sources, and the FY/AY labels are right everywhere.
4. SEO: titles, descriptions, canonicals, one H1, JSON-LD validity,
   sitemap vs redirects vs noindex, internal links, broken links,
   duplicate content, redirects (all old URLs resolve with one 301 hop).
5. MOBILE, ACCESSIBILITY AND DESIGN: every page at 375px and 1366px (no
   sideways scroll, nothing overlapping, tap targets), keyboard use,
   labels, contrast, focus states, consistent style and icons.
6. FORMS, PRIVACY AND SECURITY: contact and newsletter forms (Netlify
   detection, honeypot, validation, success and error states), privacy
   policy matches what's collected, no secrets or keys in the code, no
   third-party scripts beyond the known ones (AdSense, analytics),
   security headers in netlify.toml, and no user input rendered unsafely.
7. PERFORMANCE: Lighthouse (mobile) on the homepage, one calculator, one
   rates page and one bank page; JavaScript bundle size per page; images
   and fonts; layout shift from ads.

Then:
- Verify each blocker and high finding yourself before fixing it (drop
  false alarms, and say which ones you dropped).
- Fix confirmed blockers and highs. Ask me before anything that changes a
  feature or the design, or needs a paid service or secret.
- List mediums and lows without fixing them unless trivial.
- Re-run the build, all tests and the affected audits after the fixes.
- Commit as "Final audit fixes", push, and end with a clear verdict:
  READY TO DEPLOY or NOT READY (with the exact remaining blockers), plus
  the full findings table.
````
