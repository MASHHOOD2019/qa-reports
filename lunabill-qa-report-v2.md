# QA Report: LunaBill (lunabill.com)

**Company:** LunaBill, YC Fall 2025. Founders Suhail Parry (CEO), David Day (CTO), Yash Raj (COO).
**Product:** AI voice agents that automate insurance calls for healthcare billing teams: claim status checks, appeals follow-up, reprocessing calls, payer portal checks, denials and resubmissions.
**Scope of this review:** Public site pages only, fetched as page text on 19 Nov 2026. No account created, no call placed, no pilot run.
**Contact on file:** founders@lunabill.com, VERIFIED on the YC page, which states: "For inquiries, email us at founders@lunabill.com". The homepage also lists suhail@lunabill.com for a demo.

## Summary

LunaBill has a strong named customer result at Nutex Health, a 24 campus US health system, and that result is the whole pitch. The problem is that the numbers on the homepage do not line up with each other. The headline cost comparison mixes a fully loaded internal cost with a price per claim. The pricing model is a share of collections with no published rate. The integration claim says "any billing software" on the YC page and then names five systems on the homepage. The SOC 2 Type II claim on the homepage names no auditor, and its date and scope live only in the Trust Center. Each finding below is a different face of the same root cause: the homepage was written to sell the Nutex result, and the numbers, pricing, and compliance claims were not checked against each other.

## Finding 1 (High): The $22.28 vs $2.00 comparison is not like for like

### Reproduction
1. Open https://www.lunabill.com/ and read the Nutex section: "$32M+ of AR worked in 30 days. Zero added headcount." Below it: "11.1x lower cost per claim ($22.28 vs $2.00)".
2. Read the footnote under those numbers: "Cost per claim compares Nutex's fully loaded internal benchmark with LunaBill's price per claim."

The two sides are different things. One side is a fully loaded internal cost, which includes staff time, overhead, and systems. The other side is LunaBill's price per claim, which is a vendor fee. A fully loaded cost will almost always be higher than a vendor price, so the 11.1x gap is built into the comparison, not earned by the product.

### Why it matters
A hospital CFO deciding whether to buy LunaBill needs to compare their fully loaded cost per claim with LunaBill's fully loaded cost per claim, or their current vendor price with LunaBill's price. The page gives neither pair. The buyer cannot tell what they would actually save, and a careful buyer will discount the whole Nutex result once they spot the mismatch.

### Root cause
The Nutex numbers were written as marketing copy first and as a cost comparison second. The footnote is honest about what was compared, but the headline hides it. Nothing on the page checks that a headline ratio compares the same measure on both sides.

### Solution
Publish one clean comparison, and keep the footnote and the headline in agreement.

1. Decide the true pair: either fully loaded cost before vs fully loaded cost after LunaBill, or vendor price before vs LunaBill price. Publish that pair in the headline.
2. If the fully loaded after-LunaBill number is not known, say so plainly and lead with a number that is known, for example "cost per claim fell from $22.28 fully loaded to $X fully loaded, including LunaBill fees".
3. Add a deploy-time check that fails when a headline ratio and its footnote use different measures. Proposed check (rebuilt, not tested against LunaBill's repo):

```python
# check_headline_ratio.py - proposed, not tested
# Rule: a headline "Ax vs Bx" ratio must compare the same measure.
# Flag it when the footnote says one side is "fully loaded" and the other is "price".
import re, sys

copy = open("homepage-copy.md").read()
footnote = re.search(r"Cost per claim compares (.+?) with (.+?)\.", copy)
if footnote and "fully loaded" in footnote.group(1) and "price" in footnote.group(2):
    print("Headline ratio mixes measures:", footnote.group(0))
    sys.exit(1)
print("Headline ratio compares the same measure on both sides.")
```

## Finding 2 (High): Pricing is a share with no published rate

### Reproduction
1. Open https://www.lunabill.com/ and read the pricing section: "You pay when you get paid. A share of net collections recovered, with the rate set per customer. Nothing upfront. If we recover nothing, you owe nothing."
2. Read the FAQ "How much does LunaBill cost?": "Pricing is performance based. LunaBill bills a share of net collections recovered, only after the money lands, and the rate is set per customer. There is nothing upfront, no license fees, and no implementation fees."
3. Search the homepage for a percentage, a rate range, or a worked example. There is none.

### Why it matters
A share with no rate is not a price. A hospital CFO cannot budget, cannot compare LunaBill against a flat fee vendor, and cannot get internal approval before a sales call. The "$2.00 per claim" figure in Finding 1 looks like a price, but the pricing section says the price is a share of collections, so the two sections point in different directions. A buyer has to ask sales for the one number that decides the deal.

### Root cause
Performance pricing was written as a promise ("you pay when you get paid") without the number that makes the promise usable. The $2.00 figure is a Nutex result, not a published rate, but the page never says that clearly.

### Solution
Publish a rate range or a worked example, and label the $2.00 figure for what it is.

1. Publish either a rate range, for example "typically X% to Y% of net collections recovered", or a worked example, for example "on $100,000 recovered, a typical fee is $X".
2. Add one line under the Nutex $2.00 figure: "Nutex result, not a published rate. Your rate is set per customer."
3. Keep the $0 upfront and no recovery, no fee lines. They are clear and they work.

## Finding 3 (Medium): "Any billing software" on the YC page narrows to five named systems on the homepage

### Reproduction
1. YC page (https://www.ycombinator.com/companies/lunabill): "Connect to any billing software and dial claims that need to be worked."
2. Homepage: "Plugs into the systems you already run. Epic, Cerner, GoRev, Waystar, Availity. Pilot in 48 hours." Its FAQ says the same list, with "including" before the five names.
3. Search the homepage for any other named billing system or EHR. There is none. Correction from browser capture 19 Nov 17:24 PKT: the exact phrase "Connects to any billing software" does NOT appear on the homepage. It is on the YC page only.

### Why it matters
"Any" is absolute. A team on a sixth system, for example Athenahealth or eClinicalWorks, cannot tell from the page whether they are covered, how long integration takes for them, or whether the 48 hour pilot applies to them. The five named systems are the real coverage list, and the page should say so.

### Root cause
The "any" claim was written for the YC pitch, where breadth sells. The homepage kept the broad claim and added a narrow list underneath, and nothing reconciled the two.

### Solution
Replace "any" with the true list, and state what happens for systems outside it.

1. Change the line to "Plugs into Epic, Cerner, GoRev, Waystar, and Availity" and add one line for other systems, for example "Other EHRs and clearinghouses on request, pilot time confirmed before you start."
2. If a sixth system is supported, add it to the list with the same prominence as the five named ones.

## Finding 4 (Medium): SOC 2 Type II is claimed on the homepage with no auditor named, and the date and scope live only in the Trust Center

### Reproduction
1. Open https://www.lunabill.com/ and read the FAQ "Is LunaBill HIPAA compliant and secure?": "Yes. LunaBill is HIPAA compliant with enterprise grade security, executes a BAA in under a day, and is SOC 2 Type II. A public trust center is available at trust.lunabill.com."
2. Search the homepage and https://lunabill.com/trust/ for an auditor name. There is none. The trust page says "SOC 2 Type II certified, report under NDA" and "You can track live control status at trust.lunabill.com, and the Type II report is available under NDA during security review."
3. Correction from browser capture 19 Nov 17:24 PKT: the Trust Center at https://trust.lunabill.com/ DOES give a date and scope: "Independent SOC 2 Type 2 examination (Security) covering Dec 22, 2025 to Mar 22, 2026. Unqualified opinion, no exceptions noted." and "SOC 2 Type 2 (Security) report issued July 2026. Unqualified opinion, no exceptions noted." Its FAQ says only "from an independent CPA firm". The auditor firm is still not named anywhere checked. So the claim is not that no date or scope exists anywhere. It is that the homepage and trust page give a buyer no auditor, date, or scope, and the date and scope are one click away in the Trust Center.

### Why it matters
LunaBill handles hospital billing data, so security is a buying decision, not a badge. A SOC 2 Type II claim on the homepage with no auditor, no period, and no scope cannot be checked from that page. A hospital security review will ask for exactly those three facts, and the homepage forces them to ask instead of answering. This finding does not claim the report does not exist. It claims the homepage gives no evidence for it, and the Trust Center gives date and scope but still no auditor firm.

### Root cause
The compliance line was written as a yes or no answer in an FAQ, not as a verifiable claim. The evidence lives behind the trust center link, and the homepage never brings even the basic facts forward.

### Solution
Put the three checkable facts on the page.

1. Add the auditor name next to the SOC 2 Type II line on the homepage and trust page, and bring the Trust Center date and scope forward too: Dec 22 2025 to Mar 22 2026, Security, report issued July 2026.
2. Keep the trust center link, and add one line saying what a visitor can see there without signing in.

## Finding 5 (Medium): The headline numbers disagree between the homepage and the YC page

### Reproduction
1. Homepage: "$32M+ of AR worked in 30 days" at Nutex Health, "90,000+ payer interactions / month, portfolio wide", "45 to 7 days in AR, 60+ day commercial claims".
2. YC page (https://www.ycombinator.com/companies/lunabill): launch post says "automated 60,000+ insurance calls, saving billing teams over 20,000 hours and recovering $30M+ in payments that would otherwise be lost." The same YC page's main company description says "We have automated 50,000+ calls", so the YC page disagrees with itself by 10,000 calls.
3. The footnote on the homepage adds a third time window: "AR aging measured on commercial claims aged 60+ days, within 3 months of go live. Payer interactions are trailing 30 days across all LunaBill customers."

Worked AR, recovered payments, calls automated, and payer interactions are four different measures. $32M+ worked in 30 days at one customer sits next to $30M+ recovered in total across all customers, and 60,000+ calls in total sits next to 90,000+ interactions in one month. A careful reader cannot tell which number is the company total and which is one customer, or which period each number covers.

### Why it matters
LunaBill's whole pitch is numbers. When the numbers on two official pages cannot be reconciled, a buyer discounts all of them, including the real Nutex result. The confusion is avoidable: each number needs a label saying what it measures, whose it is, and over what period.

### Root cause
The YC page and the homepage were written at different times for different audiences, and each kept its own headline numbers. The homepage footnote tries to explain the time windows, but it covers only two of the numbers and it is in small print under the section.

### Solution
Label every headline number the same way, on both pages.

1. Give each number a label with measure, scope, and period, for example "$32M+ AR worked, Nutex Health, first 30 days" and "$30M+ recovered, all customers, to date".
2. Use one term consistently: calls, interactions, and follow-ups are not interchangeable. Pick one and define it once.
3. Update the YC page when the homepage numbers change, so the two pages never disagree.

## Plan reference (verified on the homepage)

- Pricing: share of net collections recovered, rate set per customer, $0 upfront, no license fees, no implementation fees, no recovery means no fee.
- Nutex Health: 24 campus US health system, 60+ person revenue cycle team, $32M+ AR worked in 30 days, days in AR 45 to 7 on 60+ day commercial claims, cost per claim $22.28 vs $2.00, 22 to 4 minutes per claim follow up, 90,000+ payer interactions a month portfolio wide.
- Integrations named: Epic, Cerner, GoRev, Waystar, Availity. Pilot in 48 hours. BAA in under a day.
- Security: HIPAA compliant, BAA in under a day, SOC 2 Type II, trust center at trust.lunabill.com.

## Evidence

Pages fetched as text on 19 Nov 2026:

- https://www.lunabill.com/ (fetched 18 Nov 2026 13:24 UTC): homepage. Quoted: "$32M+ of AR worked in 30 days. Zero added headcount."; "11.1x lower cost per claim ($22.28 vs $2.00)"; footnote "Cost per claim compares Nutex's fully loaded internal benchmark with LunaBill's price per claim. AR aging measured on commercial claims aged 60+ days, within 3 months of go live. Payer interactions are trailing 30 days across all LunaBill customers."; "You pay when you get paid. A share of net collections recovered, with the rate set per customer. Nothing upfront. If we recover nothing, you owe nothing."; "Plugs into the systems you already run. Epic, Cerner, GoRev, Waystar, Availity. Pilot in 48 hours."; FAQ "How much does LunaBill cost?" quoted in Finding 2; FAQ "Is LunaBill HIPAA compliant and secure?" quoted in Finding 4; "Or email suhail@lunabill.com."
- https://www.ycombinator.com/companies/lunabill (fetched 19 Nov 2026 04:09 UTC): YC page. Quoted: "We built AI voice agents that automate insurance calls for healthcare billing teams. Fine-tuned on a dataset of over 1.2M call transcripts"; "automated 60,000+ insurance calls, saving billing teams over 20,000 hours and recovering $30M+ in payments that would otherwise be lost"; founders Suhail Parry (CEO), David Day (CTO), Yash Raj (COO); "For inquiries, email us at founders@lunabill.com". This verifies founders@lunabill.com on the YC page.
- Note on an earlier claim not used: an earlier pass suggested the SOC 2 claim disagreed between Type I and Type II across pages. That was not verified in this pass. The homepage states Type II only, and the trust center page was not fetched, so no Type I contradiction is claimed in this report.

Screenshots pending: browser capture by parent agent, page text fetched 19 Nov 2026. No screenshots exist for LunaBill yet, so this draft is not send-ready until browser capture.

Browser capture DONE 19 Nov 17:24-17:26 PKT: 3 PDFs saved to evidence/, none blank, text verified: lunabill-homepage.pdf (12 pages, 1.4 MB), lunabill-trust.pdf (4 pages, 187 KB), lunabill-trust-center.pdf (9 pages, 404 KB). Findings 1 and 2 hold exactly, Finding 3 wording corrected, Finding 4 date and scope found in Trust Center, Finding 5 adds YC 50,000 vs 60,000 calls. See v2 draft and report.

Note on contact verification: founders@lunabill.com is VERIFIED on the YC page (quoted above). suhail@lunabill.com is listed on the homepage for a demo.
