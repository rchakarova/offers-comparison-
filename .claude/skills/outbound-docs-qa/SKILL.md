---
name: outbound-docs-qa
description: QA review of 7Sec outbound commercial documents before they are sent to a client or prospect - offers/proposals, contracts and MSAs, appendices, annexes, SoWs, amendments and renewals for PCI DSS assessments, penetration tests, ASV scans and related services. Checks the main document and every appendix/annex together for internal consistency (parties, dates, prices, totals, scope, asset counts, cross-references, retest terms) and for hygiene problems (leftover placeholders, text from another client, wrong template version, typos). Use this whenever the user asks to review, check, QA, proof or sanity-check an offer, contract, appendix, annex, amendment or renewal, or says a document is "ready to send", even if they don't say "QA".
---

# Outbound documents QA

Outbound documents go to clients under 7Sec's name and often become the contractual basis for the work. The mistakes that cost the most are rarely clause wording; they are mismatches between parts of the same package (an offer that says 3 endpoints and a scope annex that lists 4, a total that doesn't add up, an annex still naming the previous client). Review the package as a whole, not each file in isolation.

## Workflow

1. **Collect the package.** Identify the main document and every appendix, annex, SoW, pricing table or amendment that belongs with it. If something referenced in the main document is missing (e.g. "see Annex C"), that is itself a finding. Ask the user for the missing parts only if the review can't be meaningful without them.
2. **Extract the text faithfully.** For .docx use pandoc or python-docx and include tables, headers, footers, comments and tracked changes; leftovers often hide there. For PDFs extract text and check tables. Do not review from memory or a summary.
3. **Build a fact table** before judging anything: parties and legal names, document type and version/date, validity period, service(s), scope items and counts (hosts, endpoints, apps, environments, SAQ/ROC type), dates and durations, prices, quantities, totals, taxes/currency, payment terms, retest terms, named contacts, references to other documents/sections. Record where each fact appears.
4. **Cross-check the fact table** (see checks below), then run the hygiene pass.
5. **Report** using the format below.

## Checks

**Internal consistency** (highest value)
- Party names, addresses, registration/VAT numbers identical everywhere; client name spelled the same in body, annexes, headers/footers and file name.
- Dates are logical: issue date before validity end, start before end, renewal and amendment dates match the agreement they modify; no past dates on a new offer.
- Prices: line items x quantities = subtotals = total; currency and VAT treatment consistent; any discount applied consistently.
- Scope: asset lists, counts, environments and service types match between offer, SoW and annexes; nothing in the pricing that isn't in scope, or in scope that isn't priced.
- Terms stated in more than one place (retest policy and limits, payment terms, reporting timelines, liability, confidentiality, termination) agree with each other.
- Cross-references ("Annex B", "section 4.2") point to something that exists and says what the reference claims.
- Amendments and renewals: identify exactly what changes versus the original and verify that everything not meant to change still matches the original terms; check that the amendment states which clauses it supersedes.

**Hygiene**
- Unfilled placeholders (`[CLIENT]`, `XXX`, `TBD`, `<date>`), highlighted text, brackets.
- Text, names, prices or scope from another client or earlier engagement.
- Tracked changes, comments or hidden text still present.
- Template/version markers and branding current; page numbering, headings and numbering sequences intact; signature blocks name the right signatories.
- Typos and wording that reads as unfinished or contradictory.

## Boundaries

No reference templates or clause library have been given to this skill yet. Do not invent 7Sec policy or legal positions; flag legal-sounding terms that look unusual or inconsistent with the rest of the package as "check with owner/legal" rather than declaring them wrong. If the user supplies a template, standard clause set or checklist, use it as the baseline and diff against it. If a document concerns PCI DSS obligations, flag wording that appears to conflict with the standard (for example on penetration test retests or tester independence), citing the requirement only when you are certain of it, and otherwise mark it for confirmation.

Never edit the documents unless asked; the review is read-only.

## Output format

Start with a one-line verdict: **Ready to send**, **Send after fixes**, or **Do not send**.

Then a prioritised list:

- **Blocking** - wrong or contradictory facts, money, parties, scope, missing referenced documents, content from another client.
- **Should fix** - ambiguity, inconsistent terms that probably mean the same thing, unlogical dates.
- **Minor** - typos, formatting, style.

For each finding give: where (document and section/page/table), what is wrong (quote the conflicting text or values), and a suggested fix. Where two places disagree, show both side by side.

End with a short **Verified OK** list (what you checked and found consistent) so the user knows the coverage, and a **Could not check** list (missing documents, unreadable parts, items needing a reference you don't have).
