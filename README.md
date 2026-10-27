# MBTI Quiz Lite

A short MBTI-style quiz built on everyday situations, by [Frianz13](https://github.com/Frianz13).
One file, no frameworks, no tracking. Works offline once loaded.

**Live:** https://frianz13.github.io/mbti-quiz-lite/ (once GitHub Pages is switched on: Settings > Pages > Deploy from branch > `main` / root)

## How it works
- 28 situations, 7 per axis (E/I, S/N, T/F, J/P). Each one has two statements and a 5-point scale between them.
- Left/right is mixed on purpose, so clicking "agree" on everything lands you in the middle instead of on one type.
- Each axis is shown as a percentage. Within 10% of 50 it counts as a close call and both letters are shown.
- All 16 types are ranked by fit (the four axis shares multiplied together). #1 is shown big with its description; the rest are tap-to-open rows, top 5 first and "Show all 16" for the rest. It is a fit ranking, not a probability.
- Nothing is stored or sent anywhere.

## Edit the questions
Open `index.html`. Questions are in the `QUESTIONS` array at the top of the script:

```js
{ axis: "EI", leftPole: "E", situation: "Free Saturday, no plans.",
  left: "You message people to see who's around.",
  right: "You're relieved and keep it that way." }
```

- `leftPole` is the letter the LEFT statement points to. The right one points to the opposite letter.
- Keep 7 items per axis (an odd number avoids exact ties). The counts everywhere else update themselves.
- Descriptions for the 16 types are in the `TYPES` object right below.

## Files
- `index.html` is the whole app.
- `PLAN.md` is the brief it was built from.
- `original/MBTI_Quiz.html` is the first version, kept unchanged for comparison.

MBTI is a fun lens for self-reflection, not a scientific diagnosis.
