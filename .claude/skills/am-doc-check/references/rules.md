# Rules for outbound document checks

Maintained by: [owner - to be named]. Update this file and `scripts/check_doc.py` together when a template changes. Last reviewed: [date].

## Numbers

- **Contract number:** `[CLIENT][proposal date YYMMDD][seq 2 digits][optional suffix, e.g. PT]/[contract date YYMMDD][seq 2 digits]` - two parts. Examples: `ODDS25052710/25052710`, `PEANUDS26091810PT/26101210`.
  - The first part equals the Proposal number.
  - The second part's date equals the contract date stated in the document.
- **Order No.:** contract number + `/N` (for example `.../26042711/3`). The `/N` suffix appears only in the Order No., never in a reference to the contract.
- An appendix or annex quotes the contract number exactly as it appears in the contract.

## 7Security details (identical everywhere)

- Registration number `561824m` (no "FN", no space), VAT `ATU77298229`, address `Hiessgasse 12/3, 1030 Vienna, Austria`.
- The appendix and annex preambles include all three, as in the templates.
- Signatory for 7Security: Tanya Klyasova, COO.

## Parties

- The party named in the signature block must be the client named in the preamble (same legal entity).
- Bulgarian-client templates add 7Security Ltd as subcontractor (it invoices and is paid) and use Bulgarian law and courts. Universal templates use Austrian law and courts, with only 7Security GmbH. Mixing the two is an error.

## Order forms

- Line items minus discounts equal the order total; yearly totals add up to the contract total.
- Order forms are signed by the client only. Do **not** flag a missing 7Security signature block, unless the client has asked for one.

## Payment terms by template (as in the current templates)

| Template | Payment | Invoice due |
|---|---|---|
| PCI monthly | Monthly, invoiced by the 5th | 10 days from invoice date |
| PCI 1-year | 50% advance, 50% on draft deliverables | Advance 10 days from receipt; final on receipt |
| PCI 3-year | As 1-year, with yearly table | [being fixed in template] |
| ASV | 100% before start | [being fixed in template] |
| Pentest | 100% before start | On receipt |
| PSD2 SCA, ISO recertification, SWIFT | 50% before, rest after each service | 10 days from issuance |
| ISO implementation, SOC2 | 40% / 40% / 20% | 10 days from issuance |

Effective date wording differs by template and is intentional: "parties sign" (most), "Client signs" (ASV, pentest), "last Party signs" (Bulgarian monthly), "kick-off" (universal monthly, being aligned).
The pentest scope, authorization and retest terms are set in the Proposal, not the contract.

## Accepted - do not flag

- Footers: the footer reads "CONTRACT Ver. 1.0" in every template, including appendices. Do not check footers.
- Cyrillic look-alike letters in text.
- "Agreement" in place of "Contract" in the 3-year template.
- The blank "PCI ___" in the 1-year template is a placeholder the AM must fill in: **do** flag it if it is still blank.
- No subscription start date on ASV order forms (the template has none).

## Offers (commercial proposals)

- Offers are built from the service-specific offer templates (cover: service name, client, issue date, version, classification, code; footer "COMMERCIAL PROPOSAL Ver. 1.0").
- Everything the template asks to fill in must be gone: client name on the cover and in the intro, registration/VAT numbers (not "...."), "2 sentences about the Client", "(alter as needed)", "(if applicable)", the issue date, the code, empty "€" price fields, "....".
- Totals: services minus discounts equal the TOTAL COST.
- Payment terms in the offer match the contract template for that service (see table above). Validity: PCI (DSS, 3DS, PIN), PSD2 and SWIFT offers stand for 60 days; ISO, SOC2 and ISO recertification offers for 30 days; this is intended. An offer past its validity is not sent as is.
- PCI offers: 50% prepayment before the project starts, the remaining 50% invoiced on delivery of the draft deliverables; prepayment invoice due within 10 calendar days **from the issuance date** (the 1-year and 3-year contract templates are to be aligned to say issuance, not receipt; the script flags 'from its receipt'), final invoice due on receipt.
- PCI DSS offers list reporting price options (L1, ML2, SPL2 V4.0) and a discount shown as an either/or amount ('1/2,900'): the AM keeps only the option that applies and picks one discount amount. The either/or amount is intentional in the template and the script flags it until it is resolved. The 3DS offer has its own reporting prices (ROC and AOC), not the PCI DSS levels. The year labels in the 3-year table (1st Year 2026 ...) must match the offer year. Template instructions ((NOTE: ...), (OPTIONAL)) must be gone.
- The main PCI DSS offer also has a Monthly Payments Option (€1,200 per month, no minimum duration, 3-month notice, paid by bank transfer); the 3-year and 1-year tables show the yearly prices. The offer says 50/50 payment only for the 3-year option.
- The Bulgarian PCI DSS offer intentionally omits the sentence about travel costs when remote assessment is not possible.
- Bulgarian-client offers name 7Security Ltd as the entity that invoices and is paid.
- Text from another service's offer is an error, for example ISMS wording in a SOC2 or SWIFT offer. The script flags this. The "About Us" and team-credentials sentences are the same in every offer and are ignored.
- "QSA company" is PCI wording; it should not appear in PSD2, SOC2, SWIFT or ISO offers. Use the neutral wording each offer template uses ("7Security GmbH", "a SWIFT Partner company", "assessor company" in PSD2).
- "ISO/IEC 27001:2017 / ISO/IEC 27001:2022" in the ISO implementation offer is accepted.

- The project role named in the Project Initiation text is **Project Manager (PM)**. "Customer Success Manager (CSM)" and "Service Delivery Manager (SDM)" are old wording; the script flags them.
- Reports by standard: PCI DSS and 3DS use ROC and AOC; PIN uses ROC and AOC; **SSF** uses ROV and AOV for the Secure Software Standard and ROC for the Secure SLC Standard; P2PE uses P-ROV and AOV. The SSF assessor is the "Secure Software Assessor".
- The 7Security registration number is `561824m` in every document, including the offer intro.

- Pentest offers (also for apps) stand for 30 days; the ASV offer stands for 60 days. Pentest offers: 100% payment before the test, the final report only after full payment, invoices due on receipt. ASV offers: 100% before the start, invoices due within 10 days from the issuance date.
- Offers cite PCI DSS v4.0.1 requirement numbers: external ASV scans 11.3.2, internal scans 11.3.1, segmentation testing 11.4.5/11.4.6 (v3.2.1 used 11.2.x and 11.3.4). The script flags the old numbers.

- Pentest offer total = **testing days x €900 per day**; report preparation days are not charged. The table's "TOTAL COST: € 900 per day" is the template's rate line and must become the real total. Every pentest offer keeps the note "Includes Remediation Testing for up to 4 (four) identified vulnerabilities…" after the table (the retest terms live there).

## Confirmed as intended (do not flag)

- The 1-year option in the main PCI DSS offer has no separate payment sentence; the 50/50 terms stated for the 3-year option are enough.
- "Loyal Partner Discount" and "Valuable Partner Discount" are both acceptable names.
- The either/or discount amount ("1/2,900") is deliberate; the AM must resolve it before sending (the script flags it until then).
- The "Formal bi-annual scope validation (req. 12.5.2.1)" line in the monthly option is fine as written.
- The layout of the 3-year price table (labels, rows per year) is correct as it is. Tables lose merged cells when converted to text, so never report table layout or "missing" rows from converted text alone.

## Not covered yet
- Checking an offer's code against the contract number it later becomes (needs both documents).
- Checking pricing against the price list, discounts, or clauses that differ from the template. Send those to the SDD.
- Other clients' names left in a document (needs a client list).
