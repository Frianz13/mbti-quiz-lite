# MBTI Quiz Lite

A short MBTI-style quiz built on everyday situations, by [Frianz13](https://github.com/Frianz13).
One file, no frameworks, no tracking. Works offline once loaded.

**Live:** https://frianz13.github.io/mbti-quiz-lite/ (once GitHub Pages is switched on: Settings > Pages > Deploy from branch > `main` / root)

## How it works
- 28 situations as shipped, 7 per axis (E/I, S/N, T/F, J/P). Each one has two statements, A and B, and a 5-point scale between them.
- Which pole sits on the left is mixed on purpose, so always picking A (or always B) lands near the middle instead of on one type.
- Each axis is shown as a percentage. Within 10 points of 50 it counts as a close call: both letters are shown in a dashed box. If all four axes are close, the page says no single type fits cleanly instead of guessing.
- All 16 types are ranked by fit (the four axis shares multiplied together). #1 is shown big with its description; the rest are tap-to-open rows, top 5 first and "Show all 16" for the rest. It is a fit ranking, not a probability.
- Responsive by screen width: under 720px it is one column (A over B). From 720px up, A and B sit side by side and the result splits into two columns. Devices with a mouse also see a shortcut hint (keys 1 to 5 answer, Backspace goes back).
- Nothing is stored or sent anywhere.

## Edit the questions
Open `index.html`. Questions are in the `QUESTIONS` array at the top of the script:

```js
{ axis: "EI", leftPole: "E", situation: "Free Saturday, no plans.",
  left: "You message people to see who's around.",
  right: "You're relieved and keep it that way." }
```

- `leftPole` is the letter the LEFT statement points to. The right one points to the opposite letter.
- Keep the same number of items on every axis, and mix the left poles (roughly half of each axis pointing each way). That mix is what stops a habit of always picking one side from pushing the result toward one type.
- The question count, the progress bar and the time estimate (about 10 seconds per item) all follow the number of items, so you can add or remove items without touching the rest. Ties can still happen and the page handles them.
- Descriptions for the 16 types are in the `TYPES` object right below.

## Files
- `index.html` is the whole app.

MBTI is a fun lens for self-reflection, not a scientific diagnosis.
