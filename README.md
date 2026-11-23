# Grey Street Dentist: CDBS eligibility landing page

Lives at **book.greystreetdentist.com.au/cdbs/** as its own project (separate from Synergy's).

## What's in this folder

| Path | What it is |
|---|---|
| `cdbs/index.html` | The landing page (eligibility check at the top, then CDBS info, booking, find us) |
| `api/cdbs-check.js` | Serverless function that emails each submission |
| `assets/` | Logo, hero photo, Medicare and payment logos |
| `vercel.json` | Sends `/` to `/cdbs/`, adds noindex headers |
| `robots.txt`, `package.json` | Blocks search engines; marks the function as an ES module |

## Deploy (about 10 minutes)

1. In Vercel, create a **new project** and upload this folder as is (Framework preset: Other, no build command).
2. Project > Settings > Domains: add `book.greystreetdentist.com.au`.
3. In the DNS for greystreetdentist.com.au, add the record Vercel shows (normally `CNAME  book  cname.vercel-dns.com`).
4. Open `https://book.greystreetdentist.com.au/cdbs/` and run the test below.

Important: the page and the function must sit in the same Vercel project. Never upload `api/cdbs-check.js` to ordinary web hosting, because it would be served as a readable file.

## Where submissions go

- To: `reception@greystreetdentist.com.au`
- Copy: `rowayne@gyaclients.com`
- Subject: `Grey Street Dentist: new CDBS eligibility check: <child name>`
- Reply-To is the parent's email, so reception can reply straight from the inbox.

To change these without touching code, set Vercel environment variables `CDBS_TO` and `CDBS_CC` (comma separated for more than one address). 

Nothing is stored anywhere else. Submissions exist only as the emails.

## Three things to fill in

All three are clearly marked in `cdbs/index.html`. The page works without them.

1. **Booking embed** (search for `BOOKING CONFIG`). Paste the St Kilda booking address from the iframe on greystreetdentist.com.au/book-now/. Until then the booking section shows a button that opens that page. Do not copy the address from the Synergy pages, that one is the Parkdale calendar.
2. **Tracking** (search for `TRACKING CONFIG`). Add Grey Street's own GTM container, Meta Pixel and Google Ads IDs. Left blank, nothing loads and no conversions are recorded.
3. **Sender address** (optional). Emails currently send from the verified Synergy sender with the display name "Grey Street Dentist Website". Once greystreetdentist.com.au is verified in SMTP2GO, set the `SMTP_FROM` environment variable to a Grey Street address.

## Test after deploying

1. Fill the form with Medicare number `2123 45670 1` and ref number `2` (a valid test number).
2. Confirm the thank-you message shows, and that the email reaches reception and the copy address.
3. In GTM preview, confirm these events fire: `cdbs_check_start`, `cdbs_check_step_2`, `cdbs_check_step_3`, `cdbs_check_submit`. The Meta Pixel fires `Lead` on submit.

## Events available for conversion tracking

| Event | When |
|---|---|
| `cdbs_check_submit` | Form submitted successfully (main conversion; Meta `Lead`) |
| `cdbs_check_start`, `cdbs_check_step_2`, `cdbs_check_step_3` | Funnel steps |
| `cdbs_check_click` | A "Check eligibility" button was clicked |
| `call_click` | A phone link was tapped (Meta `Contact`) |
| `book_now_click` | A booking button was clicked (Meta `Schedule`) |
