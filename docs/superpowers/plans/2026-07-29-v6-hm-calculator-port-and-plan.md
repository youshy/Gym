# v6 Half-Marathon Refresh — Calculator Port + Executable Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Move the running-paces + nutrition cards onto the current v6 calculator, repoint the landing page, and author the day-to-day executable half-marathon plan that sits on the v6 program.

**Architecture:** Two independent deliverables. (B) Port the two already-verified cards from `v5/v5-calculator.html` into `v6/v6-calculator.html` (structurally identical scaffolding), extend v6's `selfCheck()`, and repoint `index.html`. (C) Write `v6/2026-07-29-half-marathon-plan.md` — an executable companion to the v6 lifting doc that translates the (v6-retargeted) HM spec into concrete sessions, week-by-week tables, a checkpoint calendar, and a fuelling quick-reference.

**Tech Stack:** Vanilla JS + HTML + CSS, single file, no deps, no build step (calculator). Markdown (plan doc).

## Global Constraints

- Calculator: single file, vanilla JS, **no dependencies, no build step** (match existing v6-calculator).
- Store only *inputs* in `state`; **recompute all derived values on render**; persist nothing derived.
- Every new pure math function gets an assertion in v6-calculator's existing `selfCheck()` IIFE.
- Reuse existing CSS classes (`.card`, `.tmline`, `.est`, `.pill`, `.note`, `table`) — no new styles.
- Pace offsets / macro multipliers are rules-of-thumb → keep the `// ponytail:` comments noting the monthly 5k TT is the calibrator.
- Plan doc: **phase-relative weeks**, race = **September 2027**. Reference the v6 lifting engine + `v6-calculator.html` for live numbers; **do not** duplicate rationale from the spec (`docs/superpowers/specs/2026-07-29-half-marathon-add-on-design.md`).
- Calculator verification for every task: open `v6/v6-calculator.html` in a browser → console prints `v6calc self-check done` (or existing equivalent) with **no** `SELF-CHECK FAIL` lines. (Math is already node-verified: 27:00 5k → 2:04:12 HM @ 5:53/km; macros(97) → protein 194 g, hard carbs 460.75 g, moderate day 2638 kcal.)

---

## Part B — Calculator port

### Task 1: Port both cards into v6-calculator.html

**Files:**
- Modify: `v6/v6-calculator.html` (math fns after `fmt` ~line 103; DEFAULTS fields after `clean: 70,` ~line 127; two card HTMLs before `<footer>` ~line 94; `renderRunning()`+`renderNutrition()` after the last `render*()`; wiring in `renderAll()` ~line 189 and after the `#clean` listener; assertions in `selfCheck()` ~line 241; `<title>` line 6, `<h1>` line 48).
- Reference (source of the exact known-good code): `v5/v5-calculator.html`.

**Interfaces:**
- Produces (pure fns): `parseTime(str)->sec`, `paceStr(sec)->"m:ss"`, `fmtClock(sec)->"h:mm:ss"`, `riegelSeconds(t1,d1,d2)->sec`, `paces(fiveKsec)->{easy,threshold,interval,hm,hmFinish}`, `macros(bw)->{protein,fat,carb:{hard,moderate,easy},kcal:{hard,moderate,easy}}`, `PHASE` (keys `base|build|peak`).
- State fields added to `DEFAULTS`: `fiveK` (string), `goalHM` (string), `bw` (number), `phase` (string).

- [ ] **Step 1: Confirm the port target matches the source scaffolding**

Run: `grep -n "const DEFAULTS\|clean: 70\|function renderAll\|function selfCheck\|const fmt\|<footer\|<title>\|<h1" v6/v6-calculator.html`
Expected: `DEFAULTS`, `clean: 70`, `renderAll`, `selfCheck`, `fmt`, `<footer>`, `<title>`, `<h1>` all present (same scaffolding as v5). If `const fmt` line differs, insert math after whatever the last top-of-script helper is.

- [ ] **Step 2: Add the failing self-check assertions** — inside `selfCheck()` IIFE, before its final `console.log`:

```js
  A(parseTime("25:00") === 1500, "parseTime 25:00 = 1500");
  A(paceStr(313) === "5:13", "paceStr 313 = 5:13");
  A(fmtClock(6900) === "1:55:00", "fmtClock 6900 = 1:55:00");
  A(Math.abs(riegelSeconds(1500, 5, 21.0975) - 6900) < 3, "riegel 25:00 5k -> ~1:55:00 HM");
  A(paceStr(paces(1500).easy) === "6:25", "easy pace off 25:00 5k = 6:25");
  A(paceStr(paces(1500).threshold) === "5:22", "threshold pace off 25:00 5k = 5:22");
  A(macros(97).protein === 194, "macros(97) protein = 194g");
  A(Math.abs(macros(97).carb.hard - 460.75) < 1e-9, "macros(97) hard carbs = 460.75g");
  A(macros(97).kcal.moderate === 2638, "macros(97) moderate day = 2638 kcal");
  A(PHASE.peak.rate.startsWith("0"), "peak phase loss rate is 0");
```

- [ ] **Step 3: Verify it fails** — reload `v6/v6-calculator.html` → console. Expected: `ReferenceError: parseTime is not defined` (self-check aborts; fns don't exist yet).

- [ ] **Step 4: Add the pure math** — insert after the `fmt` definition (~line 103):

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

- [ ] **Step 5: Verify the math passes** — reload → console prints self-check done, no FAIL.

- [ ] **Step 6: Add DEFAULTS fields** — inside `DEFAULTS`, after `clean: 70,`:

```js
  fiveK: "27:00", goalHM: "1:50:00", bw: 97, phase: "base",
```

- [ ] **Step 7: Add both cards' HTML** — insert immediately before `<footer>` (~line 94):

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

- [ ] **Step 8: Add both render functions** — insert after the file's last `render*()` function:

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

- [ ] **Step 9: Wire into renderAll + events** — (a) inside `renderAll()`, after the existing lift renders:

```js
  $("#fiveK").value = state.fiveK;
  $("#goalHM").value = state.goalHM;
  $("#bw").value = state.bw;
  $("#phase").value = state.phase;
  renderRunning();
  renderNutrition();
```

(b) after the existing `#clean` change listener:

```js
$("#fiveK").addEventListener("change",  e => { state.fiveK = e.target.value; renderRunning(); save(); });
$("#goalHM").addEventListener("change", e => { state.goalHM = e.target.value; renderRunning(); save(); });
$("#bw").addEventListener("change",     e => { state.bw = Number(e.target.value); renderNutrition(); save(); });
$("#phase").addEventListener("change",  e => { state.phase = e.target.value; renderNutrition(); save(); });
```

- [ ] **Step 10: Retitle** — change `<title>` (line 6) and `<h1>` (line 48) from `v6 Lift Calculator — 5's PRO` to `v6 Training Calculator — Lifts · Running · Fuel`. Update `<footer>` text to mention loads, paces, and fuel if it only says loads.

- [ ] **Step 11: Verify end-to-end** — reload `v6/v6-calculator.html`. Expected: console self-check done, no FAIL; Running Paces shows HM `2:04:12 (5:53/km)` for default `27:00`, goal pace `5:13/km` for `1:50:00`, four zone paces populate; Nutrition shows protein `194 g`, fat `78 g`, hard/mod/easy carbs `461 / 291 / 194 g`, kcal `3317 / 2638 / 2250`, Base intake/rate line. Editing an input + tab re-derives and survives reload.

- [ ] **Step 12: Commit**

```bash
git add v6/v6-calculator.html
git commit -m "feat: port running-paces + nutrition cards into v6-calculator

Co-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>"
```

### Task 2: Repoint the landing page at v6-calculator

**Files:**
- Modify: `index.html` (the single `<a class="card" href=...>` and its copy).

- [ ] **Step 1: Repoint + refresh copy** — change the card `href` from `v5/v5-calculator.html` to `v6/v6-calculator.html`. Update the card `<h2>`/`<p>` if they name v5, so the copy reads as the current v6 calculator (lifts + running paces + carb-cycled nutrition).

- [ ] **Step 2: Verify** — open `index.html`; the card links to `v6/v6-calculator.html`; clicking it loads the v6 calculator with all cards. `grep -n "href" index.html` shows the v6 path, no `v5` link remains.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "chore: point landing page at v6-calculator (v5 frozen)

Co-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>"
```

---

## Part C — Executable half-marathon plan

### Task 3: Author v6/2026-07-29-half-marathon-plan.md

**Files:**
- Create: `v6/2026-07-29-half-marathon-plan.md`
- Reference: `docs/superpowers/specs/2026-07-29-half-marathon-add-on-design.md` (spec — do not duplicate its rationale), `v6/2026-07-26-workout-plan.md` (lifting engine), `v6/v6-calculator.html` (live paces/fuel).

**Interfaces:**
- Consumes: the v6 season map, targets/pace table, weekly templates, fuelling tables from the spec; the v6 week structure + deload timing from the lifting doc.
- Produces: a standalone day-to-day companion. No code; the "test" is the coverage gate in Step 3.

This is a document task — no TDD. Write it complete in one pass from the content spec below, then run the coverage gate.

- [ ] **Step 1: Write the document** with these sections and content:

**Header / how to use.** One paragraph: this is the running companion to v6. It adds *quality running + long-run progression* on top of the v6 week; open `v6-calculator.html` for live paces (it re-derives from your latest 5k) and macros. It does not restate the lifting engine (see the v6 workout plan) or the rationale (see the spec). Race: **September 2027**. Weeks are phase-relative; the checkpoints keep them honest.

**Phase map (quick reference).** A compact table restating the four phases (Base ~28 wk → Build ~16 → Peak ~8 → Taper ~2), what running does in each, and the two nesting rules in one line each (specialization waves live in Base; HM cutback weeks = v6's week-4 deload). One line, not the spec's full treatment.

**The quality-session library** (concrete, prescriptive — this is the executable core). Three sub-sections; all paces "per the calculator" with the current-fitness example in parentheses (27:00 5k → T ≈ 5:53+... — actually give the *goal-fitness* numbers too). Warm-up/cool-down: 10–15 min easy + strides before quality.

- *Threshold (T) ladder* — progress across the season:
  | Stage | Session | Total @ T |
  |---|---|---|
  | Base intro | 3 × 6 min @ T, 90s jog | 18 min |
  | Base build | 4 × 6 min / 3 × 8 min | 24 min |
  | Build | 4 × 8 min → 3 × 10 min, 75–90s jog | 32–30 min |
  | Build/Peak | 2 × 15 min @ T, 3 min jog | 30 min |
  | Peak | 20 min continuous T / 2 × 12–15 min | 20–30 min |
- *Intervals (VO2)* — introduce late Base/Build:
  - `5 × 3 min` hard @ interval pace, 2–3 min jog
  - `6 × 800 m` @ interval pace, 2 min jog
  - `8 × 400 m` @ slightly faster, 90s jog (sharpener)
  - progression: add a rep or trim recovery; cap ~5 km of hard volume.
- *Race-pace / long-run-finish* — the race-specific work:
  - long run with `last 3–5 km @ HM pace` (Build), growing to `last 6–8 km` (Peak)
  - progression run: easy → last third at HM pace
  - `3 × 2 km @ HM pace`, 3 min jog (Peak sharpener)
  - **6-weeks-out rehearsal:** `8–10 km continuous @ goal HM pace` — the honest test.

**Week-by-week progression tables** — one table per phase, columns `Wk | Long run | ~Weekly km | Quality sessions | Notes`. Mark **cutback weeks (every 4th = v6 deload)** with a ⤵ and −30% long run. Concrete numbers:

- *Base (~28 wk, LR 13→16, 40–45 km/wk, 1 quality/wk = threshold):* ramp LR ~1 km/wk, cut every 4th. Example first 8 wk: 13,14,15,**10⤵**,14,15,16,**11⤵** … then hold 15–16 with a cutback every 4th for the rest of Base. Quality = the threshold ladder, Base stages. Note: run v6 specialization waves here.
- *Build (~16 wk, LR 16→20, 45–55 km/wk, 2 quality = 1 threshold/interval + long-run RP finish):* 16,17,18,**12⤵**,18,19,20,**13⤵**,19,20,20+RP,**14⤵**,20,21,18,taper-in. Fri jumps cut, evening block pull-only/off.
- *Peak (~8 wk, LR 20–21, 50–60 km/wk, 2 quality = threshold + race-pace/intervals):* 20,21,20+RP,**14⤵**,21,20(rehearsal wk: 8–10 km @ RP inside a 18–20 km run),18,taper-in. Lifting = 2 maintenance days.
- *Taper (2 wk):* LR 14 then 10; volume −40–50%; keep 1 short sharp session/wk (e.g. 3×3 min or 4×400) to stay sharp; race week carbs up.

**Checkpoint calendar.**
  - **5k TT every 4 weeks**, on the deload week (fresh legs) → update the calculator's 5k input; all paces re-derive.
  - **Monthly:** log bodyweight trend + easy-run HR-at-pace.
  - **~6 weeks out (early Aug 2027):** HM-pace rehearsal → set the honest race target; race off that number.
  - **Race week:** taper carb-load final 2–3 days.

**Fuelling quick-reference.** A compact restatement (not re-derivation) pointing at the calculator's Nutrition card for live grams:
  - carb day-types: hard ~4.5–5 g/kg (threshold/long/heavy-leg) · moderate ~3 · easy ~2; protein ~2 g/kg + fat ~0.8 g/kg floor constant.
  - threshold-day timeline: caffeine 3–6 mg/kg ~45 min pre · pre-run 60–80 g carb + 25–30 g protein · during water <60 min · post 30–40 g whey + 60–80 g carb.
  - long runs >75 min: 30–60 g carb/hr — practice it.
  - phase intake: Base/Build ~3400, Peak maintenance ~3700; the scale is the arbiter, raise calories first if paces slide.

**Placement reminder (one box).** Quality only on **Wed & Sun** (v6's no-lift days); never two quality days back-to-back; never quality within 24 h of heavy legs (Mon/Thu/Fri). The evening GtG block is the first fatigue knob (pull-only → off).

- [ ] **Step 2: Verify internal consistency** — the phase-week counts in the tables match the spec's season map (Base ~28, Build ~16, Peak ~8, Taper ~2). Cutback weeks fall on every 4th week. Long-run peak = 21 km. All paces defer to the calculator (no hardcoded pace that would go stale except illustrative examples clearly marked).

- [ ] **Step 3: Coverage gate** — confirm every task-3 requirement from the original brief is present:
  - [ ] concrete threshold sessions (3×6→4×8→2×15) ✓
  - [ ] interval sessions (5×3min, 6×800m) ✓
  - [ ] race-pace / long-run-finish workouts ✓
  - [ ] week-by-week long-run + mileage tables per phase with cutback weeks marked ✓
  - [ ] checkpoint calendar (monthly 5k TT, 6-wk-out HM rehearsal) ✓
  - [ ] fuelling quick-reference ✓
  - [ ] phase-relative weeks; references v6 lifting + calculator; no spec-rationale duplication ✓

- [ ] **Step 4: Commit**

```bash
git add v6/2026-07-29-half-marathon-plan.md
git commit -m "feat: executable v6 half-marathon plan (day-to-day companion)

Co-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>"
```

### Task 4: Push everything

- [ ] **Step 1: Push** — `git push origin main`. If rejected (remote moved again), `git fetch && git rebase origin/main` (disjoint files → clean), then push. Report the result and, if the user wants Pages, confirm Settings → Pages → deploy from `main` / root so `index.html` serves.

---

## Self-Review

**Spec coverage:**
- §9 calculator port (cards into v6-calculator, extend selfCheck, repoint index) → Task 1 + 2. ✓
- §1–§3 season map, targets, weekly templates → plan doc phase map + week tables. ✓
- Quality sessions (threshold/interval/race-pace) → quality-session library. ✓ (spec §2 gives paces; sessions are the executable expression, new to the plan doc per the brief.)
- §5 durability / cutback alignment → week tables mark every-4th cutback = v6 deload; placement box. ✓
- §6 nutrition → fuelling quick-reference (restated, not duplicated) + calculator Nutrition card. ✓
- §7 checkpoints → checkpoint calendar. ✓
- Nesting rules (specialization-in-Base, shared deload) → phase map one-liners + Base/Build notes. ✓

**Placeholder scan:** Calculator code blocks are complete, verbatim from verified v5 source. Plan-doc content is fully specified (every table has concrete numbers). No TBD/TODO. ✓

**Type consistency:** `paces()`/`macros()`/`PHASE` signatures identical to the verified v5 implementation and to the self-check assertions; `state` fields `fiveK/goalHM/bw/phase` match DEFAULTS, render fns, and listeners. ✓

**Number check (already node-verified):** default 27:00 → 2:04:12 @ 5:53/km; macros(97) protein 194, hard 460.75, moderate 2638 kcal; riegel(1500)→~6900. ✓
