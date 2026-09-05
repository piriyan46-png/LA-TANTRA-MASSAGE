# LA Tantra Massage — How to Run This Project

This guide is written for someone who is **not** a software engineer. Follow it
top to bottom. Every command goes into the **Terminal** app (macOS) or
**PowerShell** (Windows).

If a step fails, stop there — the later steps will not work. Jump to
[Troubleshooting](#12-troubleshooting).

> **Current status:** the website is not built yet. See [`todo.md`](todo.md)
> for what is done and what is next. Sections 1–4 and 8 of this guide work
> today; the rest apply once Phase 1 of the build is complete.

**Other documents:** [`todo.md`](todo.md) — task list ·
[`TECHNICAL.md`](TECHNICAL.md) — technical reference ·
[`ProjectDocs.md`](ProjectDocs.md) — business facts

---

## 1. What this project is

A single-page website for LA Tantra Massage. Visitors read about the services
and submit a booking request without ever leaving the page. Each request is:

1. **saved as a row in a private Google Sheet**, and
2. **emailed to you**, so you know immediately.

You then confirm availability with the customer yourself, over WhatsApp or
phone. The website never confirms a booking on its own.

---

## 2. What you need installed

### Node.js (version 20 or newer)

Node.js is the program that runs the website on your computer.

Check whether you already have it:

```bash
node --version
```

If you see something like `v20.11.0` or higher, you are set. If you see
"command not found", download the **LTS** version from
[nodejs.org](https://nodejs.org) and install it. Then close and reopen your
Terminal and check again.

### Git

```bash
git --version
```

If missing: macOS will offer to install it when you run that command. On
Windows, get it from [git-scm.com](https://git-scm.com).

### A code editor

[VS Code](https://code.visualstudio.com) is free and works well. You need it
to edit the settings file in step 5.

---

## 3. Get the project onto your computer

If you already have the folder (you are reading this file inside it), skip
this step.

```bash
git clone <your-repository-url>
cd LA-TANTRA-MASSAGE
```

---

## 4. Install the project's dependencies

From inside the project folder:

```bash
npm install
```

This downloads the libraries the site needs into a `node_modules` folder. It
takes a minute or two the first time. You only rerun this when someone
changes the project's dependencies.

---

## 5. Set up the Google Sheet (where bookings are saved)

This is the longest step. Take it slowly — you only ever do it once.

### 5.1 Create the spreadsheet

1. Go to [sheets.google.com](https://sheets.google.com) and create a blank
   spreadsheet.
2. Name it **LA Tantra Bookings**.
3. At the bottom, rename the tab from `Sheet1` to exactly **`Bookings`**
   (right-click the tab → Rename). The capital B matters.
4. In row 1, type these 15 column headers, one per cell from A1 to O1:

   `Timestamp` · `Booking Ref` · `Name` · `Phone` · `Service` · `Duration` ·
   `Quoted Price` · `Preferred Date` · `Preferred Time` · `City` ·
   `Visit Type` · `Area` · `Notes` · `Status` · `Owner Notes`

5. Look at the address bar. The URL looks like:

   ```
   https://docs.google.com/spreadsheets/d/1a2B3cD4eF5gH6iJ7kL8/edit#gid=0
                                          └──── this is your Sheet ID ────┘
   ```

   Copy the long code between `/d/` and `/edit`. **Save it somewhere** — this
   is your `GOOGLE_SHEET_ID`.

### 5.2 Create a "service account"

A service account is a **robot user** with its own email address. Instead of
giving the website your personal Google password, you give this robot access
to one single spreadsheet. If its key were ever stolen, the thief gets that
one spreadsheet and nothing else — not your Gmail, not your Drive.

1. Go to [console.cloud.google.com](https://console.cloud.google.com) and
   sign in.
2. At the top, click the project dropdown → **New Project**. Name it
   `la-tantra-website` → **Create**. Wait for it, then make sure it is the
   selected project.
3. In the search bar at the top, search for **Google Sheets API**, open it,
   and click **Enable**.
4. In the search bar, go to **Service Accounts** (under IAM & Admin).
5. Click **Create Service Account**.
   - Name: `la-tantra-booking-writer`
   - Click **Create and Continue**, then **Continue**, then **Done**.
     You do not need to grant it any project roles.
6. You now see it in the list. **Copy its email address** — it looks like
   `la-tantra-booking-writer@la-tantra-website.iam.gserviceaccount.com`.
   Save it. This is your `GOOGLE_SERVICE_ACCOUNT_EMAIL`.
7. Click the service account → **Keys** tab → **Add Key** → **Create new key**
   → choose **JSON** → **Create**. A `.json` file downloads to your computer.

> ⚠️ **That JSON file is a password.** Never email it, never put it in a chat,
> and never place it inside this project folder. You will copy two values out
> of it in step 7, then delete it.

### 5.3 Give the robot access to your spreadsheet

1. Open your **LA Tantra Bookings** spreadsheet.
2. Click **Share**.
3. Paste the service account's email address from step 5.2.6.
4. Set its permission to **Editor**.
5. **Untick "Notify people"** (robots do not read email) and click **Share**.

> ⚠️ **Never set this spreadsheet to "Anyone with the link".** It will contain
> customers' names, phone numbers and localities. Sharing it with the service
> account is the only access it should have.

---

## 6. Set up email alerts

1. Sign up free at [resend.com](https://resend.com).
2. Go to **API Keys** → **Create API Key**. Copy it — you can only see it
   once. This is your `RESEND_API_KEY`.
3. For testing, you can send from `onboarding@resend.dev`. Before going live,
   add and verify your own domain under **Domains** so alerts come from your
   real address and do not land in spam.

---

## 7. Create your settings file

In the project folder, create a file named exactly **`.env.local`**. The
leading dot matters. This file holds your passwords and is never uploaded to
GitHub.

There is a template to copy:

```bash
cp .env.example .env.local
```

Open `.env.local` in your editor and fill it in:

```bash
# --- Google Sheets ---
GOOGLE_SERVICE_ACCOUNT_EMAIL="la-tantra-booking-writer@la-tantra-website.iam.gserviceaccount.com"
GOOGLE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\nMIIEvQIBADAN...\n-----END PRIVATE KEY-----\n"
GOOGLE_SHEET_ID="1a2B3cD4eF5gH6iJ7kL8"
GOOGLE_SHEET_TAB="Bookings"

# --- Email alerts ---
RESEND_API_KEY="re_xxxxxxxxxxxx"
BOOKING_ALERT_FROM="onboarding@resend.dev"
BOOKING_ALERT_TO="your-real-email@example.com"

# --- Public (these are visible to visitors — no secrets here) ---
NEXT_PUBLIC_WHATSAPP_NUMBER="919876543210"
NEXT_PUBLIC_SITE_URL="http://localhost:3000"
```

**Where the two Google values come from:** open the JSON file you downloaded
in step 5.2.7 in your editor. Find the line starting `"client_email"` — that
value goes into `GOOGLE_SERVICE_ACCOUNT_EMAIL`. Find `"private_key"` — copy
its value, **including** the `\n` sequences and the surrounding quotes, into
`GOOGLE_PRIVATE_KEY`. Those `\n` marks are line breaks; if you remove or
"tidy" them, the key stops working.

**Once both values are copied across, delete the downloaded JSON file** —
including from your Downloads folder and your Trash.

`NEXT_PUBLIC_WHATSAPP_NUMBER` is the country code followed by the number,
with no `+`, no spaces and no dashes.

---

## 8. Run the website on your computer

```bash
npm run dev
```

Then open **http://localhost:3000** in your browser.

Leave that Terminal window open — closing it stops the site. Edit any file
and save it, and the browser updates by itself within a second.

To stop the site, click into the Terminal and press **Ctrl + C**.

---

## 9. Everyday commands

| Command | What it does |
|---|---|
| `npm run dev` | Run the site locally while you work |
| `npm run build` | Build the production version — catches errors before deploying |
| `npm start` | Run the built production version locally |
| `npm test` | Run the automated tests |
| `npm run test:coverage` | Run tests and report how much of the code they cover |
| `npm run test:e2e` | Run browser tests that fill in the booking form for real |
| `npm run lint` | Check code style |
| `npm run format` | Auto-tidy the code formatting |

**Before you deploy anything, run `npm run build` and `npm test`.** If either
reports an error, do not deploy.

---

## 10. Changing prices and text

Almost everything you will want to change lives in one folder:
**`src/content/`**.

| To change | Edit this file |
|---|---|
| Service names, descriptions, durations, **prices** | `src/content/services.ts` |
| Cities, phone number, email, trust badges | `src/content/site.ts` |
| FAQ questions and answers | `src/content/faqs.ts` |
| Customer testimonials | `src/content/reviews.ts` |

For example, to change the Thai Massage price, open
`src/content/services.ts`, find the `thai` entry, and change `price: 999` to
the new figure. Save, and the local site updates immediately. To put the
change live, follow step 11.

Colours and fonts come from `UI-Mockup/DESIGN.md` and are applied
project-wide — do not change colours inside individual components, or the
site drifts out of consistency.

---

## 11. Putting the site on the internet (Vercel)

### First time

1. Push this project to a **private** GitHub repository.
2. Sign up at [vercel.com](https://vercel.com) with your GitHub account.
3. Click **Add New → Project** and pick this repository.
4. Vercel detects Next.js automatically. Before clicking Deploy, open
   **Environment Variables** and add **every** line from your `.env.local`,
   one at a time — name on the left, value on the right.
   - Set `NEXT_PUBLIC_SITE_URL` to your real address, not `localhost`.
   - For `GOOGLE_PRIVATE_KEY`, paste the value exactly as in `.env.local`,
     `\n` sequences and all.
5. Click **Deploy**. You get a live URL in a couple of minutes.
6. **Submit one real test booking on that URL.** Confirm a row appears in the
   spreadsheet *and* the alert email arrives. Do this before pointing your
   domain at it.

### After that

Every push to your `main` branch deploys automatically. Pushing to any other
branch gives you a private preview link to check first.

### Your own domain

In Vercel: **Settings → Domains → Add**, then follow its instructions for
updating the DNS records at whoever sold you the domain. HTTPS is set up for
you automatically.

---

## 12. Troubleshooting

**`command not found: npm`**
Node.js is not installed, or the Terminal was open before you installed it.
Install from [nodejs.org](https://nodejs.org), then close and reopen Terminal.

**`Error: Missing environment variable: ...`**
That variable is absent or misspelt in `.env.local`. Names are
case-sensitive. On Vercel, check it was added there too — `.env.local` stays
on your computer and is never uploaded.

**Bookings do not appear in the spreadsheet**
Work through these in order:
1. Is the tab named exactly `Bookings`?
2. Is `GOOGLE_SHEET_ID` the code from between `/d/` and `/edit`, and nothing
   else?
3. Did you share the spreadsheet with the service account's email as
   **Editor**?
4. Is the **Google Sheets API** enabled in your Google Cloud project?

**`error:1E08010C:DECODER routines::unsupported`, or any Google auth error**
`GOOGLE_PRIVATE_KEY` is malformed. It must be wrapped in double quotes and
keep every `\n` exactly as it appeared in the JSON file.

**Port 3000 is already in use**
Another copy is already running. Either use it, or run `npm run dev -- -p 3001`.

**The site looks unstyled**
Stop the server (Ctrl + C), delete the `.next` folder, and run `npm run dev`
again.

---

## 13. Safety rules

1. **Never commit `.env.local` or the Google JSON key file to Git.** They are
   in `.gitignore` for this reason — leave that entry alone.
2. **Never share the booking spreadsheet publicly.** It holds customers'
   names, phone numbers and localities.
3. **Never put a secret in a variable starting `NEXT_PUBLIC_`.** That prefix
   means "send this to every visitor's browser".
4. **Run `npm run build` and `npm test` before deploying.**
5. **Do not invent business facts.** Prices, hours, policies, fees and areas
   served must come from the business owner. Anything still unknown is listed
   in Phase 9 of [`todo.md`](todo.md).
6. **If a key is ever exposed**, delete it in Google Cloud / Resend and
   create a new one immediately. Do not just remove it from a file — the old
   key stays valid until you revoke it.
