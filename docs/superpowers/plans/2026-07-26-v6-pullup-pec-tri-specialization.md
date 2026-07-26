# v6 Pullup + Pec/Tri Specialization Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Produce the standalone v6 plan (a full workout-plan doc, a no-equipment hotel backup, and a self-contained lift calculator) that supersedes v5 by folding the pullup + pec/tri specialization into v5's athletic-power base.

**Architecture:** This is a documentation/content deliverable, not code. Three files land in a new `v6/` folder, mirroring the `v4/` and `v5/` house style. There is no build/test runner — "tests" here are **verification checks** (grep for required content, checks for dangling `v5` references, and a browser render check of the calculator). The approved design spec at `docs/superpowers/specs/2026-07-26-v6-pullup-pec-tri-specialization-design.md` is the single content source; tasks reference its sections by name rather than duplicating prose.

**Tech Stack:** Markdown (GitHub-flavored), a single self-contained HTML file with inline CSS/JS (no external deps), git.

## Global Constraints

- **v6 supersedes v5 and must be fully standalone** — no file may require the reader to open v5. No dangling references to `v5`/`v4` except the calculator's localStorage-key change note.
- **Power stays #1.** v6 adds nothing to leg/power days (Mon/Fri); it must not change any barbell TM or the running structure carried over from v5.
- **The governing rule of the evening block:** *always submaximal — if it ever feels like a grind, it has failed its purpose.* This sentence (or a faithful paraphrase) must appear in the plan doc and the hotel backup.
- **The add-on is EVENING, not morning** (main session is ~11–12am; evening gives ~8h separation). Do not describe it as a morning block except in the retained cold-body warm-up fallback.
- **Single source of truth for weights:** barbell loads live only in `v6/v6-calculator.html`; the plan doc must not hand-maintain barbell numbers (matches v5's `<!-- ponytail -->` convention).
- **House style:** match v5's doc voice, heading structure, and table formatting. Files are dated `2026-07-26`.

---

## File Structure

- `v6/v6-calculator.html` — copy of `v5/v5-calculator.html`, rebranded v5→v6. Self-contained. **Owns all barbell loads.**
- `v6/2026-07-26-workout-plan.md` — the full standalone v6 plan. **Owns the training prescription.** Points at `v6-calculator.html` for numbers.
- `v6/2026-07-26-hotel-backup.md` — the no-equipment travel version. **Owns the zero-gear substitutions.**

Order: calculator first (the plan doc links to it), then the plan doc, then the hotel backup (references the plan doc's structure).

---

### Task 1: v6 calculator (self-contained copy of v5)

**Files:**
- Create: `v6/v6-calculator.html` (copied from `v5/v5-calculator.html`, 253 lines)
- Reference only: `v5/v5-calculator.html:6,48,94,129,249` (the five `v5` strings)

**Interfaces:**
- Consumes: nothing.
- Produces: `v6/v6-calculator.html` — the file the plan doc (Task 2) links to as its single source of truth for loads. Its localStorage key is `"v6calc"`.

- [ ] **Step 1: Copy the v5 calculator to v6**

```bash
mkdir -p v6
cp v5/v5-calculator.html v6/v6-calculator.html
```

- [ ] **Step 2: Verify the copy is byte-identical (baseline before rebrand)**

```bash
diff v5/v5-calculator.html v6/v6-calculator.html && echo "IDENTICAL"
```
Expected: prints `IDENTICAL` (no diff output).

- [ ] **Step 3: Rebrand the five v5 strings to v6**

Edit `v6/v6-calculator.html`, changing exactly these:
- Line 6: `<title>v5 Lift Calculator — 5's PRO</title>` → `<title>v6 Lift Calculator — 5's PRO</title>`
- Line 48: `<h1>v5 Lift Calculator — 5's PRO</h1>` → `<h1>v6 Lift Calculator — 5's PRO</h1>`
- Line 94: footer `v5 · 5's PRO ·` → `v6 · 5's PRO ·`
- Line 129: `const KEY = "v5calc";` → `const KEY = "v6calc";` *(fresh storage; v5 was never run so no saved TMs are lost)*
- Line 249: `console.log("v5calc self-check done` → `console.log("v6calc self-check done`

- [ ] **Step 4: Verify no `v5` strings remain**

```bash
grep -n "v5" v6/v6-calculator.html
```
Expected: **no output** (exit code 1). Any hit is a missed rebrand — fix it.

- [ ] **Step 5: Verify it renders and the JS self-check passes**

Open `v6/v6-calculator.html` in a browser. Expected: page titled "v6 Lift Calculator — 5's PRO" renders; DevTools console shows `v6calc self-check done` with no `FAIL` lines.

- [ ] **Step 6: Commit**

```bash
git add v6/v6-calculator.html
git commit -m "feat: v6 calculator (self-contained copy of v5, rebranded)"
```

---

### Task 2: v6 workout-plan doc (standalone)

**Files:**
- Create: `v6/2026-07-26-workout-plan.md`
- Content source: `docs/superpowers/specs/2026-07-26-v6-pullup-pec-tri-specialization-design.md` (all sections)
- Pattern reference: `v5/2026-07-20-workout-plan.md` (voice, headings, tables)

**Interfaces:**
- Consumes: `v6/v6-calculator.html` (links to it for barbell loads).
- Produces: the plan doc referenced by the hotel backup (Task 3).

- [ ] **Step 1: Write the doc from the spec, section by section**

Create `v6/2026-07-26-workout-plan.md` in v5 house style. Required sections, sourced from the named spec sections — write full prose/tables for each (do not stub):

1. **Title + intro** — "Workout Plan v6 — Pullup + Pec/Tri Specialization over the v5 Athletic-Power Base." State it supersedes v5, keeps the athletic-power base, and is standalone. (Spec: *Relationship to v5*, *Goals*.)
2. **Goals & priority order** — power #1, pullups, pec/tri size; 6–8 week time-boxed block = two 4-week waves. (Spec: *Goals*.)
3. **Core architecture — division of labor** table (midday vs evening; vertical pull → evening, horizontal pull → midday; intensity vs frequency) + the evening-not-morning rationale. (Spec: *The core architecture*.)
4. **The full week** table — three layers per day (midday main / evening GtG / run), Sunday evening marked **optional**. Reproduce all v5 midday content so the doc stands alone (snatch Mon, bench Tue, deadlift Thu, cleans Fri, bench-volume Sat, Wed/Sun rest+long runs). (Spec: *Full weekly integration*; source midday details from `v5/2026-07-20-workout-plan.md`.)
5. **Midday session** — the full v5 prescription (explosive openers, 5's PRO engine + starting TMs note pointing to the calculator, accessories, core, running) **plus** the v6 changes: pullups removed from midday; barbell rows stay Thu; Tue adds incline DB + triceps ext, dips as rotating compound; Sat adds weighted dips/incline + pushdown. (Spec: *Midday (main) session changes*; base content from v5 doc.)
6. **The evening block** — warm-up (evening ~2 min + cold-body fallback), the 4–5 superset rounds table, submaximal rule, progression, per-day light/full emphasis table. (Spec: *The evening block*.)
7. **Mesocycle waves** — the 3-load + 1-deload table synced to 5/3/1, retest-fresh rule. (Spec: *Mesocycle waves*.)
8. **Progression engine (5's PRO)** — carry over v5's TM/5's-PRO tables and rules so the doc is standalone; note numbers live in `v6-calculator.html`. (Source: v5 doc *Progression Engine*.)
9. **Running** — carry over v5's running table unchanged. (Source: v5 doc *Running*.)
10. **Recovery / auto-regulation** — deload-together, time-boxing, ordered knobs (evening → pull-only first), v6 red flags. (Spec: *Recovery / auto-regulation*.)
11. **Footer note** — the `<!-- ponytail -->`-style one-source-of-truth comment pointing at `v6-calculator.html`.

Link the calculator as: `[\`v6-calculator.html\`](v6-calculator.html)`.

- [ ] **Step 2: Verify all required concepts are present**

```bash
f=v6/2026-07-26-workout-plan.md
for s in "supersedes v5" "division of labor" "submaximal" "grind" "optional" \
         "grease" "5's PRO" "incline" "triceps" "pushdown" "barbell rows" \
         "deload" "v6-calculator.html" "Snatch" "Cleans" "Deadlift"; do
  grep -qi "$s" "$f" && echo "OK: $s" || echo "MISSING: $s"
done
```
Expected: every line prints `OK:`. Any `MISSING:` → add that content.

- [ ] **Step 3: Verify no dangling v5 file references and no hand-maintained barbell numbers**

```bash
grep -ni "v5-calculator\|v5/2026\|see v5\|from v5" v6/2026-07-26-workout-plan.md
```
Expected: **no output** (the only allowed `v5` mentions are the "supersedes v5 / v5 base" narrative, which this grep excludes). Any hit means a dangling dependency — rewrite it to be standalone.

- [ ] **Step 4: Verify it renders as clean Markdown**

Preview the file (GitHub preview or editor Markdown preview). Expected: all tables render as tables (aligned columns, no broken pipes), headings nest correctly, the calculator link is clickable.

- [ ] **Step 5: Commit**

```bash
git add v6/2026-07-26-workout-plan.md
git commit -m "feat: v6 standalone workout plan (pullup + pec/tri specialization)"
```

---

### Task 3: v6 hotel backup (no-equipment)

**Files:**
- Create: `v6/2026-07-26-hotel-backup.md`
- Content source: spec section *No-equipment backup (travel / hotel)*
- Pattern reference: `v5/2026-07-20-hotel-backup.md` and `v4/2026-07-06-hotel-backup.md`

**Interfaces:**
- Consumes: `v6/2026-07-26-workout-plan.md` (mirrors its evening-block structure and waves).
- Produces: nothing downstream.

- [ ] **Step 1: Read the existing hotel backups for house style**

```bash
sed -n '1,40p' v5/2026-07-20-hotel-backup.md
```
Match its tone, heading structure, and framing (a swap-in for when you have no gym/gear).

- [ ] **Step 2: Write the v6 hotel backup**

Create `v6/2026-07-26-hotel-backup.md` covering:
1. **Intro** — zero-gear travel version of the v6 evening block; same GtG philosophy, same 3+1 waves, same submaximal rule.
2. **Push (chest/tri) — handstand-pushup ladder:** pike → feet-elevated pike → wall HSPU negatives → wall HSPU → deficit HSPU (books under hands), plus diamond/close-grip pushups (triceps) and chair dips (tri/lower pec).
3. **Pull (the hard part, no bar/bands):** inverted rows under a sturdy desk/table (walk feet out to harden); door-anchored towel rows; backpack gorilla/bent-over rows; sliding floor lat pull-ins.
4. **Structure** — same antagonist superset (pull + push), same per-day light/full emphasis, same reps-in-reserve rule.
5. **Progression** — reps → range/leverage → load (backpack), mirroring the main doc.

- [ ] **Step 3: Verify all substitutions are present**

```bash
f=v6/2026-07-26-hotel-backup.md
for s in "handstand" "pike" "diamond" "chair dip" "inverted row" \
         "towel" "backpack" "submaximal\|grind" "superset" "wave\|deload"; do
  grep -qiE "$s" "$f" && echo "OK: $s" || echo "MISSING: $s"
done
```
Expected: every line prints `OK:`. Any `MISSING:` → add it.

- [ ] **Step 4: Verify no equipment leaks in (should be truly gear-free)**

```bash
grep -niE "kettlebell|\bKB\b|\bband\b|barbell|pull-?up bar|calculator" v6/2026-07-26-hotel-backup.md
```
Expected: **no output**. Any hit contradicts "no-equipment" — replace with a bodyweight/improvised substitute.

- [ ] **Step 5: Commit**

```bash
git add v6/2026-07-26-hotel-backup.md
git commit -m "feat: v6 hotel backup (no-equipment: HSPU ladder + improvised rows)"
```

---

### Task 4: Final cross-check (whole v6 folder stands alone)

**Files:**
- Verify: `v6/` (all three files)

**Interfaces:**
- Consumes: Tasks 1–3 outputs.
- Produces: a verified, self-contained `v6/` folder.

- [ ] **Step 1: Confirm the folder contents**

```bash
ls -1 v6/
```
Expected exactly:
```
2026-07-26-hotel-backup.md
2026-07-26-workout-plan.md
v6-calculator.html
```

- [ ] **Step 2: Confirm no cross-file dependency on v5**

```bash
grep -rniE "v5[-/]|see v5|open v5" v6/ | grep -viE "supersed|v5 base|athletic-power base|never run"
```
Expected: **no output**. Any hit is a standalone-violation — fix it.

- [ ] **Step 3: Confirm the three global-constraint invariants hold**

```bash
grep -qi "submaximal" v6/2026-07-26-workout-plan.md && echo "OK submax-rule"
grep -qi "evening" v6/2026-07-26-workout-plan.md && echo "OK evening"
grep -qi "v6-calculator.html" v6/2026-07-26-workout-plan.md && echo "OK single-source"
```
Expected: three `OK` lines.

- [ ] **Step 4: Confirm the working tree is clean (everything committed)**

```bash
git status --porcelain
```
Expected: **no output**. If anything is uncommitted, commit it.

---

## Self-Review (completed while writing this plan)

- **Spec coverage:** every spec section maps to a task — *Relationship/Goals/architecture/weekly/midday changes/evening block/waves/recovery* → Task 2; *No-equipment backup* → Task 3; *Implementation deliverables* (calculator copy, standalone docs) → Tasks 1–3; standalone + single-source constraints → Task 4. No gaps.
- **Placeholder scan:** no TBD/TODO; every content step enumerates the exact sections/items to write and pairs them with a concrete grep verification.
- **Type/name consistency:** filenames, the `v6calc` localStorage key, and the `v6-calculator.html` link string are identical everywhere they appear across Tasks 1, 2, and 4.
