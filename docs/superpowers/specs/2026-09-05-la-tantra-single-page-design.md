# LA Tantra Massage — Single-Page Website Design

**Date:** 2026-09-05
**Status:** Approved
**Supersedes:** the React + Vite + Django + MySQL stack described in `ProjectDocs.md` (that stack was never built in this repository)

---

## 1. Purpose

A single-page marketing and booking website for LA Tantra Massage, a doorstep
massage and wellness service operating in Chennai, Pondicherry, Trichy and
Bangalore.

**Goal:** generate booking requests. Availability is confirmed manually by a
human afterwards. The website never promises a confirmed slot.

**Audience:** adults (18+) seeking a discreet, professional, at-home or
in-hotel wellness session.

## 2. Scope

One primary route (`/`). All marketing and booking content lives on that page,
and navigation links scroll to anchors within it rather than loading new
documents.

Three additional minimal routes exist solely so the footer's legal links
resolve: `/privacy`, `/terms`, `/boundaries`. These are plain text pages with
no shared navigation complexity, and they are not part of the single-page
experience.

Section order, following `UI-Mockup/code.html` and `UI-Mockup/screen.png`:

| # | Section | Anchor | Source of truth |
|---|---------|--------|-----------------|
| 1 | Header (sticky) | — | mockup |
| 2 | Hero | `#home` | mockup |
| 3 | Trust bar (18+, consent-led, boundaries, doorstep, 4 cities) | — | mockup |
| 4 | How It Works (3 steps) | `#how-it-works` | mockup |
| 5 | Services | `#services` | mockup + `ProjectDocs.md` |
| 6 | About / philosophy | `#about` | content TBD by owner |
| 7 | Locations | `#locations` | `ProjectDocs.md` |
| 8 | Reviews | `#reviews` | mockup |
| 9 | **Booking form** | `#booking` | this document |
| 10 | FAQ | `#faq` | content TBD by owner |
| 11 | Contact / closing CTA | `#contact` | mockup |
| 12 | Footer (incl. policy links) | — | mockup |

### Explicitly out of scope

- Blog and blog detail pages
- Individual service detail pages
- Online payment (payment is taken after service)
- Customer accounts or login
- Any admin dashboard — the Google Sheet *is* the admin view
- Multi-language support

### Blocked on the owner, recorded in `todo.md`

The *text* of the legal pages (Privacy Policy, Terms, Boundaries & Consent) is
a content task for the business owner, not a build task. The routes are built
with placeholders; the wording must be supplied before launch.

## 3. Architecture

```
┌──────────────────────────────────────────────┐
│ Browser — single Next.js page                │
│  Server Components render all static content │
│  One Client Component: the booking form      │
└───────────────┬──────────────────────────────┘
                │ POST /api/booking  (same origin)
                ▼
┌──────────────────────────────────────────────┐
│ Server (Vercel Function)                     │
│  1. Zod re-validation                        │
│  2. Spam checks: honeypot + time trap + rate │
│  3. Build booking reference                  │
└──────┬──────────────────────────┬────────────┘
       ▼                          ▼
  Google Sheets API          Resend (email)
  (private sheet,            owner alert
   service account)
```

**Rendering.** Every section except the booking form is a React Server
Component: no JavaScript is shipped for content the visitor only reads. The
form is the single `"use client"` island.

**Trust boundary.** The browser never holds Google or Resend credentials and
never calls those APIs. It talks only to our own origin.

### Module boundaries

Each unit has one job and can be tested alone.

| Module | Responsibility | Depends on |
|--------|----------------|------------|
| `content/services.ts` | Service catalogue data | nothing |
| `content/site.ts` | Cities, FAQs, reviews, contact details | nothing |
| `lib/booking-schema.ts` | Zod schema + inferred types | zod |
| `lib/booking-reference.ts` | Generate `LT-YYMMDD-XXXX` | nothing |
| `lib/sheets.ts` | Append one row; knows the column order | googleapis, env |
| `lib/email.ts` | Send owner alert; knows the template | resend, env |
| `lib/env.ts` | Validate env vars at startup, fail loudly | zod |
| `app/api/booking/route.ts` | Orchestrate the above | all of the above |
| `components/sections/*` | One file per page section | content modules |

`lib/sheets.ts` and `lib/email.ts` each expose a single async function and
know nothing about HTTP. That is what makes the route handler testable with
both of them faked.

## 4. Design system

`UI-Mockup/DESIGN.md` frontmatter is structured data and ports directly into
the Tailwind theme. No colour, font size, radius or spacing value may be
hardcoded in a component — every one is referenced through a token name
(`bg-surface`, `text-headline-lg`, `rounded-lg`, `p-space-6`).

Fonts: **EB Garamond** for headings, **Manrope** for body and controls,
loaded via `next/font` so they self-host rather than calling Google Fonts at
runtime.

Prices render with the Indian rupee symbol and Indian comma grouping
(`₹1,499`), via one shared formatter, never hand-typed.

## 5. Booking data model

### Form fields

| Field | Required | Validation |
|-------|----------|------------|
| Name | yes | 2–80 characters |
| Phone | yes | Indian mobile, normalised to `+91XXXXXXXXXX` |
| Service | yes | must be an ID from the catalogue |
| Duration | yes | `60` or `90` minutes |
| Preferred date | yes | today or later, within 60 days |
| Preferred time window | yes | fixed set of windows |
| City | yes | must be a served city |
| Visit type | yes | `home` or `hotel` |
| Area / locality | yes | 2–120 characters — **not** the full address |
| Notes | no | max 500 characters |
| `website` (honeypot) | must be empty | hidden from humans |
| `renderedAt` (time trap) | — | reject if submitted < 3s after render |

**Deliberate omission: we do not collect the full street address.** The
booking is a *request*; the exact address is taken during the human WhatsApp
or phone confirmation, once the booking is real. Locality is enough to judge
travel feasibility. This meaningfully reduces what is sitting in a
spreadsheet.

### Google Sheet columns

Sheet name: `Bookings`. Row appended in this exact order:

```
Timestamp (IST) | Booking Ref | Name | Phone | Service | Duration |
Quoted Price | Preferred Date | Preferred Time | City | Visit Type |
Area | Notes | Status | Owner Notes
```

`Status` is written as `NEW`. `Owner Notes` is written empty. Both columns
exist so the owner can update them by hand in the spreadsheet; the
application only ever appends and never reads or rewrites rows.

`Quoted Price` is a snapshot of the price at the time of booking, so a later
price change does not rewrite history.

## 6. Error handling

The route handler returns one of three outcomes:

| Situation | Sheet | Email | Visitor sees |
|-----------|-------|-------|--------------|
| Normal | written | sent | Confirmation + booking ref + WhatsApp button |
| Sheet fails | failed | sent, subject prefixed `⚠️ SHEET WRITE FAILED` | Normal confirmation |
| Email fails | written | failed | Normal confirmation |
| Both fail | failed | failed | Error: "please WhatsApp us directly", with the number |

The email carries the complete booking details in its body, so it is a
standalone record. That is why a Sheet failure alone is not a visitor-facing
error: nothing has been lost.

**Validation failures** return field-level messages that the form displays
next to the offending input. **Spam rejections** return the ordinary success
message — telling a bot it was detected only helps the bot.

**Logging never includes form contents.** Logs carry the booking reference,
the outcome, and an error code. Never a name, phone number or locality.

## 7. Security and privacy

- The spreadsheet is shared with the service account's email address **only**.
  It is never set to "anyone with the link".
- The service account is granted the `spreadsheets` scope and is added to one
  spreadsheet, not to a Drive folder.
- All secrets live in environment variables. `.env.local` is gitignored. No
  credential is ever committed.
- `lib/env.ts` validates required variables at startup so a missing secret
  fails immediately and visibly rather than at the moment a customer submits.
- Security headers are set: `Content-Security-Policy`,
  `Strict-Transport-Security`, `X-Content-Type-Options`, `Referrer-Policy`.
- Rate limiting on `/api/booking` by IP.
- The site states 18+ throughout, per the existing mockup and `ProjectDocs.md`.

## 8. Accessibility

Target WCAG 2.1 AA.

- Every section is a landmark with a heading; a skip link precedes the header.
- Anchor navigation moves focus to the target section, not just the scroll
  position — otherwise keyboard users are left behind.
- Form inputs have real `<label>` elements; errors are linked via
  `aria-describedby` and announced in a live region.
- The design's teal on cream is checked against contrast requirements; the
  gold accent is decorative only and never the sole carrier of meaning.
- `prefers-reduced-motion` disables scroll animation and transitions.

## 9. Testing

| Level | Tool | Covers |
|-------|------|--------|
| Unit | Vitest | schema, phone normalisation, price formatting, booking reference, sheet-row builder |
| Integration | Vitest | `/api/booking` with Google and Resend faked: happy path, sheet-down, email-down, both-down, invalid input, honeypot filled |
| E2E | Playwright | fill form → confirmation; every nav anchor resolves to an existing section; mobile viewport renders |

Target: 80%+ coverage, with the booking path at 100%. Written test-first.

## 10. Content ownership

Service names, durations and prices come from `ProjectDocs.md` and are
transcribed, not invented:

| Service | Duration | Price | Note |
|---------|----------|-------|------|
| Thai Massage | 60–90 min | ₹999 | |
| Full Body Massage | 60–90 min | ₹999 | |
| Neuro Massage | 60–90 min | ₹999 | |
| Tantra Massage | 60–90 min | ₹1,499 | |
| Yoni Massage | 60–90 min | ₹1,499 | adult women |
| Breast Tissue Massage | 60–90 min | ₹1,499 | adult women |

Served cities: Chennai, Pondicherry, Trichy, Bangalore.

**Content the owner must supply before launch** — these are marked `TODO` in
`ProjectDocs.md` and must not be invented: real WhatsApp number and email
address, operating hours and last booking time, cancellation and no-show
policy, travel or late-night fees, therapist gender rules, health
restrictions, the About text, the FAQ answers, and whether the mockup's
testimonials are real. Placeholders in the codebase are marked
`TODO(owner):` so they are greppable.

The mockup's `concierge@latantra.com` and its three testimonials are
**mockup placeholders** and are treated as unconfirmed.

## 11. Delivery phases

1. **Foundation** — scaffold, design tokens, fonts, tooling, CI
2. **Content layer** — typed content modules, formatters, unit tests
3. **Static sections** — all twelve sections, responsive, accessible
4. **Booking pipeline** — schema, Sheets, email, route handler, tests
5. **Booking UI** — the form, states, confirmation
6. **Hardening** — SEO, security headers, rate limiting, Lighthouse, E2E
7. **Launch** — Vercel, domain, owner content, final review

Each phase ends in a working, deployable state.
