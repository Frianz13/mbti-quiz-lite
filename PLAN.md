# MBTI Quiz by Frianz13 — build plan for Sonnet

**Owner:** Frianz (@Frianz13). **Planner:** Opus. **Builder:** Sonnet (building as of 27 Oct 2026).
**Target repo:** `Frianz13/mbti-quiz-lite` (created by her 27 Oct 2026).
**Starting point:** her existing `MBTI Quiz.html` (kept unchanged at `original/MBTI_Quiz.html`) (single file, ~1,600 lines). **Read by the planner on 27 Oct 2026; findings are in section 2.**

---

## 0. Decisions already made by her

| Question | Her answer |
|---|---|
| Language | **English only**, like the original |
| Look | **"UI lebih manusiawi"**. Outside references are welcome |
| Questions | Add more relatable ones, or replace (her call: optional) → **recommendation below: replace** |
| Pictures / art slot | **None.** She never asked for images. Pure UI and text, no image slots, no illustrations |

Her brief, verbatim: *"Modifnya biar gak jd kayak AI slop njir, terus tambahin pertanyaan2 yg lebih relate, atau ganti (opsional)."*

---

## 1. Keep from the current file

The structure is sound and should survive:
- Three screens: start → quiz → results.
- One question at a time, Previous / Next, progress bar.
- 5-point answer scale.
- Results with a per-axis breakdown and a description per type.
- Everything in one file, no dependencies. Good.

---

## 2. What is wrong with the current file (fix all of these)

### 2a. Scoring bugs — these produce wrong results, not just ugly ones
1. **Every item is keyed the same direction.** All 40 statements mean "agree = E / S / T / J". Nothing is reverse-keyed, so anyone who tends to click "Agree" drifts toward **ESTJ**, and anyone who tends to disagree drifts toward **INFP**. This is acquiescence bias and it is the biggest flaw in the quiz.
2. **Ties always fall to I, N, F, P** (`scores.E > scores.I ? 'E' : 'I'`). Answering "Neutral" to everything returns **INFP**.
3. **Items filed under the wrong axis.** In the E/I block: *"I tend to jump into tasks without spending much time planning ahead"* measures J/P, not E/I; *"action-oriented, quick results"* is not E/I either. In S/N: *"practical with money"* is not S/N. In T/F: *"motivated by achievements and competence"* is not T/F.
4. **The result bars are drawn wrong.** `.score-bar-fill.left` is anchored at `right: 50%` and given `width: percent1%`, so it can never visibly exceed half the track, and it always shows the first letter's share regardless of which side won.
5. **Hard-coded 40** in at least five places (`new Array(40)`, `index === 39`, `/ 40`, the HTML text "of 40"). Derive everything from `questions.length`.
6. **The scale labels never change.** `← Introversion / Extraversion →` is static HTML, so it shows on S/N, T/F and J/P questions too.

### 2b. Content problems
7. **Type names are 16Personalities' branding** ("The Logistician", "The Architect", "The Debater"...). Do not use them in a public repo. Write her own short names or use none.
8. **Role labels are wrong in places**, e.g. ISTJ is labelled "Analyst".
9. **Every description flatters.** No type has a single weakness. "Career Paths" lists stereotypes.

### 2c. "AI slop" markers in the UI and copy
- Tailwind indigo `#6366f1` + pink `#ec4899` gradients on the logo, title text, buttons, badges, progress bar and type letters.
- Dark slate background `#0f172a` with `#1e293b` cards: the default "AI dashboard" palette.
- Floating 🧠 logo with a bounce animation and glow shadow.
- Emoji everywhere: 📋 ⏱️ 🎯 on the start screen, 🤝 👍 😐 👎 ❌ on the answer buttons, 💪 🎯 🤝 on the results.
- Copy: *"Discover Your Personality Type"*, *"a powerful tool for understanding your personality preferences"*, *"Your Personality Type Is"*.
- Hover lifts (`translateY(-3px)`) on every element.

---

## 3. The new look: "human" means looking made by a person on purpose

### Recommended direction: **flat "sticker" / neo-brutalist UI that matches her art**
Her illustration style (memory chip): **flat solid colour, bold clean single-weight outlines, minimal shading, one solid background colour per piece.** A UI built on the same rules will look like hers, not like a template.

Neo-brutalism is the established web version of exactly that:
- **Solid fills, no gradients.**
- **Thick dark outlines** (2-3px) on cards and buttons.
- **Hard offset shadows** instead of blurry ones: `box-shadow: 4px 4px 0 #2b1d14;`. Button press = shadow shrinks to 0 and the button moves 4px.
- A **light background**, not dark.

Reference worth looking at: **Gumroad's site** (its 2021 redesign is the best-known neo-brutalist product UI). Borrow the principles, not the layout.

### Palette (starting point, she has the final say)
Taken from her own character's colours:
```
background   warm off-white      #FFF8EC
ink/outline  dark brown           #2B1D14
accent 1     warm yellow          #FCD98A
accent 2     soft pink            #F7B6C2
accent 3     muted blue           #9CC3E6   (one per axis is fine)
```

### Type
- System font stack is fine. If she wants character, **one** rounded or hand-drawn display font for headings only, embedded or with a system fallback so it still works offline.
- No gradient text, no letter-spacing tricks on the type code.

### Interaction
- The 5 answer buttons become **5 dots or 5 blocks on a line** between two statements (see 4), not a vertical list of emoji rows.
- One small, deliberate animation is fine (e.g. the result letters "stamping" in). Not on everything.

---

## 4. Questions: replace all 40 with relatable, two-sided items

Recommendation: **replace**, because fixing the keying (2a-1) and the mis-filed items (2a-3) touches nearly every question anyway.

### Format: two concrete statements, five points between them
```
Your group project chat was just created and it's dead silent.
  You send the first message.   ○ ○ ○ ○ ○   You wait for someone else to start.
```
- No "agree / disagree" at all. This **removes acquiescence bias by design**.
- **Randomise which pole sits on the left**, per item, in the data. Never all E on the left.
- Middle dot = "honestly, both".

### Rules for writing items
- **Behaviour in a concrete situation**, never self-image (*"Are you an introvert?"* is banned).
- **Neither side sounds better.** If one option reads as the "good person" answer, rewrite it.
- **One axis per item.** Check each item against only its own axis.
- **Balanced count:** suggest **7 per axis = 28 items** (about 5 minutes). Odd count per axis avoids exact ties.
- Settings her audience lives in: commuting, group chats, group assignments, deadlines, part-time work, gaming with friends, family gatherings, a friend asking for help, scrolling at night, an empty weekend.

### Example items (English, to set the register)
- **E/I:** *Free Saturday, no plans.* — "You message people to see who's around." vs "You're relieved and keep it that way."
- **S/N:** *A lecturer gives an assignment with no example.* — "You look for last year's version first." vs "You start your own take straight away."
- **T/F:** *A friend vents about a problem that's clearly their own fault.* — "You point out what went wrong." vs "You listen first and leave the fault for later."
- **J/P:** *The deadline is a week away.* — "You start chipping at it today." vs "You already know it's happening the night before."

### Data shape
```js
{ axis: "EI", situation: "...", left: "...", right: "...", leftPole: "E" }
```
All items in one array at the top of the script so she can edit text without touching logic.

---

## 5. Scoring and results: honest

- Each item gives -2..+2 toward `leftPole` / the opposite pole. Sum per axis, convert to a **percentage per pole**.
- **Tie rule:** if an axis is within ±10% of 50, say *"you're close to the middle here"* and show **both letters** for that axis (e.g. `IN?P` or `INT/FP`). Do not silently pick one.
- **Bars:** one horizontal bar per axis, filled from the winning side, labelled with both percentages.
- **Per type: 3-4 short, specific lines**, written in plain English, including **one honest weak spot**. No career lists. No borrowed names.
- **One-line footnote:** *"MBTI is a fun lens for self-reflection, not a scientific diagnosis."*
- No sign-up, no tracking, nothing sent anywhere.

---

## 6. Open questions

None. Everything is decided.

---

## 7. Technical constraints

- **Single `index.html`**, inline CSS and JS, no frameworks, no build step.
- **Mobile-first.** She works from her phone. Test at 360px width.
- Works **offline** once loaded (any font must have a system fallback).
- Deploy with **GitHub Pages** from the repo root.

---

## 8. Acceptance checklist (Sonnet checks before handing back)

- [ ] All six bugs in 2a are fixed, and there is no hard-coded question count.
- [ ] No 16Personalities names; role labels removed or correct.
- [ ] Nothing from the 2c slop list remains (no gradients, no emoji UI, no dark slate theme).
- [ ] Neo-brutalist flat look: solid fills, thick outlines, hard offset shadows, light background.
- [ ] 28 items, 7 per axis, two-sided, left pole randomised, each one a concrete situation.
- [ ] Close axes show both letters; bars drawn from the winning side with both percentages.
- [ ] Each result has one honest weak spot; one-line "fun, not diagnosis" footnote.
- [ ] Works at 360px with no horizontal scroll; no external network requests required.
- [ ] Short README: what it is, how to edit questions, live link.
