# CalcMate: menu restructure + bank logos everywhere

Paste the box below into the CalcMate session. It replaces the earlier "header menus are too crowded" prompt. If you also send the homepage declutter prompt, send it together with this one.

---

````text
On a new branch off main (two commits: J then K, then one PR):

==========================================================================
PHASE J: MENU LIKE THE BEST SITES: MORE CATEGORIES, ONE CLEAN LIST EACH
==========================================================================
Today we have 6 mega-menus with sub-headings; "Loans & EMI" has ~30 links
and runs off the screen. Competitors use ~11 top-level menus, each a
single clean list of up to ~13 links, and a Banks menu listing every bank.
Do the same in our clean style.

1. Top menu (11): Loans | Credit Cards | Product EMI | Investments |
   FD & Savings | Post Office | Tax | Salary & Retirement | Insurance &
   Goals | Banks | More
2. Each dropdown (except Banks) is ONE list in one column, max 13 links,
   no sub-headings, short labels without the word "Calculator" (page
   titles keep it), no label wrapping to two lines, never taller than the
   screen, ending with "All <category> →" to the hub page. Suggested
   content (adjust so every calculator appears in exactly one menu or in
   More):
   - Loans: EMI, EMI with down payment, EMI affordability, Home loan,
     Personal loan, Car loan, Bike loan, Education loan, Gold loan,
     Business loan, Loan against property, Loan prepayment, Loan
     eligibility (+ Lowest loan rates pages via the hub)
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
     NPS annuity, EPF vs NPS / NPS vs MF (via All →)
   - Insurance & Goals: How much insurance do I need?, Term, Health,
     Life, Human life value, Insurance needs, Child education, Home down
     payment, Rent vs buy, Debt payoff
   - More: Everyday math (Percentage, Average, Gold price), Guides, Money
     tips, How we verify rates, All 107 calculators
3. BANKS menu (like competitors, up to ~38 banks, organised): a wide
   panel with every bank in our data, each with its logo and short name,
   grouped under three headings: Public sector banks | Private banks |
   Small finance banks (+ Payments bank / Post Office if present), sorted
   A–Z, in a tidy grid (3–4 columns on desktop). At the bottom: "All banks
   & rates →", "Compare FD rates →", "Highest FD rates →". The panel must
   fit the screen at 1366×768 (scroll inside the panel if needed, never
   the page).
4. Fit on one line at 1366px: logo + 11 items + search. If it doesn't
   fit, reduce spacing and font size slightly first; between 1024 and
   1365px move the lowest-priority items (Product EMI, Insurance & Goals,
   Post Office) into More automatically. Never wrap to two lines.
5. Mobile menu: the same groups as an accordion (one open at a time);
   Banks shows the logo list grouped the same way.
6. Hubs, breadcrumbs, /all-calculators and the homepage "Browse by
   category" section use the SAME groups (homepage cards: 5–6 links per
   card + "All →"), so menu, homepage and hubs always match. Build the
   menu, hubs and homepage from one shared navigation data file.
7. No URL changes, only grouping. If a hub page is renamed, merged or
   added, add 301 redirects and tests, and add new hubs to the sitemap.
8. Keep our style: same colours and fonts, no badges, emoji or coloured
   icon tiles. Keyboard and screen reader support as now (Esc closes,
   arrow keys move, focus visible).

==========================================================================
PHASE K: SHOW BANK LOGOS EVERYWHERE A BANK IS NAMED
==========================================================================
All 36 logos exist in public/banks/logos (BankLogo component), but they
only appear on /banks and the bank pages. Add them (small, ~20–24px, next
to the bank name, same height, aligned) on:
- the homepage "Rates today" tiles and "Compare bank rates" tables
- /highest-fd-interest-rates, /highest-savings-account-interest-rates,
  /lowest-home-loan-interest-rates, /lowest-personal-loan-interest-rates,
  /lowest-car-loan-interest-rates
- /fd-comparison-calculator, /savings-account-interest-rates,
  /savings-account-interest-calculator (bank dropdown/list)
- /fd-suggestion (top options, the insured split and the ladder)
- the Banks menu (Phase J) and the mobile menu
- the ratings / D-SIB sections where a bank is named
Rules:
- Use the existing BankLogo component and its fallback (initials in a
  neutral circle) for any bank or company without a logo (e.g. Bajaj
  Finance, Shriram, Mahindra Finance, PNB Housing, LIC Housing, Sundaram,
  ICICI Home Finance, Post Office, IPPB). Don't download new logos from
  the internet without asking me.
- Check every existing logo file renders correctly and isn't cropped,
  blurry or squashed at small sizes (e.g. wide logos like Karnataka Bank
  or ICICI need a wider box, not a squashed square). Fix sizing in CSS;
  report any file that looks wrong.
- Logos are decorative next to the name: alt="" (the name is already
  written), lazy-loaded below the fold, fixed width/height (no layout
  shift). Keep tables readable at 375px.
- Add one line on /banks and the methodology page: "Bank names and logos
  are trademarks of their owners and are used only to identify the bank.
  CalcMate is not affiliated with or endorsed by any bank."

When done: before/after screenshots of every dropdown and the Banks panel
at 1366px, the header at 1024px, the mobile menu at 375px, and two
ranking pages with logos (375 and 1366). Run all tests, crawl for broken
links, check page weight didn't grow much (logos lazy-loaded), and open
the PR.
````
