# Debugging methodology

After a particularly frustrating debugging session today (four hours for a bug that turned out to be a timezone assumption), writing down the methodology I know I should follow but often don't.

## The method

**1. Reproduce it first.**
Don't look at code until you can reproduce the bug reliably. Debugging unreproducible bugs is guessing with extra steps.

**2. State your hypothesis explicitly.**
Write down: "I think the bug is happening because X." This forces specificity. If you can't state it precisely, you don't have a hypothesis yet.

**3. Design a test that would falsify the hypothesis.**
If X is the cause, then Y should be observable. If Y is not observable, X is not the cause.

**4. Run the test. Only one variable at a time.**
The instinct to change multiple things simultaneously is strong and wrong. You can't know which change fixed it.

**5. Update your beliefs based on evidence.**
If the test fails your hypothesis, abandon the hypothesis. Don't rescue it.

## Where I went wrong today

I had a hypothesis at hour 1 (timezone issue in the date parsing). I then convinced myself that wasn't it because "we use UTC everywhere." This belief was false — the third-party service we call uses local time in some responses. Hours 2-4 were wasted because I was testing hypotheses downstream of a wrong assumption I wouldn't let go of.

The principle I violated: [David Dunning on calibration](https://www.psychologytoday.com/us/basics/dunning-kruger-effect). Specifically, the confidence I had in "we use UTC everywhere" was not proportional to my actual evidence for it.
