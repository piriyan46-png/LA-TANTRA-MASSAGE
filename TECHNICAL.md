# LA Tantra Massage — Product & Technical Details

The working technical reference for this project. For *why* each decision was
made, see
[`docs/superpowers/specs/2026-09-05-la-tantra-single-page-design.md`](docs/superpowers/specs/2026-09-05-la-tantra-single-page-design.md).
For *how to run it*, see [`README.md`](README.md). For *what is left to do*,
see [`todo.md`](todo.md).

---

## 1. Product summary

| | |
|---|---|
| **Product** | Single-page marketing and booking website |
| **Business** | LA Tantra Massage — doorstep massage & wellness |
| **Cities served** | Chennai, Pondicherry, Trichy, Bangalore |
| **Primary goal** | Capture booking *requests* |
| **Confirmation** | Manual, by a human, over WhatsApp or phone |
| **Payment** | After the service. No online payment. |
| **Audience** | Adults 18+ only |
| **Tone** | Calm, private, professional, consent-led |

The website never confirms a slot. It collects a request and tells the
visitor a human will confirm shortly. Every piece of copy must respect that.

## 2. Stack

| Concern | Technology | Notes |
|---|---|---|
| Framework | Next.js (App Router) | React Server Components by default |
| Language | TypeScript | `strict: true` |
| Styling | Tailwind CSS | theme generated from `UI-Mockup/DESIGN.md` |
| Fonts | `next/font` | EB Garamond (headings), Manrope (body) |
| Validation | Zod | one schema, shared browser + server |
| Booking store | Google Sheets | `googleapis`, service-account auth |
| Email alerts | Resend | owner notification per booking |
| Unit / integration tests | Vitest | |
| E2E tests | Playwright | |
| Lint / format | ESLint + Prettier | |
| Hosting | Vercel | |
| CI | GitHub Actions | lint, typecheck, test on every push |

Exact versions are pinned in `package.json` at scaffold time against what is
current then, rather than being frozen into this document.

## 3. Repository layout

```
LA-TANTRA-MASSAGE/
├── README.md                  ← how to run this project
├── TECHNICAL.md               ← this file
├── todo.md                    ← task list with status
├── ProjectDocs.md             ← business facts (source of truth for content)
├── UI-Mockup/
│   ├── DESIGN.md              ← design tokens (drives the Tailwind theme)
│   ├── code.html              ← homepage mockup
│   └── screen.png             ← rendered mockup
├── docs/superpowers/specs/    ← design decision records
└── src/
    ├── app/
    │   ├── layout.tsx         ← fonts, metadata, skip link
    │   ├── page.tsx           ← the single page: composes all sections
    │   ├── globals.css        ← Tailwind + design tokens
    │   ├── api/booking/route.ts
    │   ├── privacy/page.tsx
    │   ├── terms/page.tsx
    │   └── boundaries/page.tsx
    ├── components/
    │   ├── sections/          ← one file per page section
    │   ├── booking/           ← the form (the only client island)
    │   └── ui/                ← Button, Input, Card, Chip, Accordion
    ├── content/
    │   ├── services.ts        ← EDIT HERE to change prices
    │   ├── site.ts            ← cities, contact, trust badges
    │   ├── faqs.ts
    │   └── reviews.ts
    └── lib/
        ├── env.ts             ← validates env vars at startup
        ├── booking-schema.ts
        ├── booking-reference.ts
        ├── sheets.ts
        ├── email.ts
        ├── rate-limit.ts
        └── format.ts          ← ₹ formatting, phone normalisation
```

**If you only ever edit one file, it is `src/content/services.ts`.** That is
where service names, descriptions, durations and prices live.

## 4. Page sections

Single route `/`. Nav links are in-page anchors.

| Order | Section | Anchor | Interactive? |
|---|---|---|---|
| 1 | Header (sticky) | — | mobile menu toggle |
| 2 | Hero | `#home` | no |
| 3 | Trust bar | — | no |
| 4 | How It Works | `#how-it-works` | no |
| 5 | Services | `#services` | "Reserve" scrolls to booking, pre-selects service |
| 6 | About | `#about` | no |
| 7 | Locations | `#locations` | city chips |
| 8 | Reviews | `#reviews` | no |
| 9 | Booking form | `#booking` | **yes — the only client island** |
| 10 | FAQ | `#faq` | accordion |
| 11 | Contact / CTA | `#contact` | WhatsApp + call links |
| 12 | Footer | — | policy links |

## 5. Service catalogue

Transcribed from `ProjectDocs.md`. **Do not invent or alter prices.**

| ID | Name | Duration | Price | Audience |
|---|---|---|---|---|
| `thai` | Thai Massage | 60–90 min | ₹999 | — |
| `full-body` | Full Body Massage | 60–90 min | ₹999 | — |
| `neuro` | Neuro Massage | 60–90 min | ₹999 | — |
| `tantra` | Tantra Massage | 60–90 min | ₹1,499 | — |
| `yoni` | Yoni Massage | 60–90 min | ₹1,499 | Adult women |
| `breast-tissue` | Breast Tissue Massage | 60–90 min | ₹1,499 | Adult women |

The homepage Services section shows three cards as in the mockup; the full
list of six appears in the booking form's service selector.

## 6. Booking API contract

### `POST /api/booking`

Request body (JSON):

```jsonc
{
  "name":          "string, 2–80 chars",
  "phone":         "string, Indian mobile",
  "serviceId":     "one of the catalogue IDs",
  "durationMins":  60,               // 60 | 90
  "preferredDate": "YYYY-MM-DD",     // today .. today+60d
  "preferredTime": "morning",        // morning | afternoon | evening | late-evening
  "city":          "chennai",        // chennai | pondicherry | trichy | bangalore
  "visitType":     "home",           // home | hotel
  "area":          "string, 2–120 chars",
  "notes":         "string, ≤500 chars, optional",
  "website":       "",               // honeypot — must be empty
  "renderedAt":    1725500000000     // ms epoch when form rendered
}
```

Responses:

| Status | Body | Meaning |
|---|---|---|
| `200` | `{ ok: true, reference: "LT-260905-A7K2" }` | Request recorded |
| `400` | `{ ok: false, fieldErrors: { phone: "..." } }` | Validation failed |
| `429` | `{ ok: false, error: "RATE_LIMITED" }` | Too many attempts |
| `503` | `{ ok: false, error: "STORAGE_UNAVAILABLE" }` | Sheet **and** email both failed |

Spam-rejected submissions return `200` with a reference, exactly like a real
success. Signalling detection to a bot only helps it adapt.

### Booking reference format

`LT-YYMMDD-XXXX` — e.g. `LT-260905-A7K2`. The four characters are random from
an unambiguous alphabet (no `0`/`O`, no `1`/`I`), so a reference can be read
aloud over the phone without confusion.

## 7. Google Sheet structure

Spreadsheet shared with the service account **only**. Tab named `Bookings`.
Row 1 is a header row; the app appends from row 2 down.

| Col | Header | Written by | Example |
|---|---|---|---|
| A | Timestamp (IST) | app | `2026-09-05 18:42:11` |
| B | Booking Ref | app | `LT-260905-A7K2` |
| C | Name | app | |
| D | Phone | app | `+919876543210` |
| E | Service | app | `Tantra Massage` |
| F | Duration | app | `90 min` |
| G | Quoted Price | app | `₹1,499` |
| H | Preferred Date | app | `2026-09-08` |
| I | Preferred Time | app | `Evening (6–9 PM)` |
| J | City | app | `Chennai` |
| K | Visit Type | app | `Home` |
| L | Area | app | |
| M | Notes | app | |
| N | **Status** | **you, by hand** | `NEW` → `CONFIRMED` / `DECLINED` / `COMPLETED` |
| O | **Owner Notes** | **you, by hand** | |

The application only ever **appends**. It never reads, edits or deletes rows,
so your manual edits in columns N and O can never be overwritten.

The full street address is deliberately **not** collected — see §9.

## 8. Environment variables

| Variable | Required | Purpose |
|---|---|---|
| `GOOGLE_SERVICE_ACCOUNT_EMAIL` | yes | Service account identity |
| `GOOGLE_PRIVATE_KEY` | yes | Its private key (keep the `\n` escapes) |
| `GOOGLE_SHEET_ID` | yes | ID from the spreadsheet URL |
| `GOOGLE_SHEET_TAB` | no | Tab name, defaults to `Bookings` |
| `RESEND_API_KEY` | yes | Sending email alerts |
| `BOOKING_ALERT_FROM` | yes | Verified sender address |
| `BOOKING_ALERT_TO` | yes | Where alerts are delivered |
| `NEXT_PUBLIC_WHATSAPP_NUMBER` | yes | Public — used in WhatsApp links |
| `NEXT_PUBLIC_SITE_URL` | yes | Public — canonical URL for SEO |

`src/lib/env.ts` validates all of these when the server starts. A missing or
malformed variable stops the server with a clear message, rather than
failing later while a real customer is submitting a form.

Anything prefixed `NEXT_PUBLIC_` is visible in the browser. Never put a
secret behind that prefix.

## 9. Security & privacy

- **No credentials in the browser.** The booking form posts to our own
  origin; only the server holds Google and Resend keys.
- **Least privilege.** The service account is added to one spreadsheet, not
  to a Drive folder or an organisation.
- **Minimal collection.** No full street address, no email address, no date
  of birth, no payment details. The address is taken by a human during
  confirmation, once the booking is real.
- **No PII in logs.** Logs record the booking reference, outcome and error
  code — never a name, phone number or locality.
- **Server-side re-validation.** Browser validation is a convenience; the
  server never trusts input.
- **Spam defence.** Honeypot field + minimum-time trap + per-IP rate limit.
  No CAPTCHA at launch — it suppresses genuine bookings, and this traffic
  profile does not warrant it. `todo.md` records it as a later option.
- **Security headers** set in `next.config`: CSP, HSTS,
  `X-Content-Type-Options: nosniff`, `Referrer-Policy: strict-origin-when-cross-origin`.
- **Secrets never committed.** `.env.local` is gitignored; the downloaded
  Google JSON key file is deleted from disk after its values are copied out.

## 10. Performance & SEO targets

| Metric | Target |
|---|---|
| Lighthouse Performance (mobile) | ≥ 90 |
| Lighthouse Accessibility | ≥ 95 |
| Lighthouse SEO | ≥ 95 |
| Largest Contentful Paint | < 2.5s on 4G |
| Cumulative Layout Shift | < 0.1 |
| Client JS shipped | < 100 KB gzipped |

Achieved by: Server Components for all read-only content, one client island,
`next/image` for the hero and service photography, self-hosted fonts with
`display: swap`, and no third-party scripts.

SEO: one `<h1>`, descriptive metadata, Open Graph tags, `LocalBusiness` +
`FAQPage` JSON-LD, `robots.txt`, `sitemap.xml`, canonical URL.

## 11. Accessibility

WCAG 2.1 AA. Skip link; landmark regions; anchor navigation that moves
keyboard focus and not only scroll position; labelled inputs with errors
linked by `aria-describedby` and announced via a live region; visible focus
rings; contrast-verified palette; `prefers-reduced-motion` respected.

## 12. Testing strategy

| Level | Tool | Scope |
|---|---|---|
| Unit | Vitest | schema, phone normalisation, ₹ formatting, booking reference, sheet-row builder |
| Integration | Vitest | `/api/booking` with Google + Resend faked: success, sheet-down, email-down, both-down, invalid input, honeypot filled, rate limited |
| E2E | Playwright | booking happy path; every nav anchor resolves; mobile viewport |

Coverage target 80%+ overall, 100% on the booking path. Tests are written
before the implementation they cover.

## 13. Content still owned by the business

These are `TODO` in `ProjectDocs.md` and **must not be invented**. They are
marked `TODO(owner):` in the codebase so they can be found with a search.

Real WhatsApp number · real email address · operating hours and last booking
time · cancellation, reschedule and no-show policy · travel / late-night /
home-visit fees · therapist gender rules · client gender policy · health
restrictions · exact areas served and not served · the About copy · the FAQ
answers · the logo file · whether the mockup's three testimonials are real.

The mockup's `concierge@latantra.com` address and its testimonials are
placeholders and are treated as unconfirmed.
