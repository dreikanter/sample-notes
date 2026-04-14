---
title: Meeting notes — design review 22 May
slug: meeting-notes-design
tags: [work, meetings, design]
---

# Meeting notes — design review 22 May

Attendees: myself, Lena, Priya, a product manager from the mobile team (Ibrahim) joining for the first half.

**Search redesign — mobile filter panel**

Lena presented two options for the mobile filter panel:

*Option A (bottom sheet):* Filters open in a bottom sheet overlaying the results. Filter state is preserved if you close and reopen. Swipe-to-dismiss. Standard pattern for iOS/Android.

*Option B (full-screen overlay):* Takes over the screen entirely. Cleaner for complex filter sets. More disorienting as a navigation step.

General preference for Option A. Ibrahim noted that their analytics show most mobile users apply 1–2 filters, not the full set — so the bottom sheet's quick-access affordance is a better fit for actual use patterns. I raised the question of what happens when filters overflow the sheet height — Lena said she'll add an internal scroll on the filter list.

**Keyboard navigation spec**

I walked through the spec I wrote. Two rounds of feedback:

1. The focus trap in the filter panel needs an explicit escape path — not just Escape key but also Back button on mobile. Added.
2. The "clear all filters" button should be focusable separately, not just triggered via the reset shortcut. Makes sense.

**Action items**

- Lena: update designs with scrollable filter list (by 26 May)
- Me: update spec for escape path and "clear all" focus (today)
- Priya: schedule session with data team on analytics naming (open)

[WCAG keyboard navigation requirements](https://www.w3.org/WAI/WCAG21/Understanding/keyboard.html)
