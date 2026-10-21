# CalcMate: Claude prompt for Phase 10 (icons only)

Paste the box below into the same session where you pasted Phases 5–9, as a new message. Best sent after Claude has finished Phase 9. If you send it earlier, it tells Claude to finish Phases 5–9 first.

---

````text
One more phase, to do LAST, only after Phases 5–9 are fully finished and
committed. If you're still working on any of them, finish those first. Same rules as before: check what already exists and
skip anything done, keep the site's clean style, run the build, lint and
tests, check at 375px and 1366px, and commit it as its own phase.

==========================================================================
PHASE 10: ICONS (consistent, quiet, one set)
==========================================================================
The site has very few icons. Add them where they help people scan and
find things, not everywhere.

1. ONE ICON SET. Check what the repo already uses (e.g. lucide-react,
   heroicons, inline SVGs). If there's one, keep it; otherwise use
   lucide-react. Import icons one by one (tree-shaken SVG). No icon
   fonts, no emoji as icons, no icons from a CDN at runtime. Same stroke
   width and size scale everywhere (e.g. 16px inline, 20px in cards,
   24px in category headers).
2. ONE ICON PER CALCULATOR AND CATEGORY. Create a single map
   (calculator slug → icon, category → icon) and use it everywhere the
   calculator appears. Suggested meanings:
   - EMI / loans: Landmark, Home (home loan), Car, Bike, GraduationCap
     (education), Gem (gold loan), CreditCard
   - Product EMI: Smartphone, Laptop, Tv, AirVent, Refrigerator,
     WashingMachine
   - Investment & savings: TrendingUp (SIP), PiggyBank (FD/RD/savings),
     Sprout (PPF/SSY), LineChart (CAGR/lumpsum)
   - Tax & salary: Receipt, FileText, Wallet, Briefcase
   - Post Office & Govt: Mail, Building2, Shield (pensions)
   - Insurance & retirement: ShieldCheck, HeartPulse, Umbrella, Armchair
   - Math: Percent, Calculator, Sigma
   - FD suggestion: Lightbulb; rankings: ArrowUpWideNarrow /
     ArrowDownWideNarrow; ratings: BadgeCheck
   Use whatever is closest in the chosen set, and check each icon exists.
3. WHERE TO ADD THEM (keep it small; only these three places):
   - Homepage category cards (one icon per category header) and
     calculator cards on hub pages, /all-calculators, "Popular
     calculators" and related-calculator lists (small icon left of the
     title)
   - The mobile menu (and the desktop header menu, if it fits without
     crowding)
   - Action buttons and key notes: Download (CSV), Printer, Share/Link,
     Bookmark (save), Copy; Info, AlertTriangle ("may be outdated"),
     ShieldCheck ("Insured by DICGC up to ₹5 lakh"), ShieldOff ("Not
     covered by deposit insurance")
   DON'T add icons anywhere else: not on result cards, the trust line,
   headings, FAQ questions, table rows or article text.
4. STYLE: icons in the brand teal or the current grey text colours, on
   no background or a very light brand-50 tint. ONE colour scheme for
   all of them: no multi-coloured icon tiles, no gradients, no shadows.
5. ACCESSIBILITY AND SPEED: decorative icons get aria-hidden="true";
   icon-only buttons get an aria-label. Icons must not push text or cause
   layout shift (fixed width/height). Check that the JavaScript bundle
   size doesn't grow noticeably; report the before/after size.
6. Show me before/after screenshots of the homepage, one hub page and
   one calculator page at 375px and 1366px.

When done: list what changed, report the bundle size before/after, show
the screenshots, commit, and push to the same branch.
````
