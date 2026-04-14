# Meeting notes — product sync 21 April

Attendees: myself, Priya, Tomás, Lena (remote)

**Agenda items covered**

1. Q2 roadmap priorities
2. Search redesign scope
3. Mobile performance audit results

**Q2 roadmap**

Agreement to deprioritise the notification centre rework until Q3. Not enough engineering capacity alongside the search redesign. Tomás flagged that the notification codebase is tangled enough that any meaningful change needs a larger block of time than we can carve out this quarter.

**Search redesign**

Lena presented the Figma designs. General positive reaction. Key open questions:
- What happens to the advanced filter panel on mobile? Currently no design for that state.
- Keyboard navigation spec not yet written — needs to be done before engineering starts.
- Analytics events for the new flow — need to agree naming convention with the data team.

Priya to follow up with data team on analytics naming. Lena to produce mobile filter designs by 28 April. I'm blocking out time to write the keyboard nav spec next week.

**Mobile performance audit**

Results came back from Lighthouse run on production. Worst offenders are the dashboard page (LCP 4.8 s on mid-tier Android) and the export modal (renders-blocking JS). Both are known issues but this gives us numbers to prioritise against.

Related reading: [web.dev guide to LCP optimisation](https://web.dev/lcp/)

**Next sync:** 5 May, same time.
