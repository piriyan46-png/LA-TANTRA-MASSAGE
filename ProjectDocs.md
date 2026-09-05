# Project
- Business name: LA Tantra Massage
- What this website is: A responsive website for professional doorstep massage and wellness services, including home and hotel visits.
- Live URL / domain: TODO
- One-line goal: Generate massage booking requests and manually confirm availability through WhatsApp.

# Offerings
- All massage / spa services:
  - Thai Massage — duration: 60–90 min; price: ₹999; who it is for: TODO; description: Traditional massage using guided movement, pressure techniques and assisted stretching.
  - Full Body Massage — duration: 60–90 min; price: ₹999; who it is for: TODO; description: Full-body wellness session focused on comfort and relaxation.
  - Neuro Massage — duration: 60–90 min; price: ₹999; who it is for: TODO; description: Wellness-focused massage approach centred on relaxation and body awareness.
  - Tantra Massage — duration: 60–90 min; price: ₹1,499; who it is for: TODO; description: Consent-based wellness experience focused on mindful relaxation and energy awareness.
  - Yoni Massage — duration: 60–90 min; price: ₹1,499; who it is for: Adult women; description: Private wellness service with explicit consent and professional boundaries.
  - Breast Tissue Massage — duration: 60–90 min; price: ₹1,499; who it is for: Adult women; description: Private wellness service focused on comfort, relaxation and professional communication.
  - Database-managed service list and final prices: TODO
- Packages, combos, memberships, gift cards: TODO
- Add-ons: oils, hot stone, aromatherapy, couples, etc. + extra price: TODO
- In-centre, home visit, hotel visit: Home and hotel visits are supported; in-centre availability: TODO
- Extra fees: travel, late night, home visit: TODO

# Features
- Pages and what each page does: Home; Services; service details; About; Locations; location details; Reviews; Blog; blog details; FAQ; Contact; Booking; Trust; Boundaries; policy pages; Not Found.
- Service listing: Yes; services can be loaded from the backend with frontend fallback data.
- Pricing display: Yes; prices and durations are shown on service cards and details.
- Booking form / WhatsApp booking / phone booking: Booking form submits a booking request to the backend; final availability is manually confirmed through WhatsApp; phone booking: TODO.
- Contact, map, click-to-call, click-to-WhatsApp: Contact links and click-to-WhatsApp are present; map and click-to-call: TODO.
- Gallery / testimonials / FAQ / offers: Reviews/testimonials and FAQ pages are present; gallery and offers: TODO.
- Mobile layout: Responsive mobile layout is present.
- Language options: TODO
- Admin / backend, if any: Django admin manages services, locations, bookings, reviews, blog posts, FAQs, policies, trust standards, boundaries and site settings.
- Features planned later: TODO
- Features we do not want: TODO

# Business rules
- Hours and last booking time: TODO
- Walk-in vs appointment only: Prior appointment; walk-ins: TODO
- Male / female clients accepted: TODO
- Therapist gender rules: TODO
- Areas served / not served: Chennai, Pondicherry, Trichy and Bangalore are listed; other locations may be considered by prior arrangement and travel availability; exact served/not-served areas: TODO
- Deposit, payment methods, cancellation, reschedule, no-show: No advance payment in the current FAQ; payment is after service; all other rules: TODO
- Age limit and health restrictions worth showing on the site: Services are strictly for adults aged 18 and above; health restrictions: TODO

# Brand and content
- Tone: Calm, private, professional, wellness-focused and consent-led.
- Colors, fonts, logo: Colors include dark ink, cream/paper, teal, sage and gold; headings use Cormorant Garamond and body text uses Manrope/system UI; logo: TODO
- Words to use / avoid: Use professional wellness, consent, privacy, comfort and boundaries; words to avoid: TODO
- Important finalized text: headline: TODO; about: TODO; policies: TODO; WhatsApp message: TODO

# Tech
- Stack, hosting, domain: React 19 + Vite frontend; React Router; Django 5.2 + Django REST Framework backend; MySQL database; local development uses Vite and Django; hosting and domain: TODO
- How booking is stored or sent: Booking requests are stored in the Django/MySQL backend with status and payment status; WhatsApp is used for manual availability confirmation.
- File / page list if known: frontend/src/App.jsx, frontend/src/App.css, frontend/src/index.css, frontend/src/main.jsx; backend/config; backend/premium_wellness; frontend routes are documented under Features.

# This phase
- Finalized changes to do now: TODO
- Things not to change: Do not invent prices, services, policies, hours, areas, contact details or availability rules; preserve consent, adult-only and professional-boundary wording unless finalized changes say otherwise.

# Critical notes
- Business facts in the backend/database may override frontend fallback content; verify before changing displayed services or prices.
- Booking submission creates a request and does not itself confirm availability.
- Do not publish private identity-verification documents or sensitive document numbers.
- TODO: Confirm all legally and operationally sensitive business rules before publishing.
