---
name: am-doc-check
description: Pre-send check for 7Sec account managers on outbound documents - contracts, offers/proposals, appendices and order forms, annexes (PCI DSS, ASV, pentest, PSD2, ISO 27001, SOC2, SWIFT). Catches the mistakes that normally bounce back for correction - wrong contract/order numbers, contract-number/date mismatch, wrong signing party, totals that don't add up, missing 7Sec details, left-over blanks, Bulgarian-vs-universal template mix-ups. Use this whenever an AM is about to send or hand over a contract, appendix, order form, annex or offer, or says "check this", "is this ready", "can you review this contract/appendix", even if they don't say "QA".
---

# AM document check

You are checking a document an account manager (AM) is about to send. The goal is that the common, mechanical mistakes are caught by the AM before the document reaches the SDD or the client, so nobody has to go back and forth over simple errors. Be brief and practical: the AM is not a lawyer and just wants to know what to fix.

## Steps

1. **Run the automatic checks.** The script reads PDF or Word files and checks the exact rules (numbers, dates, parties, totals, blanks):

   ```
   python3 scripts/check_doc.py <path-to-file>
   ```

   It prints findings as BLOCK (must fix before sending), FIX (should fix) and INFO (small). If it can't read the file, say so and ask for a PDF or docx.

2. **Then read the document yourself** for what a script can't judge. Check against `references/rules.md`:
   - The client's name, address and registration details are consistent everywhere, including the signature block.
   - The right template was used for the client (Bulgarian clients have their own versions with the Bulgarian subcontractor and Bulgarian law; the others are universal).
   - Payment terms match the service type (see rules).
   - Dates are logical, and the offer or Proposal reference matches the contract number.
   - Anything odd: wording that differs from the template, amounts that look unusual, services that don't match the order.

3. **Do not flag the things the rules say are accepted** (listed in `references/rules.md`). Flagging accepted things wastes the AM's time and teaches them to ignore the check.

4. **Report** in this format, in plain language:

   **Verdict:** Ready to send / Fix first (use "Fix first" if there is any BLOCK).

   **Fix before sending** - one line each: what is wrong, where, and the exact fix.
   **Should fix** - same format.
   **Small things** - short list.
   **Checked and fine** - a few words on what you verified, so the AM knows what was covered.
   **Ask the SDD** - anything involving legal wording, prices, discounts or a clause that differs from the template. The AM should not decide those alone.

## Notes

- The script's output is a starting point. If you disagree with a finding (a false alarm), say so and explain briefly, rather than repeating it.
- Offers are covered (see the Offers section of the rules). If the document is a type the rules don't cover yet, run the script anyway, say clearly what was and wasn't checked, and don't invent rules.
- Never edit the document; only report. Offer to draft corrected wording if asked.
