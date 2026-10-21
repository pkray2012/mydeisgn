# CalcMate: Money Tips newsletter + new Contact page

Paste the box below into the CalcMate session on `claude/cool-pasteur-5p6jp3`, BEFORE opening the pull request to `main`.

If you want Claude to copy the newsletter style from your pradiptaray site, first add its domain to the environment's **Allowed domains** (e.g. `pradiptaray.com` and `*.pradiptaray.com`; use the real address), or upload a screenshot of its newsletter box.

---

````text
Two changes before we deploy, on branch claude/cool-pasteur-5p6jp3. Do them
as two phases (G, then H), each its own commit. Same rules as before: keep
the clean style, official facts only, nothing invented, check 375px and
1366px, run the build, lint, unit tests and browser tests, push, and report.

==========================================================================
PHASE G: TURN THE NEWSLETTER INTO "CALCMATE MONEY TIPS"
==========================================================================
Reference: I run a similar "money tips" newsletter on my site pradiptaray
(<PUT THE REAL URL HERE>). If you can open it, match its approach (tone,
layout of the sign-up box, what it promises). If you can't open it, follow
this spec and tell me.

1. Rename and reposition the existing Netlify "newsletter" form as
   "CalcMate Money Tips": a short, practical email with money tips for
   Indian readers. Keep the same form name ("newsletter") so existing
   submissions stay in one place.
2. Sign-up box content:
   - Title: "Get CalcMate Money Tips"
   - One line: "One practical money tip, the rate changes that matter and
     new calculators: short, free, no spam."
   - Three small bullets of what they get: a money tip you can use the
     same day (saving, tax, loans, investing); FD, savings and loan rate
     changes from our verified data; new calculators and guides.
   - Fields: email (required), first name (optional), interests
     (checkboxes, optional): Saving & FDs, Loans & EMI, Tax & salary,
     Investing & SIP, Credit cards, Retirement & insurance.
   - Consent checkbox: "Send me CalcMate Money Tips. I can unsubscribe
     any time." Honeypot and hidden "source" field as now.
   - Button: "Send me money tips". Success: "You're in! Your first money
     tip is on its way soon." Keep the error message.
   - Add every new field to the static HTML copy of the form so Netlify
     detects them.
3. Make the box fit the page it's on. One shared component with a
   "topic" prop that changes the line and pre-ticks the matching interest:
   - FD, savings and rates pages: "Get an email when FD rates change, plus
     one savings tip."
   - Loan and EMI pages: "Tips to pay less interest on your loans."
   - Tax and salary pages: "Tax-saving tips before every deadline."
   - Credit card calculators: "Tips to avoid card interest and fees."
   - Blog and everywhere else: the general version.
   Small and inline, at the end of the page content. No pop-ups.
4. New page /money-tips (title "Free Money Tips Newsletter for India |
   CalcMate"):
   - What the newsletter is, how often it comes (ask me: weekly or every
     two weeks; until I answer, say "a short email, never more than once a
     week"), and what's inside.
   - "Example tips" section with 4–6 short sample tips, each linking to
     the calculator that proves it. Every number in a tip must be
     calculated with the site's own calculators and verified data, never
     typed in, and labelled "Example". For instance:
     * "Paying only the minimum due on ₹50,000 takes N months and costs
       ₹X in interest." -> credit card interest calculator
     * "Splitting ₹10 lakh across 3 banks keeps it fully insured." -> FD
       suggestion tool
     * "₹3,000 extra a month on a ₹30 lakh home loan saves ₹X." -> home
       loan prepayment
     * "In the new regime, salary up to ₹X has zero tax." -> income tax
       calculator
   - The sign-up form, a privacy line ("We never sell or share your
     email"), and how to unsubscribe.
   - Do NOT show subscriber counts, testimonials or "join 10,000 readers"
     unless I give you real numbers.
   Add /money-tips to the footer, the sitemap and the homepage (one small
   line under the trust line: "Free money tips by email →").
5. Footer: replace the plain newsletter box with the compact Money Tips
   version.
6. Update the Privacy Policy (now also first name and interests) and the
   report's Netlify steps (the form keeps its name; new fields appear
   automatically after the next deploy).

==========================================================================
PHASE H: A BETTER CONTACT PAGE WITH "WHO ARE YOU?" OPTIONS
==========================================================================
Redesign /contact so people pick who they are first, and the form adapts.

1. Top of the page:
   - H1 "Contact CalcMate" and one friendly line: "Found a wrong number,
     want to advertise, or have an idea? Pick what fits and we'll get it to
     the right place."
   - Five option cards (icon + title + one line), as a 2- or 3-column grid
     on desktop and stacked on phones:
     a) "I use CalcMate": report a wrong result, an outdated rate, suggest
        a calculator, or ask a question
     b) "Advertise with us": ads, sponsored calculators or guides
     c) "Bank / NBFC / insurer": correct your rates or share official
        rate sheets
     d) "Media & partnerships": press, quotes, collaborations, embedding
        our calculators
     e) "Something else"
   - Clicking a card selects it (radio-group behaviour, keyboard
     accessible), scrolls to the form and updates the URL
     (?type=user|advertiser|bank|media|other), so links can open the page
     with a type already chosen. The existing "Report a wrong rate" and
     "Report a problem" links open type=user with the topic and page filled
     in, as now.
2. One Netlify form ("contact", same name as now) with a hidden
   "audience" field and fields that change by type. All fields must exist
   in the static HTML copy so Netlify detects them; hide the ones that
   don't apply.
   - Everyone: name, email, message (required).
   - User: topic (wrong result / outdated rate / suggest a calculator /
     question), page URL, "what you entered and what you expected".
   - Advertiser: company, website, what you want (display ads / sponsored
     guide / sponsored calculator / newsletter sponsorship / other),
     monthly budget range (optional), start date (optional).
   - Bank / NBFC / insurer: institution name, your role, which page or
     rate, link to the official rate page or PDF (required for a rate
     correction).
   - Media & partnerships: organisation, type (press / collaboration /
     embed a calculator / other), deadline (optional).
3. Under the form, a short "Good report example" box for users, e.g.
   "Page: /fd-calculator · I entered ₹1,00,000 at 7% for 1 year,
   quarterly · Expected ₹107,186 · It showed ₹... ". Calculate the
   example with the site's own FD formula; don't type the number.
4. For advertisers, add a short panel or a separate /advertise page:
   what can be sponsored, how sponsored content is labelled, and the
   rule from the methodology page that payment never changes rankings or
   rates. Don't show traffic numbers, audience stats or prices unless I
   give them to you; use "Ask us for current audience figures".
5. A small FAQ at the bottom (3–5 questions): Is CalcMate free? Do you
   give financial advice? (no) How do you check rates? (link to the
   methodology page) How fast do you reply? (ask me for the wording; until
   then, "We read every message and reply by email when it needs an
   answer.") Can I embed a calculator? (only say yes if the site supports
   it; otherwise "Ask us").
6. Success message stays as now, but personalised by type, e.g.
   advertisers: "Thanks! We'll reply about advertising by email."
7. Spam protection as now (honeypot, reCAPTCHA if already on). Labels
   on every field, errors tied to their field, works with the keyboard
   and at 375px.
8. Update the Privacy Policy (company and role fields), and tell me in
   the report how to set different email notifications in Netlify (e.g. a
   filter by the "audience" field if Netlify supports it; otherwise one
   notification for all).

When both phases are done: re-run the full browser tests and the crawl,
then give me screenshots of /money-tips, the footer box, one calculator
page with the topic box, and /contact with each type selected (375px and
1366px). Push, and confirm the branch is still READY TO DEPLOY.
````
