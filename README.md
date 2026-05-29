# AiTHOS Health Passport — Parent SAiRA Flow

The parent's side of the teen Health Passport signup. The parent receives a **text message** with a short opener and a link. Clicking the link drops them into this SAiRA-mediated landing page, which mirrors the teen experience: SAiRA walks the parent through the two things the teen needs from them — co-signing the Health Fiduciary Agreement, and adding the details only they know.

Companion artifact to:

- The teen acquisition page (Health Passport landing)
- The Healthograph Lite demo (concept artifact)
- The Phase 1 build architecture (separate document family)

## The text message that opens this flow

The parent receives an SMS from AiTHOS, sent on the teen's authorization at the end of the teen's signup. The text is intentionally short — designed to be tapped, not read.

```
AiTHOS · Health Passport

Maya just took her first step toward owning her health. She built her Passport and asked us to loop you in.

Continue → https://aithos.example/p/HP-M7K2-2026
```

**Why a text instead of an email.** Parents check texts faster, the medium feels more personal, and a single-tap CTA on a mobile device has a dramatically higher conversion rate than email click-through. The previous email-based version of this artifact (in the prior repo) remains available as a longer-form alternative — useful for parents who prefer email or who don't share a mobile carrier with the teen — but the SMS is the primary channel for the teen-led adoption model.

**Why this exact phrasing.** Every word is calibrated to the "first response is pride, not evaluation" principle. The text:

- Names Maya specifically
- Frames the action as a step forward, not a request
- Uses "owning her health" rather than "health records" — identity, not paperwork
- Does **not** contain the words *permission*, *consent*, *approval*, *agreement*, *sign*, or *verify*. Any of those would trip the parent into gatekeeper mode before they tap.
- Closes with a single tap action and the Passport ID embedded in the URL

The teen authorizes AiTHOS to send this text during the SAiRA flow on the teen page. The phone number was captured as part of the emergency-contact step.

## What this repo contains

- `index.html` — the parent SAiRA landing page (single-file static; HTML, CSS, and JS inline)
- `README.md` — this file

Deploy via GitHub Pages from the repo root. No build step.

## The flow this page implements

When the parent taps the SMS link, they land on this page. The teen's Passport — Maya's, fully populated with what she filled in — is visible above the fold in the same metallic AMEX-style finish as the teen page. The opening copy reads:

> **Maya took her first step toward owning her health.**
>
> She built this with SAiRA — our AI guide. She accepted a Health Fiduciary Agreement that says AiTHOS works for **her**, not for clinics or insurers. She wanted you to see it.

The parent sees the Passport at 83% complete — the teen's own contribution — with the remaining 17% reserved for what the parent will add. The CTA is "Continue with SAiRA →".

Once the parent taps Continue, SAiRA takes over:

1. **Greeting and parent's name** — SAiRA introduces herself and confirms what to call the parent.
2. **Two-things-needed intro** — joining the Fiduciary Agreement, and adding what only they know. Parent can choose "Let's do it" or "Tell me more first." If "tell me more," SAiRA explains both items with a "Got it" option to proceed and a "Read the full agreement first" option that opens the agreement text.
3. **Fiduciary Agreement co-signature** — shown inline as a clean block of agreement language. Framed as *joining* Maya, not approving for her. Options: *I join* / *Not yet*. If declined, soft re-prompt; if joined, proceeds.
4. **Pediatrician** — text input.
5. **Immunizations** — choice options (Pediatrician's office / State registry / Both / Not sure).
6. **Insurance carrier** — text input.
7. **Family health history** — categorical choice (Yes, some / None notable / I'll add later).
8. **Emergency permissions** — categorical (Yes, authorize / Not now).
9. **Completion** — the Passport moves to 100% complete and is activated. Two CTAs: "Send Maya the Apple Wallet link" (primary, Maya-centric) and "Want one of your own? →" (secondary, quiet parent-conversion option).

Each step pulses the Passport card, and the completeness percentage rises as each field is filled. Every text input includes a "Skip for now" option so the parent can complete the flow without every detail.

## Visual continuity

Same brand system as the teen page: navy background with gradient mesh, AMEX-style metallic Passport card (brushed-steel platinum-to-gunmetal with specular highlight), Calibri/Georgia type pairing, aqua accent color, the SAiRA chat-bubble pattern, the pulsing Signal dot. The two pages are deliberately the same product, viewed from the two sides of the family.

## Template variables for production

When deploying to a real domain, the static demo values become injected variables:

| Variable | Demo value | Source |
|---|---|---|
| Teen first name | `Maya` | From teen's signup |
| Teen ID | `HP-M7K2-2026` | Generated at teen signup |
| Passport state (allergies, meds, emergency) | All `None` / `Linked` | From teen's signup |
| Starting completeness | `83%` | Teen's contribution baseline |
| Parent name | (asked by SAiRA) | Captured live |
| CTA destinations | `https://aithos.example/...` | Production endpoints |

The Passport ID in the URL identifies which family record this is. The page fetches Maya's current draft state when the parent lands. (The prototype hardcodes this — in production, server-side rendering or a fetch-on-load is needed.)

## What's prototype-only

- The Apple Wallet link is a placeholder modal.
- The "Want one of your own?" CTA opens a placeholder modal.
- The full agreement text behind the "Read the full agreement" link is a placeholder. **Real implementation requires the counsel-reviewed Parent-as-Co-Principal Health Fiduciary Agreement (Phase 1, Minor Cohort Edition).**
- No data persistence — state lives in memory only.
- No real backend.

## Production checklist

Before this page goes to a real family:

- [ ] Pediatric digital-health counsel review of the Fiduciary Agreement co-signature language
- [ ] The Parent-as-Co-Principal Health Fiduciary Agreement exists in counsel-reviewed form and is linked correctly
- [ ] SMS delivery infrastructure (Twilio, MessageBird, or comparable) integrated
- [ ] Passport ID lookup and state injection working server-side
- [ ] Audit trail: every step the parent takes is logged with timestamp, scope, and the parent's identity (verified against the teen's emergency-contact entry)
- [ ] Notification back to Maya when the parent completes — confirming she's part of the loop
- [ ] Edge-case handling: stepparent / grandparent / non-guardian, parent who already has an AiTHOS Passport, teen turning 18 mid-flow

## What's deliberately not in this page

Same disciplines as the email version:

- No urgency framing
- No fear framing
- No marketing voice
- No high-pressure conversion CTA
- The "Want one of your own?" footer is muted, optional, and italic — it observes that many parents arrive at their own AiTHOS interest organically; it does not push them there

The parent's first response is pride. The parent's contribution is enrichment, not validation. Maya is the protagonist; the parent is the audience their protagonism deserves.

## License & confidentiality

Confidential concept material prepared by Cadence Cove LLC for internal review and discussion with partners under appropriate confidentiality terms.
