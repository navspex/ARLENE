# /ebay-list [item] — eBay Listing from Spreadsheet Row

Trigger: "list item [N]" / "/ebay-list [N]" / "post item [N] to eBay"
Purpose: Conversational wrapper around the proven ebay_lister.py CLI.
Turns a terminal tool into a one-command flow with human gate before anything posts.

Repo: X:\essco\ebay-lister-with-ClaudeCode
Lister: lister/ebay_lister.py (HEAD a37ce07, dry-run verified all 9 items)
Data: X:\essco\ebay-lister-with-ClaudeCode\data\avionics_ebay_listing_summary.xlsx
Accounts: pilotronics | essco (two-account switch via --account flag)

## Prerequisites Check (run before any listing attempt)
- [ ] EB-01: Pilotronics eBay developer account approved + App ID / Dev ID / Cert ID / AUTH_TOKEN retrieved
- [ ] EB-02: .env populated with all keys + Business Policy IDs + POSTAL_CODE for each account
- [ ] EB-03: Seed spreadsheet in data\ folder
- [ ] EB-04: MPN cells cleaned (item 1 has sentence in part_number — use "Does not apply")
- [ ] EB-05: Category IDs verified in CATEGORY_MAP vs Seller Hub

If any prerequisite is unmet: list which ones and STOP. Do not attempt a verify run.

## Steps (when prerequisites are met)

1. **Identify the item** — row number from the spreadsheet (1-9).
   Ask: "Which item number?" if not specified.

2. **Identify the account** — pilotronics or essco.
   Ask: "Pilotronics or ESSCO account?" if not specified.

3. **Run dry-run first** (always):
   ```
   cd X:\essco\ebay-lister-with-ClaudeCode
   python lister/ebay_lister.py --dry-run --account [pilotronics|essco] --row [N]
   ```
   Shows the XML that would be submitted. No network call. No credentials needed.

4. **Show Dale the dry-run output:**
   ```
   DRY RUN — Item [N]: [title]
   Account: [pilotronics|essco]
   Category: [name] ([ID])
   Price: $[price] | Condition: Used
   MPN: [value] | Shipping: [policy]

   XML looks right? Say GO to run Verify, or STOP to edit.
   ```

5. **On Dale GO — run Verify** (validates with eBay, returns fees, posts NOTHING):
   ```
   python lister/ebay_lister.py --account [account] --row [N]
   ```
   (Default mode = VerifyAddFixedPriceItem)

6. **Show verify result:**
   ```
   VERIFY PASSED — Item [N]: [title]
   Estimated fees: $[amount]
   eBay says: ready to publish.

   Say PUBLISH to go live, or STOP.
   ```

7. **On Dale PUBLISH — run live listing:**
   ```
   python lister/ebay_lister.py --publish --account [account] --row [N]
   ```
   Returns ItemID from eBay. Log it.

8. **Report:**
   ```
   LISTED: [title]
   ItemID: [eBay ID]
   Account: [pilotronics|essco]
   URL: https://www.ebay.com/itm/[ItemID]

   ▶ Next item? Or done for today?
   ```

## Hard Rules
- NEVER skip dry-run → verify → publish sequence. Three gates, always.
- NEVER publish without Dale explicitly saying "PUBLISH" (not "go" or "yes").
- Return policy MUST be 30-day free returns (P&A eBay mandate since Aug 2025).
  Error code 21919144 = rate limit guard (already in lister). Error 2190443 = return policy.
- Item 6 (Stormscope) title clips at 80 chars — verify title length before verify run.
- Photos: not yet wired (EB-08). Current listings post without images.
  Dale decides photo hosting approach (EB-06) before image upload is built.

## Batch Mode (future — not yet built)
Once credentials are confirmed working on item 1:
```
python lister/ebay_lister.py --account pilotronics --all --dry-run
```
Then verify + publish in batches of 3.
