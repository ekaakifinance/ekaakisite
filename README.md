# Ekaaki — site setup

A single-file site (`index.html`) for Ekaaki's financial services — secured &
unsecured loans, insurance, credit cards and mutual funds. Two forms write to one
Google Sheet (tabs **Leads** and **Contacts**). No build step, no server.

Files: `index.html` (the site), `Code.gs` (the Google Apps Script), `README.md`.

---

## 1 · Set up the Google Sheet (5 min)

1. Create a new Google Sheet (any name, e.g. *Ekaaki Leads*). Leave it empty —
   the script creates the **Leads** and **Contacts** tabs with headers for you.
2. In that Sheet: **Extensions → Apps Script**.
3. Delete the sample `function myFunction(){}`, paste in all of **`Code.gs`**, and **Save** (💾).
4. **Deploy → New deployment**. Click the gear ⚙ → **Web app**.
   - **Execute as:** Me
   - **Who has access:** Anyone
   - **Deploy** → approve the permissions prompt (it's your own script).
5. Copy the **Web app URL** it gives you (ends in `/exec`).

> **Updating later:** after editing `Code.gs`, go **Manage deployments → edit (pencil)
> → Version: New version → Deploy**, or changes won't go live. The URL stays the same.

## 2 · Connect the site

Open `index.html`, find this line near the bottom:

```js
const SCRIPT_URL = "PASTE_YOUR_APPS_SCRIPT_URL_HERE";
```

Replace the placeholder with your `/exec` URL. Save.

## 3 · Update the remaining placeholders

The real phone, WhatsApp and Bhopal address are already filled in. Still to set:
- **Email** — set to `shantanu@ekaaki.com` in the footer. Change it in `index.html` if needed.
- **Site URL** — search for `https://ekaaki.in/` and replace with your live URL
  (e.g. `https://username.github.io/repo/`) in the `canonical` link, the `og:`/`twitter:`
  tags, and the JSON-LD `@id`/`url` fields. This makes search + social previews correct.
- **og-image** (optional) — add a 1200×630 share image named `og-image.png` for nicer
  link previews on WhatsApp/social.

### What's already in for search + answer engines (SEO / GEO / AEO)
- Descriptive `<title>` and meta description (products + location)
- Open Graph + Twitter cards for link previews
- Local geo meta tags (region MP, Bhopal, coordinates)
- JSON-LD **FinancialService** (name, address, phone, hours, area served, Instagram, products)
- JSON-LD **FAQPage** + a matching on-page FAQ — the format answer engines quote
- Mobile-first responsive layout, semantic headings, keyboard focus, reduced-motion

## 4 · Publish on GitHub Pages

1. Create a repo, upload `index.html` to the root.
2. **Settings → Pages → Source: Deploy from a branch → `main` / root → Save.**
3. Your site is live at `https://<username>.github.io/<repo>/` in a minute or two.

---

## What gets captured

Both forms post to the same workbook. Columns are created automatically:

**Leads** tab (enquiry form): Timestamp · Name · Mobile · City · **Interested In**
(loan / insurance / card / mutual fund) · **Amount / Budget** · Monthly Income · Employment · Consent · Source

**Contacts** tab (contact form): Timestamp · Name · Email · Mobile · Query · Source

> If you already ran a test before this update, the **Leads** tab may still have the
> old "Loan Type" / "Amount" headers — the script only writes headers when it first
> creates a tab. Delete the old **Leads** tab once (or clear its header row) and submit
> again so the new headers are written.

## Test it

Submit each form once. A new row should appear in the matching tab within a couple of
seconds. The page shows a confirmation message; because the request is sent in
`no-cors` mode (the simplest cross-site setup), the page assumes success once the
request is sent — so check the Sheet to confirm the first few.

## Notes

- **One form, all products:** the enquiry form's "Interested in" dropdown covers
  unsecured/secured loans, insurance, credit cards and mutual funds. Amount/Budget is
  required; Monthly income and Employment are optional (they don't apply to every product).
- **Validation:** required fields + a 10-digit mobile check run before sending.
- **Consent:** the enquiry form won't submit without the authorisation checkbox.
- **Spam:** if you start getting junk, add a honeypot field or reCAPTCHA — say the word.
- **Disclaimer:** the footer disclaimer is there for compliance — Ekaaki is a referral /
  distribution service (loans, insurance, cards, mutual funds), not the provider, and
  the mutual-fund "subject to market risks" line is included. Have someone confirm the
  wording fits your partner agreements before publishing.
