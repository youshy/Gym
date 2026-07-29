# Calculator Running + Nutrition Extension — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add two cards to the existing training calculator — running paces derived from a 5k time-trial, and bodyweight-scaled nutrition targets — so the running/fuelling numbers live and recompute the same way the lift TMs already do.

**Architecture:** Extend `v5/v5-calculator.html` **in place** (per spec §9). Follow the file's existing pattern exactly: pure math functions at the top of the `<script>`, static card HTML in the `<body>` whose spans are filled by a `render*()` function, state persisted in `localStorage`, and assertions appended to the existing in-browser `selfCheck()`. Store only *inputs* (5k time, goal HM, bodyweight, phase); derive every pace and macro on render — nothing derived is persisted.

**Tech Stack:** Vanilla JS + HTML + CSS, single file, no dependencies, no build step. Tested by the file's own `selfCheck()` (browser console).

## Global Constraints

- Single file, vanilla JS, **no dependencies, no build step** (match the existing file).
- Store only inputs in `state`; **recompute all derived values on render**; persist nothing derived.
- Every new pure math function gets an assertion in the existing `selfCheck()` IIFE.
- Reuse existing CSS classes (`.card`, `.tmline`, `.est`, `.pill`, `.note`, `table`) — no new styles unless strictly needed.
- Pace offsets and macro multipliers are rules-of-thumb → mark with a `// ponytail:` comment noting the monthly 5k TT is the calibrator.
- Verification for every task: open `v5/v5-calculator.html` in a browser, open the console, confirm it prints `v5calc self-check done` with **no** `SELF-CHECK FAIL` lines.

---

### Task 1: Running paces card

**Files:**
- Modify: `v5/v5-calculator.html` (add math fns near line 103 after `fmt`; add DEFAULTS fields ~line 121; add static card HTML before `<footer>` ~line 93; add `renderRunning()` + wiring; add self-check assertions ~line 248)

**Interfaces:**
- Produces (pure fns, used by Task 2's rendering only indirectly — Task 2 reuses `paceStr` is NOT needed; these stay local to running):
  - `parseTime(str) -> seconds` (e.g. `"25:00"` → `1500`)
  - `paceStr(secPerKm) -> "m:ss"` (e.g. `313` → `"5:13"`)
  - `fmtClock(sec) -> "h:mm:ss"` (e.g. `6900` → `"1:55:00"`)
  - `riegelSeconds(t1, d1, d2) -> seconds` (Riegel endurance model)
  - `paces(fiveKsec) -> { easy, threshold, interval, hm, hmFinish }` (all sec/km except `hmFinish` in sec)
  - `state.fiveK` (string "mm:ss"), `state.goalHM` (string "h:mm:ss" or ""), added to `DEFAULTS`

- [ ] **Step 1: Write the failing tests** — append inside the `selfCheck()` IIFE (before the final `console.log`):

```js
  A(parseTime("25:00") === 1500, "parseTime 25:00 = 1500");
  A(paceStr(313) === "5:13", "paceStr 313 = 5:13");
  A(fmtClock(6900) === "1:55:00", "fmtClock 6900 = 1:55:00");
  A(Math.abs(riegelSeconds(1500, 5, 21.0975) - 6900) < 3, "riegel 25:00 5k → ~1:55:00 HM");
  A(paceStr(paces(1500).easy) === "6:25", "easy pace off 25:00 5k = 6:25");
  A(paceStr(paces(1500).threshold) === "5:22", "threshold pace off 25:00 5k = 5:22");
```

- [ ] **Step 2: Run to verify it fails**

Run: open `v5/v5-calculator.html` in a browser → DevTools console.
Expected: `Uncaught ReferenceError: parseTime is not defined` (self-check aborts — functions don't exist yet).

- [ ] **Step 3: Add the pure math** — insert after the `fmt` definition (line 103):

```js
// ---- running paces (ponytail: offsets are rules-of-thumb, recalibrated by the monthly 5k TT) ----
const HM_KM = 21.0975;
const parseTime = s => { const [m, sec] = String(s).split(":").map(Number); return (m || 0) * 60 + (sec || 0); };
const paceStr = spk => { const t = Math.round(spk); return `${Math.floor(t / 60)}:${String(t % 60).padStart(2, "0")}`; };
const fmtClock = sec => { sec = Math.round(sec); const h = Math.floor(sec / 3600), m = Math.floor(sec % 3600 / 60), s = sec % 60;
  return `${h}:${String(m).padStart(2, "0")}:${String(s).padStart(2, "0")}`; };
const riegelSeconds = (t1, d1, d2) => t1 * Math.pow(d2 / d1, 1.06);
const paces = fiveKsec => {
  const p = fiveKsec / 5; // 5k pace, sec/km
  return { easy: p + 85, threshold: p + 22, interval: p + 5,
           hm: riegelSeconds(fiveKsec, 5, HM_KM) / HM_KM, hmFinish: riegelSeconds(fiveKsec, 5, HM_KM) };
};
```

- [ ] **Step 4: Run to verify the math passes**

Run: reload the file → console.
Expected: `v5calc self-check done` prints, no `SELF-CHECK FAIL`.

- [ ] **Step 5: Add DEFAULTS fields** — inside the `DEFAULTS` object (after `clean: 70,`):

```js
  fiveK: "27:00", goalHM: "1:50:00",
```

- [ ] **Step 6: Add the static card HTML** — insert immediately before `<footer>` (line 93):

```html
<div class="card">
  <h2>Running Paces <span class="pill">from your 5k TT</span></h2>
  <div class="tmline">
    <span class="est">Latest 5k time-trial:</span>
    <input id="fiveK" type="text" style="width:80px;" placeholder="mm:ss">
    <span class="est">· Goal HM:</span>
    <input id="goalHM" type="text" style="width:90px;" placeholder="h:mm:ss">
  </div>
  <p class="note">Predicted HM off this 5k: <b id="hmPredict">—</b> · your goal pace: <b id="goalPaceOut">—</b></p>
  <table>
    <thead><tr><th>Zone</th><th>Pace /km</th><th></th></tr></thead>
    <tbody>
      <tr><td class="wk">Easy / long</td><td class="top" id="pEasy">—</td><td class="est">conversational</td></tr>
      <tr><td class="wk">Threshold</td><td class="top" id="pThr">—</td><td class="est">20–40 min in reps</td></tr>
      <tr><td class="wk">Interval (VO2)</td><td class="top" id="pInt">—</td><td class="est">3–5 min reps</td></tr>
      <tr><td class="wk">HM race pace</td><td class="top" id="pHM">—</td><td class="est">goal, else predicted</td></tr>
    </tbody>
  </table>
  <p class="note">Update the 5k time after each monthly time-trial — every pace re-derives from it.</p>
</div>
```

- [ ] **Step 7: Add `renderRunning()`** — insert after `renderCleanRange()` (line 187):

```js
function renderRunning() {
  const t = parseTime(state.fiveK);
  const goal = state.goalHM ? parseTime(state.goalHM) : 0;
  if (!t) { $("#hmPredict").textContent = "—"; return; }
  const p = paces(t);
  $("#hmPredict").textContent = `${fmtClock(p.hmFinish)} (${paceStr(p.hm)}/km)`;
  $("#goalPaceOut").textContent = goal ? `${paceStr(goal / HM_KM)}/km` : "—";
  $("#pEasy").textContent = paceStr(p.easy);
  $("#pThr").textContent  = paceStr(p.threshold);
  $("#pInt").textContent  = paceStr(p.interval);
  $("#pHM").textContent   = paceStr(goal ? goal / HM_KM : p.hm);
}
```

- [ ] **Step 8: Wire it into render + events** — (a) inside `renderAll()` after `renderCleanRange();` add:

```js
  $("#fiveK").value = state.fiveK;
  $("#goalHM").value = state.goalHM;
  renderRunning();
```

(b) after the `#clean` change listener (line 228) add:

```js
$("#fiveK").addEventListener("change",  e => { state.fiveK = e.target.value; renderRunning(); save(); });
$("#goalHM").addEventListener("change", e => { state.goalHM = e.target.value; renderRunning(); save(); });
```

- [ ] **Step 9: Verify end-to-end**

Run: reload the file.
Expected: console prints `v5calc self-check done`, no FAIL. The Running Paces card shows predicted HM `2:04` region for the default `27:00` 5k, goal pace `5:13/km` for `1:50:00`, and the four zone paces populate. Editing the 5k time and tabbing out re-derives every pace; the value survives a reload.

- [ ] **Step 10: Commit**

```bash
git add v5/v5-calculator.html
git commit -m "feat: running-paces card in calculator (5k TT → training paces)

Co-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>"
```

---

### Task 2: Nutrition targets card

**Files:**
- Modify: `v5/v5-calculator.html` (add `macros()` + `PHASE` near the running math; add DEFAULTS fields; add static card HTML before `<footer>`; add `renderNutrition()` + wiring; add self-check assertions)

**Interfaces:**
- Consumes: nothing from Task 1 (independent card).
- Produces:
  - `macros(bw) -> { protein, fat, carb: {hard, moderate, easy}, kcal: {hard, moderate, easy} }` (grams; kcal rounded ints)
  - `PHASE` lookup object keyed `base|build|peak` → `{ label, intake, rate }`
  - `state.bw` (number), `state.phase` (string), added to `DEFAULTS`

- [ ] **Step 1: Write the failing tests** — append inside `selfCheck()` (before the final `console.log`):

```js
  A(macros(97).protein === 194, "macros(97) protein = 194g");
  A(Math.abs(macros(97).carb.hard - 460.75) < 1e-9, "macros(97) hard carbs = 460.75g");
  A(macros(97).kcal.moderate === 2638, "macros(97) moderate day = 2638 kcal");
  A(PHASE.peak.rate.startsWith("0"), "peak phase loss rate is 0");
```

- [ ] **Step 2: Run to verify it fails**

Run: reload the file → console.
Expected: `Uncaught ReferenceError: macros is not defined`.

- [ ] **Step 3: Add the pure math** — insert after the running `paces` function:

```js
// ---- nutrition (ponytail: bodyweight-scaled targets; weekly balance is the athlete's job) ----
const macros = bw => {
  const protein = 2.0 * bw, fat = 0.8 * bw;
  const carb = { hard: 4.75 * bw, moderate: 3.0 * bw, easy: 2.0 * bw };
  const kcal = c => Math.round(protein * 4 + fat * 9 + c * 4);
  return { protein, fat, carb, kcal: { hard: kcal(carb.hard), moderate: kcal(carb.moderate), easy: kcal(carb.easy) } };
};
const PHASE = {
  base:  { label: "Base",  intake: "~3400 kcal",         rate: "0.3–0.4 kg/wk" },
  build: { label: "Build", intake: "~3400 (nudge up)",   rate: "0.2–0.3 kg/wk" },
  peak:  { label: "Peak",  intake: "maintenance ~3700",  rate: "0 — fuel the build" },
};
```

- [ ] **Step 4: Run to verify the math passes**

Run: reload → console.
Expected: `v5calc self-check done`, no FAIL.

- [ ] **Step 5: Add DEFAULTS fields** — inside `DEFAULTS` (after the `fiveK`/`goalHM` line from Task 1):

```js
  bw: 97, phase: "base",
```

- [ ] **Step 6: Add the static card HTML** — insert before `<footer>` (after Task 1's card):

```html
<div class="card">
  <h2>Nutrition Targets <span class="pill">carb-cycle: fuel the work</span></h2>
  <div class="tmline">
    <span class="est">Bodyweight:</span>
    <input id="bw" type="number" step="0.5" style="width:74px;"> kg
    <span class="est">· Phase:</span>
    <select id="phase" style="width:auto;">
      <option value="base">Base</option>
      <option value="build">Build</option>
      <option value="peak">Peak</option>
    </select>
  </div>
  <p class="note"><b id="phaseIntake">—</b> · target loss rate <b id="phaseRate">—</b> · the scale is the arbiter: adjust ±150 kcal to hold the rate.</p>
  <p class="note">Constants every day: protein <b id="mPro">—</b> · fat floor <b id="mFat">—</b>. Carbs flex:</p>
  <table>
    <thead><tr><th>Day</th><th>Carbs</th><th>~kcal</th></tr></thead>
    <tbody>
      <tr><td class="wk">Hard (threshold / long / heavy legs)</td><td class="top" id="cHard">—</td><td id="kHard" class="est">—</td></tr>
      <tr><td class="wk">Moderate (other lift+run)</td><td class="top" id="cMod">—</td><td id="kMod" class="est">—</td></tr>
      <tr><td class="wk">Easy / low (deload, pure easy)</td><td class="top" id="cEasy">—</td><td id="kEasy" class="est">—</td></tr>
    </tbody>
  </table>
  <p class="note">Threshold day: caffeine 3–6 mg/kg ~45 min pre · pre-run 60–80 g carbs + 25–30 g protein · post 30–40 g whey + 60–80 g carbs. Long runs &gt;75 min: 30–60 g carbs/hr.</p>
</div>
```

- [ ] **Step 7: Add `renderNutrition()`** — insert after `renderRunning()`:

```js
function renderNutrition() {
  const m = macros(Number(state.bw) || 0), ph = PHASE[state.phase] || PHASE.base;
  const g = x => `${Math.round(x)} g`;
  $("#phaseIntake").textContent = ph.intake;
  $("#phaseRate").textContent = ph.rate;
  $("#mPro").textContent = g(m.protein);
  $("#mFat").textContent = g(m.fat);
  $("#cHard").textContent = g(m.carb.hard);   $("#kHard").textContent = `${m.kcal.hard} kcal`;
  $("#cMod").textContent  = g(m.carb.moderate); $("#kMod").textContent = `${m.kcal.moderate} kcal`;
  $("#cEasy").textContent = g(m.carb.easy);   $("#kEasy").textContent = `${m.kcal.easy} kcal`;
}
```

- [ ] **Step 8: Wire it into render + events** — (a) inside `renderAll()` after `renderRunning();` add:

```js
  $("#bw").value = state.bw;
  $("#phase").value = state.phase;
  renderNutrition();
```

(b) after Task 1's `#goalHM` listener add:

```js
$("#bw").addEventListener("change",    e => { state.bw = Number(e.target.value); renderNutrition(); save(); });
$("#phase").addEventListener("change", e => { state.phase = e.target.value; renderNutrition(); save(); });
```

- [ ] **Step 9: Verify end-to-end**

Run: reload the file.
Expected: console prints `v5calc self-check done`, no FAIL. The Nutrition card shows protein `194 g`, fat `78 g`, hard/moderate/easy carbs `461 / 291 / 194 g` with kcal `3317 / 2638 / 2250`, and the Base-phase intake/rate line. Changing bodyweight re-derives grams; changing phase updates the intake/rate line; both survive a reload.

- [ ] **Step 10: Update the page title + footer** (scope now covers more than lifts):

Change `<title>` (line 6) and `<h1>` (line 48) from `v5 Lift Calculator — 5's PRO` to `v5 Training Calculator — Lifts · Running · Fuel`.

- [ ] **Step 11: Commit**

```bash
git add v5/v5-calculator.html
git commit -m "feat: nutrition-targets card in calculator (bodyweight → carb-cycled macros)

Co-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>"
```

---

## Self-Review

**Spec coverage (spec §9):**
- Card A — Running paces (5k input, goal HM, Riegel prediction, zone paces, show gap) → Task 1. ✓
- Card B — Nutrition targets (bodyweight + phase input, protein/fat constants, carb day-types + kcal, phase reminder, threshold/long-run fuelling note) → Task 2. ✓
- Store only inputs, derive on render → both cards store `fiveK/goalHM/bw/phase` only; `renderRunning`/`renderNutrition` recompute. ✓
- Vanilla, single file, no deps; extend `selfCheck()` → all tasks. ✓
- Extend in place at `v5/v5-calculator.html` → both tasks. ✓

**Placeholder scan:** No TBD/TODO; all code blocks are complete and runnable. ✓

**Type consistency:** `paces()` returns `{easy, threshold, interval, hm, hmFinish}` — all keys used in `renderRunning` exist. `macros()` returns `{protein, fat, carb:{hard,moderate,easy}, kcal:{hard,moderate,easy}}` — all keys used in `renderNutrition` exist. `PHASE` keys `base|build|peak` match the `<select>` option values and `state.phase` default. `parseTime`/`paceStr`/`fmtClock`/`riegelSeconds` names identical across definition, self-check, and render. ✓

**Self-check number verification:** `macros(97)`: protein `2·97=194`; carb.hard `4.75·97=460.75`; moderate kcal `194·4 + 77.6·9 + 291·4 = 776+698.4+1164 = 2638.4 → 2638`. `riegelSeconds(1500,5,21.0975) = 1500·(4.2195)^1.06 ≈ 6900`. `paces(1500).threshold = 300+22 = 322 → "5:22"`. All assertions are arithmetically exact. ✓
