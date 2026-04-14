# Notes on memory and forgetting

Hermann Ebbinghaus mapped the forgetting curve in the 1880s by memorising nonsense syllables and testing himself at intervals. His main finding — that retention drops sharply in the first hour and then flattens — still holds in replications. The implication is obvious: review soon after encoding, not a week later.

Spaced repetition formalises this. The SM-2 algorithm, which powers Anki, schedules reviews so that each successful recall pushes the next interval out by a multiplier (default 2.5). After four or five correct answers the interval can stretch to months. The maths is simple but the discipline is not: the queue grows whenever you add cards faster than you clear them.

A few things I notice in my own practice:

- Cued recall (flashcards) is much harder than recognition, which explains why re-reading a textbook feels productive but produces little durable memory.
- Interleaving topics during study causes short-term confusion but better long-term retention — a counterintuitive tradeoff.
- Sleep consolidates memories; studying just before sleep seems to help, though the research on timing is messier than popular accounts suggest.

The concept of "desirable difficulties" (Robert Bjork's term) ties these threads together: retrieval practice, interleaving, and spacing all make the learning process feel harder while improving outcomes. The discomfort is the signal that something is being encoded.

See: [Ebbinghaus forgetting curve on Wikipedia](https://en.wikipedia.org/wiki/Forgetting_curve) and the [Anki SM-2 algorithm documentation](https://faqs.ankiweb.net/what-spaced-repetition-algorithm.html).
