---
name: Sanctuary Quiet Luxury
colors:
  surface: '#fcf9f8'
  surface-dim: '#dcd9d9'
  surface-bright: '#fcf9f8'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#f6f3f2'
  surface-container: '#f0eded'
  surface-container-high: '#eae7e7'
  surface-container-highest: '#e5e2e1'
  on-surface: '#1c1b1b'
  on-surface-variant: '#3f4946'
  inverse-surface: '#313030'
  inverse-on-surface: '#f3f0ef'
  outline: '#6f7976'
  outline-variant: '#bec9c5'
  surface-tint: '#0f6a5e'
  primary: '#006357'
  on-primary: '#ffffff'
  primary-container: '#2a7c6f'
  on-primary-container: '#cbfff3'
  inverse-primary: '#87d5c5'
  secondary: '#44664a'
  on-secondary: '#ffffff'
  secondary-container: '#c3e9c5'
  on-secondary-container: '#486a4e'
  tertiary: '#6c540f'
  on-tertiary: '#ffffff'
  tertiary-container: '#876c27'
  on-tertiary-container: '#fff3df'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#a3f1e1'
  primary-fixed-dim: '#87d5c5'
  on-primary-fixed: '#00201b'
  on-primary-fixed-variant: '#005046'
  secondary-fixed: '#c6ecc8'
  secondary-fixed-dim: '#aad0ad'
  on-secondary-fixed: '#00210b'
  on-secondary-fixed-variant: '#2d4e33'
  tertiary-fixed: '#ffdf99'
  tertiary-fixed-dim: '#e5c374'
  on-tertiary-fixed: '#251a00'
  on-tertiary-fixed-variant: '#5a4300'
  background: '#fcf9f8'
  on-background: '#1c1b1b'
  surface-variant: '#e5e2e1'
typography:
  display-lg:
    fontFamily: EB Garamond
    fontSize: 56px
    fontWeight: '400'
    lineHeight: 64px
    letterSpacing: -0.02em
  display-lg-mobile:
    fontFamily: EB Garamond
    fontSize: 38px
    fontWeight: '400'
    lineHeight: 46px
    letterSpacing: -0.01em
  headline-lg:
    fontFamily: EB Garamond
    fontSize: 40px
    fontWeight: '400'
    lineHeight: 48px
    letterSpacing: -0.01em
  headline-lg-mobile:
    fontFamily: EB Garamond
    fontSize: 28px
    fontWeight: '400'
    lineHeight: 36px
    letterSpacing: 0em
  headline-md:
    fontFamily: EB Garamond
    fontSize: 30px
    fontWeight: '500'
    lineHeight: 38px
  headline-sm:
    fontFamily: EB Garamond
    fontSize: 22px
    fontWeight: '500'
    lineHeight: 30px
  body-lg:
    fontFamily: Manrope
    fontSize: 18px
    fontWeight: '400'
    lineHeight: 28px
  body-md:
    fontFamily: Manrope
    fontSize: 15px
    fontWeight: '400'
    lineHeight: 24px
  body-sm:
    fontFamily: Manrope
    fontSize: 13px
    fontWeight: '400'
    lineHeight: 20px
  label-lg:
    fontFamily: Manrope
    fontSize: 14px
    fontWeight: '600'
    lineHeight: 20px
    letterSpacing: 0.04em
  label-md:
    fontFamily: Manrope
    fontSize: 12px
    fontWeight: '600'
    lineHeight: 16px
    letterSpacing: 0.06em
  currency-md:
    fontFamily: Manrope
    fontSize: 18px
    fontWeight: '500'
    lineHeight: 24px
    letterSpacing: -0.01em
rounded:
  sm: 0.25rem
  DEFAULT: 0.5rem
  md: 0.75rem
  lg: 1rem
  xl: 1.5rem
  full: 9999px
spacing:
  space-1: 0.25rem
  space-2: 0.5rem
  space-3: 0.75rem
  space-4: 1rem
  space-5: 1.25rem
  space-6: 1.5rem
  space-8: 2rem
  space-10: 2.5rem
  space-12: 3rem
  space-16: 4rem
  space-20: 5rem
  space-24: 6rem
---

## Brand & Style

This design system establishes an ultra-premium, intimate, and discreet atmosphere tailored for an adults-only doorstep wellness concierge. The visual language rejects spiritual clichés, synthetic gradients, and generic spa aesthetics. Instead, it channels the understated poise of a secluded five-star boutique hotel lobby at 9 PM: deep residential warmth, low-lit intimacy, textured quietude, and rigorous confidentiality.

The design philosophy unites **Quiet Luxury Minimalism** with **Warm Tactile Nuance**:
- **Discretion First:** Interfaces are unhurried, texturally refined, and free from garish marketing banners, aggressive countdown timers, or overt transactional pressure.
- **Architectural Grounding:** Generous negative space, fine hair-line dividers, and muted tonal shifts replace loud borders and dramatic shadows.
- **Sensory Resonance:** Layouts evoke linen, honed travertine, darkened teak, and brass accents without falling into skeuomorphic gimmickry.

## Colors

The palette is tuned to evoke organic warmth, sanctuary, and quiet assurance. 

### Core Palette Hierarchy
- **Canvas Base (`#FAF5EF` - cream):** The warm, non-reflective backdrop across light mode viewports.
- **Secondary Surface (`#F2E8DA` - cream-warm):** Used for nested cards, alternating tonal bands, and modal backdrops to provide depth without visual noise.
- **Primary Typography & Anchors (`#1A1A1A` - ink):** High-clarity editorial text and deep architectural footer foundations.
- **Secondary Typography (`#3D3D3D` - ink-soft):** Balanced contrast for editorial long-form reading, session descriptions, and fine print.
- **Primary Accent (`#2A7C6F` - teal):** Grounded deep mineral green used for booking actions, prominent text links, and key milestones.
  - Hover / Active state: `#1D5A50` (teal-dark)
  - Subtle highlight / badges: `#E8F5F2` (teal-light)
- **Secondary Accent (`#7A9E7E` - sage):** Organic botanical tone applied to service demarcation lines, tranquil icons, and supportive verification badges.
  - Soft container: `#E8F0E8` (sage-light)
- **Tertiary Accent (`#C5A55A` - gold):** Muted champagne brass reserved exclusively for private suite indicators, elite memberships, and discretion guarantee marks.
  - Premium surface fill: `#F5EDD6` (gold-light)

### Contrast & Application Rules
Never place raw black (`#000000`) or stark white (`#FFFFFF`) against cream canvas. Buttons using `#2A7C6F` must render white text (`#FAF5EF`) to achieve AAA contrast ratios. All monetary figures must appear in `ink` or `teal-dark`, retaining typographic dignity over price-tag commercialism.

## Typography

The type system blends the literary grace of classical editorial serif (`EB Garamond`) with the structural clarity of a geometric-humanist sans (`Manrope`).

### Rules & Styling
- **Headings & Display:** Rendered in `EB Garamond`. Headlines should remain conversational, calm, and evocative. Use italicized serif variations sparingly for emphasis (e.g., location markers like *Chennai*, *Pondicherry*).
- **Body & Controls:** Rendered in `Manrope` for immediate, legible consumption of rituals, intake expectations, and concierge steps.
- **Price Formatting:** Format all pricing with the Indian Rupee symbol (`₹`) accompanied by comma grouping per the Indian numbering convention (e.g., `₹12,500`). The rupee symbol matches the weight and baseline of the associated numerical figure in `Manrope`.
- **Labels & Microcopy:** High-tracking uppercase labels (`letter-spacing: 0.04em - 0.08em`) bring clarity to discrete badges, private arrival times, and protocol markers.

## Layout & Spacing

A strict 8px harmonic grid provides structured breathability. Negative space is employed as an active design element to slow the user down and communicate privacy and comfort.

### Layout System
- **Desktop (>= 1200px):** 12-column grid, max container width of `1160px` to maintain focused density, 32px gutters, dynamic side margins.
- **Tablet (768px - 1199px):** 8-column grid, 24px gutters, 32px page padding.
- **Mobile (< 768px):** 4-column fluid layout, 16px gutters, 20px page edge margins.

### Vertical Rhythm & Section Padding
- Standard marketing and service overview sections use `space-20` (80px) on mobile and `space-24` (96px) on desktop.
- Booking flows, concierge chat steps, and intake questionnaires use tighter vertical spacing (`space-10` to `space-12`) to preserve focus and linear completion.

## Elevation & Depth

Visual hierarchy uses tactile surface shifts and organic warm lighting rather than high-elevation drops or synthetic floating layers.

### Tonal Stratification
- **Level 0 (Canvas):** Base page background in `#FAF5EF` (cream).
- **Level 1 (Card & Module Resting):** `#F2E8DA` (cream-warm) or pure neutral white `#FFFFFF` layered over cream, bound by a 1px border tinted in `#7A9E7E` at 15% opacity or `#1A1A1A` at 6% opacity.
- **Level 2 (Hover & Active Surfaces):** Elevated slightly with a soft, diffused shadow tinted with warm umber instead of grey:
  `box-shadow: 0 8px 24px -4px rgba(26, 26, 26, 0.05), 0 2px 6px -1px rgba(26, 26, 26, 0.03)`.
- **Level 3 (Overlays, Drawer & Privacy Dialogues):** 
  `box-shadow: 0 20px 48px -8px rgba(26, 26, 26, 0.12)`.
  Backdrop filters apply an ambient blur (`backdrop-filter: blur(8px)`) over a 70% opacity `#FAF5EF` scrim to maintain connection with the previous view.

## Shapes

The shape vocabulary prioritizes organic, welcoming forms that balance architectural serenity with touchable comfort.

### Geometry Specifications
- **Small Controls (Buttons, Inputs, Badges):** 12px border-radius (`rounded-md`).
- **Cards & Content Panels:** 20px border-radius (`rounded-lg`), ensuring nested elements mirror the interior corner curve accurately.
- **Drawers & Concierge Modals:** 24px top border-radius for mobile bottom sheets.
- **Location Selector Pills & Chips:** Fully pill-shaped (`9999px`) to invite natural tap ergonomics.

## Components

### Buttons
- **Primary Action (Concierge Request / Booking):** Deep teal (`#2A7C6F`) fill, `#FAF5EF` typography, 12px radius, 14px uppercase/medium tracking. Padding: 16px 28px. On hover: darken to `#1D5A50` with subtle translateY(-1px).
- **Secondary Action (Service Exploration / Inquiries):** Transparent surface, 1px border in `#1A1A1A` (20% opacity), text in `#1A1A1A`. Hover shifts background to `#F2E8DA`.
- **Discreet Ghost:** Borderless, text in `#2A7C6F` with a soft 1px underline that expands smoothly on hover.

### City & Experience Selection Chips
- **Resting:** Pill shape (`9999px`), background in `#F2E8DA`, text in `#3D3D3D`, border: 1px solid transparent.
- **Selected (e.g., Chennai, Bangalore):** Background in `#E8F5F2`, text in `#2A7C6F`, border in `#2A7C6F` (40% opacity). Accompanied by a subtle sage dot indicator.

### Input Fields & Intake Forms
- Discretion-driven form components. Text fields have a 12px radius, padded at 14px 18px.
- Background: `#FFFFFF` or `#FAF5EF` with a 1px border in `#3D3D3D` at 15% opacity.
- Focus State: Border shifts to `#2A7C6F` with an ambient glow (`box-shadow: 0 0 0 3px rgba(42, 124, 111, 0.12)`). Label sits cleanly above in `Manrope` 12px semi-bold.

### Service & Experience Cards
- Encased in `#F2E8DA` with 20px radius and fine outline.
- Header displays treatment category in `EB Garamond` alongside a gold privacy insignia or duration badge (`#F5EDD6` fill, `#C5A55A` text).
- Clear inclusions, therapist protocol details, and unambiguous pricing (e.g., `₹15,000 / 90 mins`) anchored at the bottom-right in `Manrope` medium.

### Concierge Verification & Discretion Accordion
- Tailored for doorstep safety and privacy FAQs (ID screening, discreet entry, non-branded vehicle arrival).
- Clean hairline dividers in `#7A9E7E` (20% opacity).
- Smooth expansion with serene chevron rotation, avoiding harsh mechanical springs.