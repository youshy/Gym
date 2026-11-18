---
name: Training
description: A paper program sheet for one lifter, read in two-second glances between sets.
colors:
  oxide: "oklch(50% 0.15 35)"
  oxide-on: "oklch(98% 0.01 75)"
  chalk: "oklch(97.5% 0.008 75)"
  chalk-paper: "oklch(99.3% 0.004 75)"
  lift-band: "oklch(94.5% 0.022 55)"
  iron-ink: "oklch(22% 0.015 50)"
  pencil: "oklch(47% 0.015 50)"
  rule-line: "oklch(89% 0.01 70)"
typography:
  headline:
    fontFamily: "-apple-system, BlinkMacSystemFont, 'Segoe UI', system-ui, sans-serif"
    fontSize: "30px"
    fontWeight: 750
    lineHeight: 1.1
    letterSpacing: "-0.015em"
  load:
    fontFamily: "-apple-system, BlinkMacSystemFont, 'Segoe UI', system-ui, sans-serif"
    fontSize: "19px"
    fontWeight: 700
    lineHeight: 1.3
    fontFeature: "tnum"
  title:
    fontFamily: "-apple-system, BlinkMacSystemFont, 'Segoe UI', system-ui, sans-serif"
    fontSize: "17px"
    fontWeight: 600
    lineHeight: 1.3
  body:
    fontFamily: "-apple-system, BlinkMacSystemFont, 'Segoe UI', system-ui, sans-serif"
    fontSize: "16px"
    fontWeight: 400
    lineHeight: 1.45
  note:
    fontFamily: "-apple-system, BlinkMacSystemFont, 'Segoe UI', system-ui, sans-serif"
    fontSize: "14px"
    fontWeight: 400
    lineHeight: 1.45
  label:
    fontFamily: "-apple-system, BlinkMacSystemFont, 'Segoe UI', system-ui, sans-serif"
    fontSize: "13px"
    fontWeight: 650
    letterSpacing: "0.08em"
rounded:
  sm: "7px"
  md: "10px"
  lg: "12px"
spacing:
  gutter: "16px"
  row: "14px"
  section: "32px"
components:
  tab-day:
    textColor: "{colors.pencil}"
    typography: "{typography.title}"
    height: "48px"
  tab-day-active:
    textColor: "{colors.iron-ink}"
  week-segment:
    backgroundColor: "{colors.chalk-paper}"
    textColor: "{colors.pencil}"
    rounded: "{rounded.md}"
    height: "40px"
  week-segment-active:
    backgroundColor: "{colors.oxide}"
    textColor: "{colors.oxide-on}"
    rounded: "{rounded.sm}"
  exercise-row:
    textColor: "{colors.iron-ink}"
    padding: "14px 0"
  exercise-row-main:
    backgroundColor: "{colors.lift-band}"
    padding: "16px"
---

# Design System: Training

## 1. Overview

**Creative North Star: "The Coach's Program Sheet"**

A sheet of paper a good coach hands you, clipped to the rack: ruled lines, a number column, terse notes in pencil. The page reads at arm's length under fluorescent gym light, between sets, with sweaty hands. The load and the sets×reps are the only loud things; everything else is pencil.

Density is moderate and vertical. One day per screen, one exercise per ruled row, numbered the way lifters already read programs (1, 2, 3a, 3b). The only persistent chrome is a row of index tabs for the days. No libraries, no web fonts, no decorative motion: it must be readable before the gym wifi finishes thinking.

This system rejects fitness-app neon on black (Strava, Whoop, gym-bro dark UI with lime or orange accents) and gamified trackers (streaks, badges, progress rings, confetti, motivational copy).

**Key Characteristics:**
- Light chalk paper by default; a warm dark variant follows the system theme.
- The weight and the sets×reps are the biggest things on every row.
- One accent (oxide red) for "you are here": active tab, active week, today.
- Hairline rules and tonal bands instead of cards and shadows.
- Superset pairs shown by numbering (3a/3b) and a dashed rule, never by tags.

## 2. Colors

Restrained: warm-tinted neutrals carry the page, one oxide accent marks position.

### Primary
- **Oxide** (oklch(50% 0.15 35); dark: oklch(72% 0.14 40)): the active week segment fill, the active day-tab underline, the today dot, links, focus rings. Nothing else.

### Neutral
- **Chalk** (oklch(97.5% 0.008 75); dark: oklch(18% 0.01 50)): page background and sticky header.
- **Chalk Paper** (oklch(99.3% 0.004 75); dark: oklch(22% 0.012 50)): the week segmented control track.
- **Lift Band** (oklch(94.5% 0.022 55); dark: oklch(24% 0.025 45)): background of the day's 5/3/1 main-lift row only.
- **Iron Ink** (oklch(22% 0.015 50); dark: oklch(94% 0.01 75)): exercise names, loads, headings.
- **Pencil** (oklch(47% 0.015 50); dark: oklch(72% 0.015 70)): notes, sets×reps, row numbers, inactive tabs, section labels. AA on Chalk in both themes.
- **Rule Line** (oklch(89% 0.01 70); dark: oklch(31% 0.012 50)): row dividers, header border, table rules.

### Named Rules
**The Position Rule.** Oxide only answers "where am I?" (day, week, today). It never decorates, never colors a number, never fills a row.

**The One Band Rule.** Lift Band appears on exactly one row per day: the 5/3/1 main lift. If two rows are tinted, the hierarchy is gone.

## 3. Typography

**Body Font:** system UI sans (-apple-system, Segoe UI, system-ui). One family for everything.

**Character:** native and invisible; numbers set in tabular figures so weights line up down the right edge like a printed column.

### Hierarchy
- **Headline** (750, 30px, 1.1): the day name. One per screen.
- **Load** (700, 19px; 21px on the main lift, tabular): the weight, or each 5/3/1 set as "90 × 5". The hero of every row.
- **Title** (600, 17px, 1.3): exercise names and day tabs.
- **Body** (400, 16px, 1.45): sets×reps under the load, rules list (max 68ch).
- **Note** (400, 14px, 1.45): coaching notes under exercise names, in Pencil, max 58ch.
- **Label** (650, 13px, 0.08em, uppercase): section labels "Session", "Evening", "Rules".

### Named Rules
**The Arm's Length Rule.** Nothing a lifter must read mid-set is smaller than 16px. 14px is for notes you read once, before the set.

## 4. Elevation

Flat. No shadows anywhere. Depth comes from tone (Lift Band, Chalk Paper track) and hairline rules. The sticky header separates from content with a 1px Rule Line border, not a shadow.

**The Paper Rule.** If something looks like it floats, it's wrong. Paper doesn't float.

## 5. Components

### Navigation: Day Index Tabs
- **Style:** full-width row of equal tabs (Mon, Tue, Thu, Fri, Notes), 48px tall, Title type, Pencil text, no background.
- **Active:** Iron Ink text plus a 3px Oxide underline.
- **Today:** a 6px Oxide dot above the label, plus "(today)" in the accessible name.
- **Behavior:** the only sticky element. Each tab sets the URL hash (`#thu`), so a tab can be linked or bookmarked. No hash opens today's session, or the next one.

### Week Segmented Control
- **Shape:** Chalk Paper track with gently curved corners (10px), 3px inset; segments 40px tall with 7px corners.
- **Active:** Oxide fill, Oxide-on text. Inactive: Pencil text, no fill.
- **Placement:** top of the day content, not sticky (it changes once a week). Hidden on Notes.

### Exercise Row (signature)
- **Grid:** number column (2em) | name + note | load column, right aligned, no wrapping.
- **Divider:** 1px Rule Line on top; dashed when the row is the "b" half of a superset.
- **Main lift variant:** Lift Band background bleeding to the screen edge on phones (12px corners from 712px up), load at 21px, three stacked sets, "TM 120" in Pencil underneath.
- **Skipped:** 45% opacity, load column reads "skip".
- **Word loads** ("bodyweight", "fast & crisp") drop to 14px Pencil so numeric loads stay dominant.

### Warm-up Line
- Plain paragraph under the day subtitle, "Warm-up:" in 650 weight. Not a box.

### Tables (Cycle tests)
- Hairline rules under each row, Pencil header text, tabular figures, horizontal scroll inside its own container only.

## 6. Do's and Don'ts

### Do:
- **Do** make the load the largest text on every row (19px / 700, 21px for the main lift).
- **Do** number exercises 1, 2, 3a, 3b; the ⇄ marker in `data.json` becomes the a/b pair.
- **Do** keep tap targets at least 40px (tabs 48px) for sweaty hands.
- **Do** meet WCAG AA contrast for Pencil on Chalk in both themes.
- **Do** keep transitions to color only, 150ms.

### Don't:
- **Don't** use fitness-app neon on black (Strava / Whoop / gym-bro dark UI with lime or orange accents).
- **Don't** add gamified tracker elements: streaks, badges, progress rings, confetti, motivational copy.
- **Don't** wrap exercises in cards or add shadows; rows are ruled lines on paper.
- **Don't** use Oxide on numbers, rows, or decoration (The Position Rule).
- **Don't** add tags or badges to rows ("superset", "new"); the numbering already says it.
- **Don't** add a web font, icon font, or JS library. System fonts only.
- **Don't** use `border-left` stripes, gradient text, or glass blur.
