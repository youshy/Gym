# Product

## Register

product

## Users

One lifter (the author). Uses the page on a phone at the gym, between sets, often with sweaty hands and under bright fluorescent light. Each look is a 2-second glance: which exercise is next, what weight goes on the bar, how many sets and reps. Planning and edits happen separately, on a laptop, by editing `data.json`.

## Product Purpose

A personal training plan viewer. `data.json` holds the program (days, exercises, 5/3/1 training maxes, rules, cycle tests); `index.html` renders the current week and day with weights calculated and rounded down to loadable plates. Success: open it mid-session and know the next lift and load instantly, with no scrolling hunt and no math.

## Brand Personality

Plain, fast, trustworthy. Like a well-made paper program sheet from a good coach: no flourish, numbers you trust at a glance, coaching notes present but out of the way.

## Anti-references

- Fitness-app neon on black (Strava / Whoop / gym-bro dark UI with lime or orange accents).
- Gamified trackers: streaks, badges, progress rings, confetti, motivational copy.

## Design Principles

1. **The number is the hero.** Load and sets×reps are the largest, boldest things on screen; everything else supports them.
2. **One glance, one answer.** Show only the current day and week; navigation is always reachable without scrolling.
3. **Paper-sheet familiarity.** Use conventions lifters already read (3a/3b supersets, TM, `+` sets) instead of inventing UI.
4. **Data drives the page.** All program content lives in `data.json`; the page never hardcodes exercises or weights.
5. **Nothing to wait for.** No libraries, no web fonts, no decorative motion; it loads instantly on gym wifi.

## Accessibility & Inclusion

WCAG 2.2 AA contrast in both light and dark themes. Readable at arm's length: large numerals, generous line height. Tap targets at least 40px for sweaty hands. Respect `prefers-reduced-motion` and `prefers-color-scheme`.
