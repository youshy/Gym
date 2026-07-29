# Half Marathon Add-On — Design Spec (on the v6 base)

**Date:** 2026-07-29 (revised same day onto the v6 base — v6 superseded v5 as the current program)
**Goal:** Run a half marathon in **September 2027** (~13–14 month runway from now). Floor is sub-2:00; the real target is meaningfully faster.
**Constraint:** This is an *add-on* to the current **v6 program** (pullup + pec/tri specialization over the athletic-power base). **Lifting stays king** for most of the year; running quality yields the priority only in the final race block.
**Athlete context:** 193 cm, 97 kg. Currently ~5:50/km stable pace, all-easy Z2 running 7 days/wk, 30–45 km/wk. Trains **two-a-days per v6**: midday (~11–12, fed) = main lift + run; evening = grease-the-groove pull/push block. Tracks macros. Omnivore. Caffeine user. Bad back is a known limiter (see v6 deadlift cap). Currently cutting.

**Chosen approach:** Periodized blocks. Sequence the emphasis over the season rather than stacking everything into one week — the long runway is what makes concurrent goals compatible. The half-marathon periodization is the **outer season cycle**; v6's time-boxed 6–8 week specialization waves are an **inner block that runs during Base** (see §1).

**Relationship to v6:** this spec adds a *quality-running and fuelling* layer on top of v6. It does **not** restate v6's lifting engine, evening block, or 5's PRO progression — that doc is authoritative for the barbell and the pull/push work. Live numbers (lift TMs *and* running paces + nutrition targets) live in `v6/v6-calculator.html`.

---

## 1. Season map (macrocycle)

Race is **September 2027**. Working backwards from there (weeks are approximate — the checkpoints in §7 keep it honest):

| Phase | Window | ~Weeks | Lifting (v6) | Running focus | Long run |
|-------|--------|--------|--------------|---------------|----------|
| **Base** | now (Aug 2026) → ~Feb 2027 | ~28 | **Full v6**, including the 6–8 wk specialization waves | 1 quality/wk + aerobic base + durability | 13 → 16 km |
| **Build** | ~Mar → early Jul 2027 | ~16 | **Lean-base v6, throttled**: TM frozen, **Fri jumps cut**, evening block trimmed, **no specialization** | 2 quality stimuli + race-pace intro | 16 → 20 km |
| **Peak** | ~Jul → late Aug 2027 | ~8 | **Maintenance: 2 heavy-brief days**, no jumps/Oly grind, evening block pull-only or off | Running temporarily king | 20–21 km |
| **Taper** | final ~2 wk | 2 | Maintenance, lighter | Sharpen, shed fatigue | pull back to ~12 km |

**Two nesting rules that make v6 and the HM season coexist:**
1. **Run the v6 specialization waves during Base only.** The pullup/pec-tri specialization is a 6–8 wk block (two 4-week waves) that belongs where lifting is king and running is light — i.e. Base. Once Build starts, you are on **lean-base v6**; you do not run a specialization block through the race build. (You can fit two or more specialization cycles across the long Base if you want them.)
2. **One shared deload.** v6 already deloads everything together in week 4 (5/3/1 + runs −40% + evening block →50%). The HM long-run **cutback weeks land on that same week** — they stack, they don't fight.

**Principle:** Strength is *maintained* on far less than it takes to build (2 heavy sessions/wk holds it for months at high load). Aerobic fitness is volume-/frequency-hungry. So when time is scarce (Peak), running gets it and the barbell is frozen, not lost. Lifting is king ~80% of the season.

---

## 2. Targets and pace math

| Finish | Pace | Verdict |
|--------|------|---------|
| 2:00:00 | 5:41/km | **Floor** — the long run alone gets here |
| 1:55:00 | 5:27/km | comfortable with any quality work |
| **1:50:00** | **5:13/km** | **Primary target** |
| 1:45:00 | 4:58/km | **Reach** — if weight drops well + quality sticks |

Weight is a free lever: ~2–3 sec/km per kg at HM pace. 97 → ~88–90 kg plausibly worth ~15–20 sec/km.

**Training paces** (starting estimates off 1:50; recalibrate monthly off a 5k time-trial — the calculator does this from one input; will drift faster as weight/fitness change):

| Zone | Pace | Feel |
|------|------|------|
| Easy / Z2 | 6:15–6:45/km | conversational — deliberately *slower* than habitual 5:50 |
| Long run | easy pace | later with race-pace finishing segments |
| Threshold (T) | ~5:00/km | comfortably hard, 20–40 min total in reps |
| Intervals (VO2) | 4:30–4:45/km | hard, 3–5 min reps |
| Race pace (HM) | 5:13/km | goal-pace rehearsal |

---

## 3. Weekly templates (on the v6 week)

**Law of placement:** quality running never lands on or next to a heavy-leg lift day (Mon squat, Thu deadlift, Fri cleans+jumps). v6's lifting-rest days — **Wed and Sun** — are already the big-run days, so they are the clean quality slots. This is the same placement v6 already uses; the add-on only changes *what* the Wed/Sun runs are.

**The v6 day has three layers** — midday main, evening GtG pull/push, run. The add-on touches only the **run** layer (and trims the evening block when running gets hard):

```
Midday (~11–12, fed):  warm-up → explosive opener → main lift (5's PRO) → adds → run
Evening (~15–20 min):  GtG pull + push, submaximal   ← the v6 block; first thing trimmed under fatigue
```

On Wed (quality run, no lift): midday = fed quality run; evening = GtG (Wed is v6's best add-on day, so keep it full unless fatigued). Sunday = long run; evening optional/off (v6 already treats Sunday as a built-in light day).

### Base week (lifting = full v6 incl. specialization; one quality run)
```
Mon  squat + snatch skill  + 3km easy   | eve GtG full
Tue  bench + upper plyo     + 4–5km easy | eve GtG (push light)
Wed  REST from lift  → QUALITY: threshold (legs fresh) | eve GtG full
Thu  deadlift               + 3km easy   | eve GtG (pull light)
Fri  cleans + jumps         + 4–5km easy | eve GtG full
Sat  bench volume + plyo    + 4–5km easy | eve GtG (push light)
Sun  REST → LONG RUN 13→16km easy        | eve optional
```
~40–45 km/wk. ~90% the existing v6 week — one easy run becomes a threshold session.

### Build week (lean-base v6: TM frozen, **Fri jumps cut**, no specialization, evening block trimmed; 2 quality stimuli)
```
Mon  squat (maint load)     + 3km easy   | eve pull-only or off
Tue  bench (maint)          + 4–5km easy | eve pull-only or off
Wed  REST → QUALITY #1: intervals or threshold           | eve off
Thu  deadlift (maint)       + 3km easy   | eve pull-only or off
Fri  cleans light, NO jumps + 4–5km easy | eve off
Sat  bench volume           + 5km easy   | eve pull-only or off
Sun  REST → LONG RUN 16→20km, last 3–5km at race pace   ← quality #2
```
~45–55 km/wk. Cutting Friday jumps is the highest-leverage interference reliever (they fight running legs most, highest tendon cost at 97 kg). The long-run race-pace finish *is* the second quality stimulus — no extra hard day, so lifting keeps its days. The evening block drops to pull-only (or off) so the freed recovery goes to running.

### Peak week (lifting = 2 maintenance days, no jumps/Oly grind, evening pull-only/off; running temporarily king)
```
Mon  MAINTENANCE LIFT A  + easy
Tue  QUALITY #1: threshold
Wed  easy / medium
Thu  MAINTENANCE LIFT B  + easy
Fri  QUALITY #2: race-pace reps or intervals
Sat  easy
Sun  LONG RUN 20–21km with race-pace finish
```
~50–60 km/wk. Then 2-week taper: volume −40–50%, intensity short-but-sharp, long run → ~12 km.

---

## 4. Maintenance lifting spec (Peak phase)

Hold the load, kill the volume (this is v6's engine stripped to a holding pattern):
- **Day A:** Squat 3×3 @ ~85% TM · Bench 3×3 · Pullups 2–3 sets (the pullup priority survives here as low-volume maintenance).
- **Day B:** Deadlift 3×2 (**back-capped, per v6**) or trap-bar · OHP 3×5 · one row/chin.
- **No jumps, no plyo, no AMRAP, no specialization, no extra accessories.** Snatch only as optional light skill greaser.

Heavy triples 2×/wk hold everything built in Base+Build through race day.

---

## 5. Durability rules (193/97 + back — non-negotiable)

- Mileage and long run ramp **≤ ~1 km/wk / ~10%**, cutback week every 3rd–4th week (−30%), **aligned to v6's week-4 deload**. Tendons adapt slower than lungs; on a heavy frame this is the real speed limit, not fitness.
- **Never two quality-run days back to back; never quality within 24h of heavy legs.** Templates enforce this — don't freelance.
- Watch heavy-runner tissues: achilles, patellar tendon, shins. Cutting jumps in Build/Peak protects exactly these.
- Cushioned shoes; rotate a second pair.
- **The back stays the canary:** deadlift/cleans yield before the running goal. DL is maintenance-capped in Peak anyway (already back-capped in v6).
- **Fatigue knobs, in order** (extends v6's existing knob order — evening block goes first because it's the cheapest thing to shed and protects the priorities):
  1. Evening GtG block → **pull-only** (drop push first).
  2. Cut evening days **6 → 4**, or off entirely in Build/Peak.
  3. Drop the Wed quality run to easy for a week.
  4. Cut the long run 20%.
  5. Extra cutback/deload week.
  6. Drop a lift day early.

---

## 6. Nutrition

### 6.1 Calorie / cut periodization

Cut is periodized like training, and **ends at a target weight, not a duration** (target ~88–90 kg; lose ~7–9 kg then hold — do NOT run the 0.6kg/wk rate across the whole phase). The long Base runway means the weight can come off slowly and cheaply, well before quality running ramps.

| Phase | Intake | Loss rate |
|-------|--------|-----------|
| **Base (now)** | **~3400 kcal** | ~0.3–0.4 kg/wk |
| **Build** | ~3400 (nudge up as volume grows) | ~0.2–0.3 kg/wk |
| **Peak** | maintenance (~3700) | 0 — fuel the race build |

**Maintenance estimate ~3500–3700** (add-up method + empirical scale data agree; the BMR×2.0 multiplier over-credits training and is discounted). Starting the cut at **3400** (up from the harsh 3000) errs toward fuelling.

**The scale is the arbiter.** Target the *rate*, not the arithmetic: losing faster than ~0.4 kg/wk → add ~150 kcal; slower than ~0.2 → trim ~150. Converge over 2–3 weeks. If strength or threshold paces slide, **raise calories first**, don't push harder.

### 6.2 Macro framework (97 kg, tracked)

Constants every day; carbs flex.

| Macro | Target | Notes |
|-------|--------|-------|
| **Protein** | **~195 g** (~2 g/kg) | Non-negotiable in a deficit — protects strength. ~40 g across 4–5 feeds. |
| **Fat** | **~70–80 g floor** (~0.8 g/kg) | Hormonal floor; don't go under. |
| **Carbs** | **flex 200 → 480 g** | The lever (below). |

**Carb cycling — fuel for the work required** (the +400 kcal bump from 3000→3400 lands here, on hard days):

| Day type | Carbs | Net calories |
|----------|-------|--------------|
| **Hard** (Wed threshold, Sun long, heavy-leg midday) | ~4.5–5 g/kg (~430–480 g) | ~maintenance |
| **Moderate** (other lift+run days) | ~3 g/kg (~290 g) | moderate deficit |
| **Easy / low** (deload, pure-easy days) | ~2 g/kg (~200 g) | deepest deficit |

Weekly average lands the target loss rate; hard days stay protected.

### 6.3 Threshold-day fuelling playbook

Rule: **never run threshold underfueled** — quality collapses without available carbs. Threshold day = high-carb day.

| When | Do |
|------|-----|
| Night before | Normal carbs — no carb-load needed for a 40-min threshold. |
| On waking | 500 ml water + electrolytes. |
| ~45–60 min pre | **Caffeine 3–6 mg/kg** → ~200–400 mg (strong coffee ×1–2 or pre-workout). ~1–2% legal lever. |
| 60–90 min pre | Pre-run meal: ~60–80 g easy carbs + ~25–30 g protein, low fat/fibre (oats+whey+banana, or toast/honey+eggs). |
| During | Water only (<60 min). Fuel only if >75 min. |
| Within 30–60 min after | ~30–40 g whey + ~60–80 g fast carbs, then a real meal within 2 h. Refuels for the evening GtG block. |
| Rest of day | Keep carbs high (high day in the cycle). |

### 6.4 Two-a-day + long-run fuelling

- **Refuel between the midday and evening sessions** — protein + carbs after the midday lift+run so the evening pull/push work is fed.
- **Pre-bed protein** ~40 g slow (dairy/casein) — defends recovery + strength in a deficit.
- **Long runs >75–90 min** (Build onward): **30–60 g carbs/hour** (gels/drink). Practice in training — gut adaptation + race-fuelling rehearsal.
- **Race week:** bump carbs the final 2–3 days (mild taper carb-load).

---

## 7. Feedback loop / checkpoints

- **5k time-trial or parkrun ~every 4 weeks** (on a deload week — legs fresh) → update the calculator's 5k input; every training pace re-derives.
- **HM-pace rehearsal ~6 weeks out** (early Aug 2027; 8–10 km at goal pace) → sets the honest race target; race off that number, not today's guess.
- Track easy-run HR-at-pace, bodyweight trend, and the loss rate weekly.

---

## 8. Key non-obvious decisions (rationale captured)

1. **Periodize, don't stack** — the runway makes sequencing possible; stacking would be a permanent recovery collision.
2. **Nest v6's specialization waves inside Base** — the pullup/pec-tri block runs where lifting is king; Build/Peak drop to lean-base v6.
3. **Cut Friday jumps, not running,** as the primary interference reliever in Build/Peak.
4. **The evening GtG block is the first fatigue knob** — pull-only, then off — because it is cheap to shed and shedding it protects both priorities.
5. **Long-run race-pace finish = the second quality session** — keeps lifting's days intact in Build.
6. **Freeze, don't drop, strength** — 2 heavy days maintain it for months.
7. **One shared deload** — HM cutback weeks land on v6's week-4 deload; the whole system recovers together.
8. **Periodized nutrition** — lose weight in Base (cheap, long runway), fuel fully in Peak; carb-cycle so the deficit lands on days that don't need fuel.
9. **Maintenance is ~3500–3700, not 4000+** — the scale and the add-up agree.
10. **Cut rate de-escalates and ends at a weight** (~88–90 kg), not a duration.

## 9. Calculator extension (live numbers, like the lift TMs)

Port the two running/nutrition cards **into `v6/v6-calculator.html`** (v6 is the current program and its calculator is the single source of truth) — same single-file, localStorage, self-checking pattern as the existing lift cards. `index.html` points at `v6-calculator.html`. (The cards were first built in `v5-calculator.html`; v5 is now frozen legacy. The math is identical — this is a mechanical re-apply of the same diff into v6's calculator, which shares v5's scaffolding.)

**Rule (mirrors the TM design):** store only *inputs*, derive everything on render. The single running input is the **latest 5k time-trial** — the monthly TT keeps every pace honest, exactly as a real set drives the TMs.

### Card A — Running paces
- **Input:** latest 5k time (mm:ss). Optional: goal HM time.
- **Derive** (Riegel `T2 = T1 × (D2/D1)^1.06` to predict HM; training zones as offsets off 5k pace — approximation, calibrated by the monthly TT):
  | Zone | Rule of thumb |
  |------|---------------|
  | Easy / long | 5k pace + ~75–90 s/km |
  | Threshold | 5k pace + ~20–25 s/km |
  | Interval (VO2) | ≈ 5k pace |
  | HM goal / race | predicted (or user's goal) HM pace |
- Show predicted HM finish time alongside the goal, so the gap is visible.

### Card B — Nutrition targets
- **Input:** current bodyweight (kg); phase selector (Base / Build / Peak).
- **Derive:** protein ~2 g/kg (constant), fat ~0.8 g/kg floor (constant), and the three carb day-types (hard ~4.5–5 / moderate ~3 / easy ~2 g/kg) with resulting total kcal per day-type. Phase selector sets the calorie/loss-rate reminder (Base ~3400 / Build ~3400 / Peak maintenance).
- Static note block: the threshold-day fuelling timeline + caffeine dose (3–6 mg/kg) + long-run 30–60 g carb/hr.

### Constraints
- Vanilla JS, single file, no dependencies (match existing v6-calculator).
- Extend `v6-calculator.html`'s existing `selfCheck()` with assertions for the new math (Riegel prediction, a known pace offset, a carb-gram calc).
- All new derived values recompute on render; nothing derived is persisted.

## 10. Biggest risk

Cutting while doing two-a-days + new threshold work + the evening GtG block. Mitigations: the calorie bump (3000→3400), the de-escalating loss rate, carb-cycling, the rule to **raise calories first** at the first sign of sliding strength or paces, and the evening block being the first thing trimmed when fatigue accumulates.
