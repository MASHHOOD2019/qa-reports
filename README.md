# LunaBill homepage QA: the numbers do not line up

I am Mashhood Ali, an independent AI evaluation and QA specialist in Karachi. I test AI products the way a buyer would read them, line by line, and I check every number against the page that makes it.

This is my check of LunaBill, lunabill.com, on 19 Nov 2026. Company website: https://lunabill.com. CEO: Suhail Parry. Founders: Suhail Parry, David Day, Yash Raj. Full report: lunabill-qa-report-v2.md in this folder. LunaBill builds AI voice agents that make insurance calls for hospital billing teams. I read the public homepage, trust page, Trust Center, pricing page, and YC page. I created no account, placed no call, and ran no pilot. The full report is lunabill-qa-report-v2.md in this folder. The page captures are in evidence: lunabill-homepage.pdf, lunabill-trust.pdf, and lunabill-trust-center.pdf.

What I tested: every headline number on the homepage, the pricing claim, the integrations list, and the SOC 2 claim, against the footnote, the pricing page, the Trust Center, and the YC page.

## Finding 1: $22.28 vs $2.00 is not like for like

Reproduction: open lunabill.com and read the Nutex section. It says "11.1x lower cost per claim ($22.28 vs $2.00)". Read the footnote under it: "Cost per claim compares Nutex's fully loaded internal benchmark with LunaBill's price per claim."

The two sides are different things. One side is a fully loaded internal cost, with staff time, overhead, and systems. The other side is LunaBill's price per claim, a vendor fee. A fully loaded cost will almost always be higher than a vendor price, so the 11.1x gap is built into the comparison.

Fix: publish one clean pair. Either fully loaded cost before vs fully loaded cost after LunaBill, or vendor price before vs LunaBill price. If the fully loaded after number is not known, say so plainly and lead with a number that is known.

## Finding 2: pricing is a share with no published rate

Reproduction: open lunabill.com and read the pricing section: "You pay when you get paid. A share of net collections recovered, with the rate set per customer." Open lunabill.com/pricing and read "What is the rate?": "It is set per customer rather than published as one number, and the team quotes it on the first call." Search both pages for a percentage, a rate range, or a worked example. There is none.

A share with no rate is not a price. A hospital CFO cannot budget, cannot compare LunaBill against a flat fee vendor, and cannot get internal approval before a sales call. The $2.00 figure in Finding 1 looks like a price, but the pricing section says the price is a share of collections.

Fix: publish a rate range, for example "typically X% to Y% of net collections recovered", or a worked example, for example "on $100,000 recovered, a typical fee is $X". Add one line under the Nutex $2.00 figure: "Nutex result, not a published rate. Your rate is set per customer."

## Finding 3: "any billing software" narrows to five named systems

Reproduction: read the YC page for LunaBill. It says "Connect to any billing software and dial claims that need to be worked." Read the homepage. It says "Plugs into the systems you already run. Epic, Cerner, GoRev, Waystar, Availity. Pilot in 48 hours." Search the homepage for any other named billing system. There is none. Note: the exact phrase "Connects to any billing software" does not appear on the homepage. It is on the YC page only.

"Any" is absolute. A team on a sixth system, for example Athenahealth or eClinicalWorks, cannot tell from the page whether they are covered, or whether the 48 hour pilot applies to them.

Fix: change the line to "Plugs into Epic, Cerner, GoRev, Waystar, and Availity" and add one line for other systems, for example "Other EHRs and clearinghouses on request, pilot time confirmed before you start." If a sixth system is supported, add it to the list with the same prominence as the five named ones.

## Finding 4: SOC 2 Type II on the homepage names no auditor

Reproduction: read the homepage FAQ "Is LunaBill HIPAA compliant and secure?" It says LunaBill is SOC 2 Type II, with a public trust center at trust.lunabill.com. Search the homepage and lunabill.com/trust for an auditor name. There is none. The Trust Center does give a date and scope: "Independent SOC 2 Type 2 examination (Security) covering Dec 22, 2025 to Mar 22, 2026. Unqualified opinion, no exceptions noted." and "SOC 2 Type 2 (Security) report issued July 2026." Its FAQ says only "from an independent CPA firm". The auditor firm is still not named anywhere I checked.

A hospital security review will ask for the auditor, the period, and the scope. The homepage forces them to ask instead of answering.

Fix: add the auditor name next to the SOC 2 Type II line on the homepage and trust page, and bring the Trust Center date and scope forward too: Dec 22 2025 to Mar 22 2026, Security, report issued July 2026.

## Finding 5: the headline numbers disagree between pages

Reproduction: homepage says "$32M+ of AR worked in 30 days" at Nutex Health, and "90,000+ payer interactions / month, portfolio wide". The YC launch post says "automated 60,000+ insurance calls, saving billing teams over 20,000 hours and recovering $30M+ in payments". The same YC page's main description says "We have automated 50,000+ calls", so the YC page disagrees with itself by 10,000 calls.

Worked AR, recovered payments, calls automated, and payer interactions are four different measures. $32M+ worked in 30 days at one customer sits next to $30M+ recovered in total across all customers, and 60,000+ calls in total sits next to 90,000+ interactions in one month. A careful reader cannot tell which number is the company total and which is one customer, or which period each number covers.

Fix: label every headline number the same way on both pages, with measure, scope, and period, for example "$32M+ AR worked, Nutex Health, first 30 days" and "$30M+ recovered, all customers, to date". Use one term consistently. Calls, interactions, and follow-ups are not interchangeable.

## Note on code

The report includes one proposed check script for headline ratios. It is marked rebuilt, not tested against LunaBill's repo. I did not present it as tested code.

## What this work is

I do this check for AI and software products: read the public pages, test the claims, and write down each finding with reproduction steps and a fix. I work on contract, remote, in the evenings Pakistan time. I can start with a 48-hour trial, and the client keeps the report either way.

GitHub: https://github.com/MASHHOOD2019
LinkedIn: https://www.linkedin.com/in/mashhood-ali-514862296
Email: mashhoodalisahito95@gmail.com
