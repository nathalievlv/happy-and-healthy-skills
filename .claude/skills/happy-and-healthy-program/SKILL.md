---
name: happy-and-healthy
description: Orchestrates a comprehensive body-and-mind program by combining the healthy-lifestyle-program (Wright, physical health) and wellbeing-program (Santos, psychological well-being) into one personalized output. Runs unified intake, executes both skills in parallel, QA-checks their outputs, and synthesizes into a single HTML. Use this skill whenever the user asks for a combined health and happiness program, a holistic well-being plan, a body-and-mind program, wants both physical health and psychological well-being covered, references "happy and healthy", or asks for a comprehensive lifestyle program. Also use when the user says "run the orchestrator", "combined program", "full program", or "both skills". In single-skill mode (user picks only health or only well-being), runs the chosen skill with a QA pass on the output.
---

# Happy & Healthy — Combined Program Orchestrator

This skill orchestrates two specialized sub-skills into one cohesive program:

- **Healthy Lifestyle Program** (`../healthy-lifestyle-program/SKILL.md`) — Wright-anchored physical health: movement, nutrition, supplements, sleep, hormones, diagnostics, skincare, environment
- **Well-Being Program** (`../wellbeing-program/SKILL.md`) — Santos-anchored psychological well-being: gratitude, social connection, meaning, flow, kindness, meditation, savoring

A 2025 network meta-analysis of 183 trials (*Nature Human Behaviour*) found that combining physical exercise with psychological approaches produces the largest well-being effects — larger than either alone. That's the rationale for this orchestrator: body and mind together, not either/or.

## First message

> *"This skill builds a personalized program covering both physical health and psychological well-being — body and mind in one plan. It draws on two research frameworks: Vonda Wright's healthy-ageing methodology for the body, and Laurie Santos' Science of Well-Being for the mind.*
>
> *It works best on **Opus** — if you're not already on it, switch with `/model opus` before we start.*
>
> *The intake is ~27 questions across 6 short rounds (some are skipped depending on your situation). It takes about 10-15 minutes — then I'll build your combined program as a bookmarkable HTML.*
>
> *Would you like the full program (physical health + well-being), or just one half?"*

## Mode selection

Three modes:

| Mode | What happens |
|---|---|
| **Both** (default) | Unified intake → parallel skill execution → QA → synthesis → one combined HTML |
| **Health only** | Run the health skill in standalone mode → QA check the output |
| **Well-being only** | Run the well-being skill in standalone mode → QA check the output |

In single-skill modes, read the chosen skill's SKILL.md and follow its standalone instructions (including its own intake). After the output is generated, run the QA checklist and fix any issues. The user experience is the same as running the skill directly, plus a QA safety net.

The rest of this document covers **both** mode — the orchestrator's primary use case.

## Hard guardrails

Apply before intake begins:

1. **Age < 18** — refuse. Both sub-skills require adult cohorts.
2. **Pregnancy / TTC / breastfeeding** (sex=F) — refuse to generate the program. Say:
   > *"Pregnancy, breastfeeding, and trying to conceive each come with specific physiological nuances that this program isn't trained to cover safely — supplement interactions, hormonal shifts, exercise thresholds, and fasting risks all change significantly. Please work with a prenatal-aware clinician who can build a plan around your specific situation."*
   Do not proceed.
3. **Clinical screening** — if under professional care for depression/anxiety/trauma, set the well-being skill's disclaimer mode (no refusal, just framing adjustment).
4. **Acute medical conditions** — active cancer, recent MI, uncontrolled severe hypertension, eating disorder → defer to clinician per the health skill.

## Unified intake (both mode)

The two skills share some questions (age, sleep) and each has unique ones. This intake merges everything into one flow, eliminating duplicates. Keep transitional text between rounds to 1-2 lines max.

### Round 1: Gates and demographics
1. **Age** (exact number — <18 guard fires here)
2. **Sex**
3. **Country + city**
4. **Clinical screening** — "Are you working with a therapist or doctor for depression, anxiety, or trauma?"

### Round 2: Physical basics
5. **Pregnancy status** (F only) — if yes, apply pregnancy gate before continuing
6. **Weight + height** (user's natural units; convert internally)
7. **Sleep** — hours, quality (good/okay/poor), AND phone situation: (a) charges outside bedroom, (b) in bedroom on plane mode and not used before bed/after waking, (c) in bedroom and used before bed or after waking. Only (c) is a Tier 1 flag.

### Round 3: Heritage and hormonal (sex/age gated)
8. **Skin type / heritage** (Fitzpatrick I-VI — don't infer from country)
9. **Hormonal contraception** (F only — specific formulation matters)
10. **Hormonal status** (F age 40+ not on HC: cycle status, HRT, symptoms in plain language; M age 35+: TRT status, low-T symptoms)

### Round 4: Current practice audits
11. **Weekly training/activity schedule** — day-by-day with times
12. **Current supplements** — brand, dose, timing (push for specifics)
13. **Current skincare** — AM and PM separately
14. **Recent labs** — even rough recall
15. **Current well-being practices** — multi-select: meditation, journaling, volunteering, therapy/counseling, creative hobbies, regular nature time, spiritual practice, none
16. **Tried and dropped** — well-being practices that didn't stick + what went wrong

### Round 5: Life situation
17. **Social situation** — living arrangement, close friends/family nearby, face-to-face frequency outside work
18. **Leisure screen time** — outside of work hours (social media, scrolling, streaming)
19. **Work meaning** — meaningful vs. pays the bills

### Round 6: Capacity and preferences
20. **Wearable data** — device, VO2max, HRV, RHR, sleep score
21. **Monthly health budget** (gym, supplements, labs, skincare)
22. **Insurance / public health access**
23. **Existing conditions** (yes/no/unknown — unknown triggers screening recommendation)
24. **Goals** — multi-select combining both domains
25. **Total new time budget** — "How many hours per week could you realistically put toward new health AND well-being practices?" This gets split between the two skills.
26. **Health preferences** — HRT/TRT openness, supplement openness, biometric tracking comfort, time-of-day preference
27. **Well-being starting preference** — reflective (journaling, meditation), social (kindness, volunteering, deeper conversations), outdoor (awe walks, nature), creative (flow, strengths). Starting-point signal, not a gate.

#### Travel aside (mention once, don't require)

> *"If you have travel planned in the next 6-12 months to a destination with cheaper private healthcare (Brazil, Mexico, Thailand, parts of EU/SE Asia), let me know — I can build a split diagnostic strategy to save on lab costs."*

## Time budget allocation

Split the total new-time budget approximately **60% physical / 40% psychological**:

- **Physical health gets 60%** because exercise does double duty — it's both the strongest physical health intervention AND one of the strongest mood interventions
- **Psychological well-being gets 40%** because practices like social connection, meaning, and gratitude address what exercise can't

**Adjustment logic:** shift the split based on the user's existing coverage:
- User already trains 4+ hrs/week but has no well-being practices → shift to 50/50 or even 40/60 for new time
- User already meditates, journals, and volunteers but does zero exercise → shift to 70/30 or 75/25
- Both domains well-covered → keep the default 60/40 and note that the program may be short (fewer gaps to fill)

Example: 5 hrs/week total → ~3 hrs health, ~2 hrs well-being (default). If user already runs 5 hrs/week: → ~2 hrs health (refinement), ~3 hrs well-being (new practices).

## Profile construction

After intake, build two structured profiles from the unified answers:

**Health profile** — maps to the health skill's `assets/profile_schema.json`:
- Demographics: age, sex, country, city, weight_kg, height_cm
- Sleep: avg_sleep_hours
- Pregnancy status, skin type, hormonal status, contraception
- Physical audits: current_weekly_schedule, current_supplements, current_skincare_routine, recent_labs
- Capacity: wearable_data, monthly_budget, insurance_plan, existing_conditions, goals, preferences
- `weekly_time_budget_hours`: the **health portion** of the split

**Well-being profile** — structured data for the well-being skill:
- Age, clinical_screening (yes/no)
- Sleep: hours, quality, phone_situation
- Social: living_arrangement, face_to_face_frequency
- Leisure screen time
- Current well-being practices, tried_and_dropped
- Work meaning, free_time
- `time_budget_hours`: the **well-being portion** of the split
- Starting preference
- `orchestrator_mode: true` — tells the well-being skill to skip exercise-as-mood-medicine (the health skill covers exercise comprehensively)

## Skill execution

Spawn two subagents in parallel — one for each skill. Each reads its SKILL.md and operates in profile mode (skip intake, produce the program directly).

**Health skill agent:**
```
Read the skill at <repo>/healthy-lifestyle-program/SKILL.md and follow its profile-mode instructions.

Profile: <health profile JSON>

The user completed intake via the orchestrator. Apply guardrails, load the corpus, run the workflow, and produce the program HTML.

Save to: <workspace>/health_output.html
```

**Well-being skill agent:**
```
Read the skill at <repo>/wellbeing-program/SKILL.md and follow its profile-mode instructions.

Profile: <well-being profile JSON>

orchestrator_mode = true — skip exercise-as-mood-medicine (the health skill covers it).
The user completed intake via the orchestrator. Apply guardrails, load the corpus, run the workflow, and produce the program HTML.

Save to: <workspace>/wellbeing_output.html
```

**Fallback:** if subagents fail (permissions, timeouts), generate both outputs sequentially inline — read each SKILL.md and follow its profile-mode workflow. This is slower but always works.

## QA checklist

After both outputs are generated, read them and verify. Fix issues during synthesis rather than re-running the skill (unless the issue is fundamental — wrong sex routing, missing critical pillar).

### Health output
- Every recommendation source-attributed
- Sex-routing correct (no cross-sex protocols without flag)
- Country availability checked for supplements/labs/drugs
- Audit tables present where user has existing practices
- Cost table present and within budget
- No Wright name-dropping in body (one sourcing note at top)
- Reader-friendly tone (plain language, not clinical jargon)
- Divergences surfaced honestly (not one-sided)
- Weekly schedule uses user's actual class times

### Well-being output
- Scaffolding tone throughout (never "this will make you happier")
- Anti-monitoring warning present ("The one thing that reliably kills these practices...")
- WOOP if-then linked to an obstacle, not a neutral event
- Practices fit within allocated well-being time budget
- No Santos name-dropping in body
- No exercise section (orchestrator_mode respected)
- Sources in collapsible `<details>`
- No nationality gating; inconsistent cross-cultural practices flagged
- Dropped practices not re-prescribed (unless failure was dosing-related)

### Cross-check
- No contradicting sleep recommendations
- No duplicate social/connection content (health skill's "Social, purpose, cognitive" pillar vs. well-being skill's social practices — pick the richer version, fold the other in)
- Combined practices fit the total time budget
- Consistent warm, evidence-based tone across both halves

## Synthesis

Build one combined HTML. The goal is a cohesive document that flows naturally — not two programs stapled together.

### Connecting the two halves

The physical health sections and well-being sections are linked by two research findings worth weaving in:

1. **Exercise is mood medicine.** Exercise is both the strongest physical health lever AND one of the top mood interventions. This connects the health sections to the well-being sections — the reader's exercise prescription is doing double duty.
2. **Well-being predicts physical health.** Longitudinal research shows people who are psychologically well take better care of their bodies. The well-being practices aren't a luxury — they make the health practices stick.

Use these as brief connecting paragraphs between the two halves, not as standalone sections.

### Combined output structure

1. **Title** — "[Name]'s Health & Well-Being Program"

2. **Combined sourcing note:**
   > "This program draws on two research frameworks. Physical health recommendations come from Dr. Vonda Wright's healthy-ageing methodology and credentialed adjacent researchers (Sims, Mosconi, Laukkanen, Phillips, and others). Psychological well-being practices come from Dr. Laurie Santos' Science of Well-Being (Yale) and the primary investigators she cites (Lyubomirsky, Seligman, Kahneman, Fredrickson, and others). Every recommendation traces to a named study. Full citations are in the Sources section."

3. **Profile summary** — one combined table

4. **Disclaimer** (if clinical flag set)

5. **Top priorities** — interleaved from both domains, ranked by actual leverage for this person. Not "health priorities" then "well-being priorities" — just "your priorities." Example ordering for someone with poor sleep, social isolation, and no exercise: (1) Sleep, (2) Movement, (3) Social connection, (4) Digital hygiene.

6. **Why your brain works against you** — from the well-being output. Covers hedonic adaptation, focusing illusion, GI Joe Fallacy as relevant. This section applies to the whole program — including compliance with health practices.

7. **What you already do well** — combined physical + psychological practices, each with brief research validation.

8. **Fix these first** — combined Tier 1 from both skills. Sleep, social, digital, plus health urgencies (labs, conditions). Skip if nothing to fix.

9. **Your body** — health pillars from the health output. Focus on the highest-priority pillars in full; summarize lower-priority ones in a collapsible section. End with the connecting note: exercise does double duty for mood.

10. **Your mind** — well-being practices from the well-being output. Open with: these address what exercise can't.

11. **Making it stick** — WOOP (from well-being output), environmental design tips (combined), anti-monitoring warning. WOOP applies to health habits too — the obstacle-linked if-then is universal.

12. **Your weekly rhythm** — ONE integrated schedule combining physical and well-being practices within the total time budget. This is the key synthesis deliverable. Show both types of activities in one grid so the user sees their whole week.

13. **Budget** — combined cost table (health + any well-being costs, if applicable)

14. **Reassessment** — "In 4-6 weeks, revisit both your physical practices and well-being practices. Drop what feels forced, keep what feels natural. The principle is the same for both: if it doesn't feel like yours, it's not your practice."

15. **Sources** — combined in collapsible `<details>`:
    - Physical health sources (Wright + adjacent)
    - Psychological well-being sources (Santos + primary investigators)
    - COI flags for both domains

### Keeping the combined document readable

The health skill alone can produce 10 pillars of detailed content. Combined with well-being, the document risks becoming overwhelming. To keep it readable:

- **Priority pillars** (top 5-6 by leverage) get full treatment
- **Lower-priority pillars** get a concise summary in a collapsible `<details>` section
- If a pillar has no gaps (user already covers it), acknowledge with a checkmark and move on — don't pad
- The first read should be skimmable in 20-30 minutes; deep detail is available but collapsible

## Output format

### HTML requirements

Same standards as both sub-skills:
- Self-contained (embedded CSS, no external dependencies)
- Clean typography (Georgia/serif, ~860px max-width)
- Print-friendly (break-inside: avoid on cards)
- Warm color palette (earth tones — greens, cream, not clinical blues)
- Cards for practices, callouts for warnings
- Dose badges for key numbers
- Sticky or linked table of contents
- Collapsible `<details>` for depth

### File path

**Combined output:** `happy_and_healthy_program.html`
- WSL: `/mnt/c/Users/[username]/Documents/happy_and_healthy_program.html` → bookmark: `file:///C:/Users/[username]/Documents/happy_and_healthy_program.html`
- Mac: `~/Documents/happy_and_healthy_program.html`
- Linux: `~/Documents/happy_and_healthy_program.html`

**Single-skill output:** use that skill's standard filename:
- Health only: `healthy_ageing_program.html`
- Well-being only: `wellbeing_program.html`

## What this skill does NOT do

- Override either sub-skill's research constraints or guardrails
- Add recommendations from outside the two research corpora
- Produce separate HTML files in both-skills mode (always one combined output)
- Replace the sub-skills — they can still run independently when the user wants just one domain
