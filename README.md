# Calorie Scanner (Vercel + Gemini)

Phone-friendly calorie tracker: AI food-photo scan, barcode scan, steps, daily history, CSV export.
Works without Claude on the viewer's device. The photo scan runs through a small server function
that holds your Gemini API key.

## Files
- `index.html` – the whole app (each person's data is saved in their own browser)
- `api/scan.js` – Vercel function: receives a photo, asks Gemini, returns the nutrition estimate
- `vendor/zxing.min.js` – barcode reader fallback for browsers without built-in support

## Deploy (Vercel)
1. Create a Gemini API key in Google AI Studio and check the pricing/quota rules for your account.
2. Put this folder in a GitHub repository.
3. In Vercel: Add New Project, import the repo. Framework preset "Other". No build command needed.
4. In Project Settings, Environment Variables, add:
   - `GEMINI_API_KEY` – your key (required)
   - `ACCESS_CODE` – a passcode you share with friends (strongly recommended)
   - `GEMINI_MODEL` – optional; defaults to `gemini-3.6-flash`. If Google renames or retires it,
     set the current vision-capable model name here.
   - `LIMIT_PER_HOUR` – optional scans per IP per hour, default 30
5. Deploy, then share the URL and access code. Friends are asked for the code on their first
   photo scan; it is remembered on that device.

Local test: `npm i -g vercel`, then `vercel dev` (variables go in `.env.local`; never commit it).

## Notes
- Barcode lookups call Open Food Facts from the browser (free, no key). Coverage varies.
- Only photo scans (sent to Google via your server) and barcode lookups leave the device.
- Free API tiers may have different data-use terms than paid ones. Read Google's current terms
  before inviting friends to send photos, and tell them where photos go.
- The in-app agreement is placeholder text. It currently says photos go to Claude, so change that
  wording to Google Gemini, and add a real privacy policy before sharing widely.
