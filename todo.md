# LA Tantra Massage — Build Plan & Status

**Last updated:** 2026-09-05
**What we're building:** a single-page Next.js website with an in-page booking
form that saves to Google Sheets and emails you each new request.

Related docs: [`TECHNICAL.md`](TECHNICAL.md) · [`README.md`](README.md) ·
[design spec](docs/superpowers/specs/2026-09-05-la-tantra-single-page-design.md)

### Status key

| | Meaning |
|---|---|
| ✅ | Done |
| 🔄 | In progress |
| ⬜ | Not started |
| ⛔ | Blocked — waiting on something |
| ⏸️ | Deferred — deliberately not now |

---

## Progress at a glance

| Phase | Status | Done |
|---|---|---|
| 0. Planning & documentation | ✅ Complete | 4 / 4 |
| 1. Project foundation | ⬜ Not started | 0 / 8 |
| 2. Design system | ⬜ Not started | 0 / 6 |
| 3. Content layer | ⬜ Not started | 0 / 5 |
| 4. Page sections | ⬜ Not started | 0 / 12 |
| 5. Booking backend | ⬜ Not started | 0 / 9 |
| 6. Booking form UI | ⬜ Not started | 0 / 7 |
| 7. Quality & hardening | ⬜ Not started | 0 / 9 |
| 8. Launch | ⬜ Not started | 0 / 7 |
| 9. Owner content | ⛔ Blocked on you | 0 / 13 |
| **Total** | | **4 / 80** |

---

## Phase 0 — Planning & documentation

| # | Task | Status |
|---|---|---|
| 0.1 | Read mockup, design tokens and business docs | ✅ |
| 0.2 | Write design spec | ✅ |
| 0.3 | Write `TECHNICAL.md` | ✅ |
| 0.4 | Write `README.md` and this `todo.md` | ✅ |

---

## Phase 1 — Project foundation

Getting an empty but correct Next.js project running.

| # | Task | Status | Notes |
|---|---|---|---|
| 1.1 | Scaffold Next.js app (TypeScript, App Router, Tailwind) into `src/` | ⬜ | |
| 1.2 | Set `strict: true` and path alias `@/*` in `tsconfig.json` | ⬜ | |
| 1.3 | Configure ESLint + Prettier, add `npm run lint` / `format` | ⬜ | |
| 1.4 | Set up Vitest with a coverage reporter | ⬜ | |
| 1.5 | Set up Playwright | ⬜ | |
| 1.6 | Write `.gitignore` — must cover `.env*`, `.next`, `node_modules` | ⬜ | Do before any secret exists |
| 1.7 | Create `.env.example` listing every variable with dummy values | ⬜ | |
| 1.8 | GitHub Actions: lint + typecheck + test on push | ⬜ | |

---

## Phase 2 — Design system

Turning `UI-Mockup/DESIGN.md` into reusable code.

| # | Task | Status | Notes |
|---|---|---|---|
| 2.1 | Port all colour, spacing, radius and typography tokens into the Tailwind theme | ⬜ | Copy from `DESIGN.md` frontmatter — do not re-pick values |
| 2.2 | Load EB Garamond + Manrope via `next/font` (self-hosted) | ⬜ | |
| 2.3 | Base layout: `<html>` metadata, skip link, landmarks | ⬜ | |
| 2.4 | `Button` component — primary / secondary / ghost variants | ⬜ | Per `DESIGN.md` §Components |
| 2.5 | `Input`, `Select`, `Textarea` with label + error + focus ring | ⬜ | |
| 2.6 | `Card`, `Chip`, `Badge`, `Accordion` | ⬜ | |

---

## Phase 3 — Content layer

Where you will edit prices and text.

| # | Task | Status | Notes |
|---|---|---|---|
| 3.1 | `content/services.ts` — all 6 services, typed | ⬜ | Copy prices from `ProjectDocs.md`. Never invent. |
| 3.2 | `content/site.ts` — cities, contact, trust badges, nav links | ⬜ | Use `TODO(owner):` for unknown contact details |
| 3.3 | `content/faqs.ts` and `content/reviews.ts` | ⬜ | |
| 3.4 | `lib/format.ts` — ₹ formatting and phone normalisation | ⬜ | |
| 3.5 | Unit tests for formatters | ⬜ | Write these first |

---

## Phase 4 — Page sections

One file per section, all on the single page.

| # | Task | Status | Notes |
|---|---|---|---|
| 4.1 | Sticky header with anchor nav | ⬜ | |
| 4.2 | Mobile menu (drawer) | ⬜ | |
| 4.3 | Hero section | ⬜ | |
| 4.4 | Trust bar | ⬜ | |
| 4.5 | How It Works — 3 steps | ⬜ | |
| 4.6 | Services section — 3 cards + "View all" | ⬜ | "Reserve" pre-selects the service in the form |
| 4.7 | About section | ⛔ | Blocked on task 9.9 (About copy) |
| 4.8 | Locations section — 4 city chips | ⬜ | |
| 4.9 | Reviews section | ⬜ | |
| 4.10 | FAQ accordion | ⛔ | Blocked on task 9.10 (FAQ answers) |
| 4.11 | Contact / closing CTA | ⛔ | Blocked on task 9.1 (real contact details) |
| 4.12 | Footer with policy links | ⬜ | |

Sections blocked on content are still **built** with `TODO(owner):`
placeholders — only the final wording waits on you.

---

## Phase 5 — Booking backend

The part that saves and notifies. Highest-risk area — tests come first.

| # | Task | Status | Notes |
|---|---|---|---|
| 5.1 | `lib/booking-schema.ts` — Zod schema + tests | ⬜ | Test first |
| 5.2 | `lib/booking-reference.ts` — `LT-YYMMDD-XXXX` + tests | ⬜ | Unambiguous alphabet |
| 5.3 | `lib/env.ts` — validate env vars at startup | ⬜ | |
| 5.4 | Google Cloud: create project + service account + key | ⬜ | Steps are in `README.md` |
| 5.5 | Create the spreadsheet, add header row, share with service account | ⬜ | Sharing must **not** be "anyone with the link" |
| 5.6 | `lib/sheets.ts` — append one row | ⬜ | Append only, never read or edit |
| 5.7 | `lib/email.ts` — owner alert, full details in body | ⬜ | Resend |
| 5.8 | `lib/rate-limit.ts` — per-IP limiting | ⬜ | |
| 5.9 | `app/api/booking/route.ts` + integration tests | ⬜ | Cover sheet-down, email-down, both-down, honeypot |

---

## Phase 6 — Booking form UI

| # | Task | Status | Notes |
|---|---|---|---|
| 6.1 | Form layout and fields per spec | ⬜ | |
| 6.2 | Live validation with inline errors | ⬜ | |
| 6.3 | Honeypot field + `renderedAt` time trap | ⬜ | |
| 6.4 | Submitting state — disable, spinner, prevent double-submit | ⬜ | |
| 6.5 | Success state — booking reference + WhatsApp button | ⬜ | |
| 6.6 | Failure state — "WhatsApp us directly" with the number | ⬜ | |
| 6.7 | Accessibility: labels, `aria-describedby`, live region, focus | ⬜ | |

---

## Phase 7 — Quality & hardening

| # | Task | Status | Notes |
|---|---|---|---|
| 7.1 | SEO metadata + Open Graph + canonical | ⬜ | |
| 7.2 | `LocalBusiness` + `FAQPage` JSON-LD | ⬜ | |
| 7.3 | `robots.txt` + `sitemap.xml` | ⬜ | |
| 7.4 | Security headers in `next.config` | ⬜ | CSP, HSTS, nosniff, referrer policy |
| 7.5 | Image optimisation via `next/image` | ⬜ | |
| 7.6 | Responsive pass: 360 / 768 / 1200 / 1600 px | ⬜ | |
| 7.7 | Playwright E2E suite | ⬜ | |
| 7.8 | Lighthouse ≥ 90 / 95 / 95 | ⬜ | |
| 7.9 | Verify coverage ≥ 80%, booking path 100% | ⬜ | |

---

## Phase 8 — Launch

| # | Task | Status | Notes |
|---|---|---|---|
| 8.1 | Push repository to GitHub (private) | ⬜ | |
| 8.2 | Connect to Vercel, deploy a preview | ⬜ | |
| 8.3 | Add all environment variables in Vercel | ⬜ | |
| 8.4 | End-to-end test on the preview: submit a real booking, confirm the row and the email arrive | ⬜ | Do this before the domain goes live |
| 8.5 | Privacy Policy, Terms, Boundaries & Consent pages | ⛔ | Blocked on task 9.11 |
| 8.6 | Connect the custom domain, verify HTTPS | ⛔ | Blocked on task 9.12 (domain not chosen) |
| 8.7 | Final accessibility + content review, then go live | ⬜ | |

---

## Phase 9 — Content only you can provide

⛔ **This whole phase is blocked on you, not on the build.** These are marked
`TODO` in `ProjectDocs.md`, and they must not be guessed — several carry
legal or safety weight.

| # | Item | Status |
|---|---|---|
| 9.1 | Real WhatsApp number and email address | ⛔ |
| 9.2 | Operating hours and last booking time | ⛔ |
| 9.3 | Cancellation, reschedule and no-show policy | ⛔ |
| 9.4 | Travel / late-night / home-visit fees, if any | ⛔ |
| 9.5 | Therapist gender rules | ⛔ |
| 9.6 | Which client genders are accepted | ⛔ |
| 9.7 | Health restrictions worth publishing | ⛔ |
| 9.8 | Exact areas served and not served | ⛔ |
| 9.9 | About section copy | ⛔ |
| 9.10 | FAQ questions and answers | ⛔ |
| 9.11 | Privacy, Terms, and Boundaries & Consent text | ⛔ |
| 9.12 | Domain name | ⛔ |
| 9.13 | Logo file, and confirmation that the mockup's 3 testimonials are real | ⛔ |

---

## Deferred — deliberately not doing now

| Item | Status | Why |
|---|---|---|
| Blog and blog detail pages | ⏸️ | Not needed to take a booking |
| Individual service detail pages | ⏸️ | Single-page design covers it |
| Online payment | ⏸️ | Payment is taken after service |
| Customer accounts / login | ⏸️ | No use case |
| Admin dashboard | ⏸️ | The Google Sheet is the admin view |
| Multi-language support | ⏸️ | Revisit after launch |
| reCAPTCHA | ⏸️ | Hurts genuine bookings; honeypot + rate limit first. Add if real spam appears. |
| Move off Google Sheets to a real database | ⏸️ | Revisit if booking volume grows or a data-privacy review requires it — see the note below |

---

## Known risks

| Risk | Why it matters | What we do about it |
|---|---|---|
| **Sensitive data in a spreadsheet** | Bookings pair a name, phone number and locality with an adults-only service. Sheets has no access audit trail and one careless share setting makes it public. | Private sheet, service-account access only, no full street address collected, no PII in logs. Revisit the database question before volume grows. |
| Google Sheets API outage | A booking could fail to save | The email alert carries full details, so it is a complete standalone record |
| Sheets API quota limits | Sustained high volume could hit them | Fine at expected volume; the deferred database migration is the answer if it changes |
| Unconfirmed business facts | Publishing a wrong policy or fee is a real-world problem | Nothing is invented; everything unknown is `TODO(owner):` and tracked in Phase 9 |
| Mockup testimonials may be fictional | Publishing fake reviews is deceptive and in some places unlawful | Task 9.13 — confirm or replace before launch |
