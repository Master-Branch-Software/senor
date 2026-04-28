# Response: email-microcopy-brand-voice (WITH skill)

_Skill loaded: SKILL.md. Primary chapter loaded: 02-brand-and-copywriting.md._

---

## Design decisions before writing

**Tone ladder — churn/cancellation context:**
Chapter 02 specifies: mid formality, warm, low urgency, no play, never guilt.

Applied rules:
- Subject line states the outcome, not the sender.
- Subject ≤ 50 characters (mobile truncation threshold is ~40).
- Body leads with the reason the email was sent.
- One action per email.
- Names the specific change (Pro → Free).
- States what ends and when.
- States what is preserved.
- Offers one action (resubscribe or contact support).
- Zero guilt language ("we hate to see you go," "are you sure?", "we'll miss you").
- Signed from a named human — churn is relationship-sensitive; a named sender signals that a real person is aware of the change.
- Brand vocabulary: root cause, signal, noise, on-call, ship, incident. Avoid: unleash, empower, revolutionize, AI-powered, game-changer.

---

## Email

**Subject line** (38 characters):
```
Your Flint plan is now Free
```

**Preheader** (extends subject, does not repeat it):
```
Pro features end today. Your data stays.
```

---

**From**: Alex at Flint `<alex@flint.dev>`

---

Hi [First name],

Your plan changed from Pro to Free today.

Here's what that means:

**What stays**: All your error history, groupings, and project data remain exactly as they were. Nothing is deleted.

**What ends today**: Pro features — unlimited team members, custom alert rules, advanced filters, and the weekly digest — are no longer active on your account.

If you need those back, you can resubscribe at any time. Your previous settings will be there when you do.

[Resubscribe to Pro →]

Questions? Reply to this email or reach us at [support@flint.dev](mailto:support@flint.dev).

— Alex

---

**Why each decision was made:**

- _"Your plan changed from Pro to Free today"_ — Leads with the reason the email was sent, per the transactional email rule in chapter 02. No warm-up sentence.
- _"What stays" / "What ends today"_ — Scannable structure that answers the two questions every downgrading user has, in that order: "Will I lose my data?" (most anxious) then "What can I not do anymore?"
- _"Nothing is deleted"_ — The word users actually fear during a plan change. Naming it removes it.
- _"You can resubscribe at any time. Your previous settings will be there"_ — Single resubscribe offer; no urgency, no guilt. The reassurance about settings is a retention signal without pressure.
- _"Questions? Reply to this email"_ — Lowest-friction support path for a relationship-sensitive email.
- Signed from a named human (Alex) — per chapter 02's rule that relationship-sensitive emails are signed from a person, not from a brand.
- Zero uses of: unleash, empower, revolutionize, AI-powered, game-changer, "we'll miss you," "are you sure?", "we hate to see you go."
- Warm tone without being cloying. No exclamation marks. Mid formality.

---

_Self-review: tone ladder position confirmed (mid formality, warm, low urgency, no play, no guilt). One action per email. Subject states outcome, 38 characters. Body leads with reason sent. No deviations from chapter 02 or the brand voice specified in the prompt._
