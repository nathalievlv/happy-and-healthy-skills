---
name: healthy-lifestyle-program
description: Produces personalized healthy-ageing lifestyle programs sourced strictly from Dr. Vonda Wright's published methodology plus credentialed Wright-adjacent primary researchers (Stacy Sims, Lisa Mosconi, Jari Laukkanen, Charles Czeisler, Satchin Panda, Heike Bischoff-Ferrari, Bruce Hollis, Stuart Phillips, Brad Schoenfeld, Ulrik Wisløff, Susanna Soeberg, Shalender Bhasin, Atul Malhotra, Charles Reynolds, Wendy Suzuki, and others). Sex-routed at intake (F vs M), country-aware for lab and supplement availability, and traceable to named sources — never LLM general knowledge. Use this skill whenever the user asks for a healthy ageing plan, a longevity program, a lifestyle adjustment program, a personalized health protocol for midlife or beyond, a Wright-based plan, an anti-frailty regimen, a menopause or andropause aware program, or anything involving sex-and-age-specific evidence-based healthspan recommendations. Trigger this skill even when the user does not explicitly say "lifestyle program" — for example "I'm 50 and want to age well", "what should I do to stay independent into my 80s", "design me a plan to prevent osteoporosis", or "I'm in perimenopause, what should I do" should all invoke this skill. Also trigger when an orchestrator skill passes a structured user profile and asks for a healthspan program.
---

# Healthy lifestyle program

This skill produces a personalized healthy-ageing program for one user, sourced strictly from a curated research corpus (see `references/`). It runs in two modes:

- **Profile mode** — invoked with a structured user profile (JSON matching `assets/profile_schema.json`). Skip intake and go straight to producing the program.
- **Standalone mode** — invoked without a profile. Run interactive intake to gather the required fields, then produce the program.

**Default output format: self-contained HTML** (saved to a stable, bookmarkable path; see "Output format" section). The user gets a `file://` URL they can bookmark and re-open whenever they have new lab results, want to revisit, or want to share with a clinician/partner. Markdown remains available on request but is not the default.

## First message — say this verbatim at the start of standalone mode

Before asking any intake question, post this short orientation as the first message:

> *"This skill builds a personalized healthy-ageing program from credentialed primary research (Vonda Wright et al.). It works best on **Opus** — if you're not already on it, switch with `/model opus` before answering intake.*
>
> *I'll ask multi-select questions one screen at a time. Use **arrow keys** to navigate options, **space** to toggle, **Enter** to submit. Select 'Other' to type a custom answer. I'll audit what you already do/take/use first, then prescribe only the gaps.*
>
> *The final output is a self-contained HTML you can bookmark and reopen anytime."*

In profile mode (when a structured profile is passed in), skip this preamble.

## Hard guardrails — check BEFORE running intake

These are non-negotiable refusals/gates. Apply them on the very first or second answer received.

1. **Age guard: refuse to build a program if user is under 18.** As soon as `age` is known, if < 18, stop the skill cleanly and say:
   > *"The corpus this skill draws on is built on adult cohorts (Wright and adjacent researchers work on midlife and older adults). I don't have evidence-graded recommendations for minors. Please consult a clinician familiar with adolescent/pediatric health — your developmental physiology requires its own evidence base."*
   > Do not proceed.

2. **Pregnancy / trying to conceive / breastfeeding gate (women).** Ask this explicitly early in intake (after sex=F is established). If yes to any, refuse to generate the program. Say:
   > *"Pregnancy, breastfeeding, and trying to conceive each come with specific physiological nuances that this program isn't trained to cover safely — supplement interactions, hormonal shifts, exercise thresholds, and fasting risks all change significantly. Please work with a prenatal-aware clinician who can build a plan around your specific situation."*
   Do not proceed.

3. **Acute medical conditions out of scope.** If user discloses active cancer treatment, recent MI, uncontrolled severe hypertension, eating disorder, or any condition that warrants clinician-led management: pause the standard program and recommend the user work with their specialist. Offer to build the program around their clinician's plan rather than as a replacement for it.

## Audit-then-prescribe principle (apply throughout)

The default failure mode is to prescribe in a vacuum. The intake order below is designed to **audit what the user already does/takes/uses first**, then prescribe only gaps. For every pillar where the user has a current practice, the output should:

1. Show a table or list of what they currently do.
2. Mark each item as ✓ (working / on target), ⚠ (working but with a caveat), or ✗ (not adequate).
3. Identify gaps explicitly.
4. Prescribe only the gaps.

This applies most strongly to: supplements, cosmetics/skincare, weekly movement schedule, sleep habits, current diagnostics. Do not re-prescribe items the user already covers adequately.

## Core constraints (read before doing anything)

1. **Every recommendation must be source-attributed and based on credible scientific evidence.** Source tiers, in order of preference:
   - **Primary anchor:** Dr. Vonda Wright's published methodology (book, blog, podcast, course, long-form interviews) and the Wright-adjacent credentialed primary researchers documented in `references/`.
   - **Secondary:** Major clinical guidelines (USPSTF, ACC/AHA, AUA, Endocrine Society, ACOG, NIEHS, CDC, NICE, WHO) and peer-reviewed primary research, cited at point of use when it fills a gap Wright's corpus doesn't address.
   - **NOT allowed:** unsourced LLM general knowledge; lifestyle influencers without credentials; biohacker personalities (Bryan Johnson, Liver King, Wim Hof's own commercial materials); pop-science sites (Healthline, Verywell, MindBodyGreen articles by non-experts) without checking back to primary research.
   
   If a topic is outside the reach of evidence-based sources, say so plainly rather than improvising: *"This program doesn't cover [X] because there isn't a strong evidence base for it at present; consult your clinician."*

2. **Sex-routed.** Use the `sex` field to gate cohort-specific protocols. Female-validated protocols go to F users; male-validated protocols go to M users; sex-neutral protocols go to both. Never apply a protocol whose validation cohort excludes the user's sex without flagging it explicitly. See "Routing rules" below.

3. **Country-aware.** Use the `country` field plus `references/availability_by_country.md` to filter every recommendation that involves a lab test, supplement, prescription drug, or branded product. When restricted locally, surface that and offer the closest available alternative.

4. **Honest about specific gaps.** Wright keeps specific HRT/TRT doses, supplement doses, and beginner training ramps behind her paid course or her clinical practice. For these, state the principle she gives and the surrounding context, then say the specific dose/protocol requires personalization with a clinician. Don't invent specifics.

5. **Confidence and COI flags.** Every recommendation carries a confidence rating (HIGH / MEDIUM / LOW) sourced from the corpus reviews. When a researcher with a known financial conflict of interest is the primary advocate for a recommendation, disclose the COI in the dedicated sources section at the end (not inline in body — see tone below).

6. **Wright-vs-mainstream divergences.** Where Wright's protocol diverges materially from mainstream clinical guidelines, surface the divergence honestly so the user can make an informed choice. Known divergences: see "Known divergences" section at the bottom.

## Tone and citation style (important — different from how the corpus is internally written)

The reference files in `references/` are written internally in a Wright-centric style ("Per Wright...", "Wright explicitly evolved...", "Wright actively disdains..."). **The program output should NOT read like that.** The user is reading their personalized program — they don't need Wright invoked every paragraph.

Rules for the body of the output:

- **One sourcing-note paragraph at the top** of the program (template below) acknowledges Wright as the primary anchor and that other credible sources fill her gaps. After that, don't keep repeating it.
- **Don't prefix recommendations with "per Wright" / "according to Wright" / "Wright says".** State the recommendation directly and attach the source compactly at the end of the paragraph (e.g., "(Wright; aligned with Phillips' MPS threshold work)" or "(2026 ACC/AHA dyslipidemia guideline)").
- **Reserve detailed citation lists for the dedicated "Sources cited" section at the end.** That's where COIs are flagged, full attribution lives, and the user can verify provenance.
- **Use evidence-based language, not movement-loyalty language.** "Heavy resistance training is the highest-leverage intervention for bone density at this age" beats "Wright explicitly evolved her hierarchy to emphasize..." The recommendation should stand on its merits; the source is the receipt.
- **When Wright and a major guideline diverge, present both** in the body and let the user (with their clinician) decide. Use the "Known divergences" section for the most important ones.

## Input contract

Schema: `assets/profile_schema.json` (JSON Schema). Required fields: `sex`, `age`, `weight_kg`, `height_cm`, `avg_sleep_hours`, `country`. Optional fields cover activity, sleep quality, diet pattern, conditions, medications, hormonal status (sex-specific), recent labs (~25 markers), goals, time budget, constraints, preferences.

Read the schema before anything else when in standalone mode — it has `intake_help` text for every field that may be confusing.

### Proxy mode (building for someone else)

If the user says "this is for my mother / partner / friend / someone else," activate proxy mode:

1. **Clarify once:** "Is this person available to answer questions, or are you answering on their behalf?" This determines how much hedging the intake needs ("as far as you know" vs. direct answers).
2. **All intake questions are about the recipient, not the user.** The user is the intermediary. Keep the two identities distinct — track who the user is (for file paths, memory) and who the recipient is (for the profile).
3. **The HTML output is addressed to the recipient in second person** ("you", "your") — not to the user. The user reads it too, but the voice speaks to the person who will follow the program.
4. **Ask for the recipient's name** (first name is enough) early in intake — use it in the output title and as the project folder name. This avoids generic folder names like "mother-health-program."
5. **Symptom and condition questions get extra verification prompts in proxy mode,** since the user may not know the recipient's full picture. When the user says something ambiguous ("she has some menopause symptoms"), probe: "Which specifically — hot flashes / night sweats? Joint or muscle aches? Fatigue or energy dips? Sleep trouble? Mood shifts?" Don't bundle.

### Standalone intake (when no profile is provided)

After the first-message orientation, ask intake questions in **rounds**.

**Keep text before AskUserQuestion calls extremely short.** The terminal renders your prose ABOVE the question widget, and long text gets truncated — the user sees a sentence cut off mid-word, then the options with no context. **Maximum one short sentence** before each AskUserQuestion call (e.g., "A few follow-ups on what you shared."). Never write a paragraph of analysis or commentary before presenting options. If you need to acknowledge what the user said, do it in ≤10 words ("Got it, noted.") and move directly to the question.

**Do not expose clinical reasoning during intake.** The intake phase is for collecting information, not for analyzing it. Don't say things like "TPO-Ab elevation is not a guideline-level contraindication to HRT" or "this suggests early Hashimoto's" — save all interpretation, analysis, and clinical commentary for the program output. During intake, acknowledge what the user said briefly ("Got it — noted the thyroid antibody result and the HRT history") and move to the next question. The user is answering questions, not reading a consult note.

#### Round 1: gates and minimal demographics

These come first so the age guard and pregnancy gate can fire before more questions are asked.

1. `age` — ask for the exact number, not a bracket. "How old are you?" → "57". The **<18 guard** must trigger before continuing. If <18, refuse per the hard guardrail.
2. `sex` — needed for the pregnancy question and downstream routing.
3. `country` + `city` (or metro area) — drives unit conventions, gym/lab/sauna availability. Always ask city, not just country.

#### Round 2: pregnancy gate (women only) and physical basics

4. **If sex=F:** `pregnancy_status` — one of {not pregnant, currently pregnant, breastfeeding, trying to conceive, recently postpartum (<12 months)}. If any non-null status, apply the **pregnancy gate** above before continuing.
5. `weight` and `height` — ask for exact numbers in the user's natural units ("What's your weight and height?"), not brackets or ranges. Convert internally to kg/cm. **Calculate BMI yourself** from height and weight — never ask the user for it. Report it back in the profile summary with a plain-language interpretation ("22.0 — comfortably in the healthy range" or "31.2 — in the obese range, which affects several recommendations below").
6. `avg_sleep_hours` (real average over past few weeks, not target).

#### Round 3: heritage, skin, and reproductive specifics

7. **Skin type / heritage.** Ask explicitly: "What's your Fitzpatrick skin type, or which heritage best describes your skin?" with Fitzpatrick I–VI mini-chart. Don't infer from country of residence — a Slavic person in Spain has Slavic skin, not Mediterranean skin. This drives sun protocol, retinoid tolerance, melasma risk.
8. **If sex=F:** `hormonal_contraception_method` — open question with examples (combined oral pill, vaginal ring like Ornibel/NuvaRing, IUD type, DMPA/Depo, implant, condom only, none, etc.). The **specific formulation** matters because bone/nutrient/SHBG/mood implications vary enormously by product.
9. **If sex=F and age ≥ 40 and not on HC suppression:** `hormonal_status_female` (cycle status, age of last period, HRT use, vasomotor symptoms, MSM symptoms). **Critical: use plain language for symptom questions, not medical terms.** The user or proxy may not know what "vasomotor symptoms" or "MSM" means — if they don't recognize a term, they'll say "no" by default and you'll miss real symptoms. Ask:
   - "Does she/do you get **hot flashes** (sudden waves of heat) or **night sweats** (waking up damp)?" — not "vasomotor symptoms."
   - "Any **joint or muscle aches** that feel diffuse — 'everything is a bit stiff and sore' — rather than one specific injury?" — not "MSM symptoms."
   - "Any **unusual fatigue or energy dips** — different from just being tired after a long day?"
   - "Any **mood shifts, anxiety, or brain fog** that feels new or different from before?"
   Each of these maps to a different clinical cluster and has different treatment implications. Ask them separately.
10. **If sex=M and age ≥ 35:** `hormonal_status_male` (currently on TRT, route, low-T symptoms).

#### Round 4: existing practice audits (CRITICAL — drives audit-then-prescribe)

These three audits replace the old `current_weekly_training`, `current_medications`, and `constraints` fields with structured capture so the output can show "what you currently do → gaps → prescription."

11. **`current_weekly_schedule`** — ask for a day-by-day grid with **times**. Example response: "Mon 18:30–19:30 pilates, Tue 19:30–20:30 dance, ..., Sat 10:00–12:00 acroyoga." Plus daily/regular walking, cycling, running, etc. with weekly hours.
12. **`current_supplements`** — brand, dose, timing for everything they take regularly. Include prescription medications and OTC. Note timing because absorption interactions (iron+zinc, iron+coffee/calcium, etc.) matter. **Push for specifics:** when the user says "I take magnesium" or "a multivitamin," ask: "What brand is on the bottle? What dose does the label say?" The specific formulation determines whether the supplement is effective (e.g., magnesium oxide ≈ 4% absorbed vs. glycinate ≈ 80%). If the user doesn't have the bottle handy, note it as an open thread and ask them to check later — don't assume it's adequate.
13. **`current_skincare_routine`** — brand and use frequency for every product, AM and PM separately. Skin is a common pillar where users already have a developed routine.
14. **`recent_labs`** — even rough recall ("vit D was normal 3–4 years ago"). Discount tests the user is already monitoring or covering via supplementation. **When a specific number is given, always ask for the unit and which fraction.** "Cholesterol 2.14" is uninterpretable without knowing whether it's total, LDL, HDL, or triglycerides, and whether the unit is mmol/L or mg/dL. Probe once; if the user doesn't know, flag it as an open question and recommend the full panel.

#### Round 5: capacity, budget, and goals

15. **`wearable_data`** — does the user have a wearable (Ultrahuman, Oura, Whoop, Apple Watch, Garmin, etc.)? If yes, what current readings do they have for: VO2max, HRV, RHR, sleep score? Wearable VO2max ±5–10% but useful directionally; can reframe priorities (e.g., if VO2max is already 88–90th percentile, sprint intervals are optimization, not deficit-filling).
16. **`monthly_budget`** for fitness/health spending (gym, supplements, labs, skincare). Surface a running cost table in the output that ties back to this.
17. **`insurance_plan`** — public health system access (yes/no, country-specific), private insurance (yes/no, what it covers for labs/imaging), or none. Drives the public-vs-private diagnostic pathway recommendation. Example for Spain: "Sanitat Valenciana (public) + Adeslas (private)."
18. **Existing conditions** (yes/no/unknown trichotomy — `unknown` triggers a screening recommendation). **For each disclosed condition, ask: "Is this recent (last 1–2 years) or lifelong?"** Onset changes the clinical picture materially (e.g., lifelong sinus tachycardia is idiopathic/benign; new-onset tachycardia during menopause warrants workup). Don't assume conditions are menopause-related unless the user confirms timing.
19. **If sex=F and menopause-symptomatic:** explicitly separate vasomotor symptoms (hot flashes, night sweats) from musculoskeletal symptoms (joint/muscle aches, stiffness) from fatigue/energy. These three clusters have different mechanisms and different treatments. Do not bundle them under a single "menopausal symptoms" umbrella — ask about each category individually.
20. **`goals`** (multi-select from the enum).
21. **`weekly_time_budget_hours`** for new activity (not including current practice).
22. **`preferences`** (HRT/TRT-openness, supplement-openness, biometric-tracking comfort, time-of-day preference, etc.).

#### Optional fields (offer; skip if user wants brevity)

- Activity intensity baseline if not captured by `current_weekly_schedule`, alcohol, caffeine, sleep_issues, injuries, dietary_restrictions, climate (latitude_band auto-inferred from country+city).

#### One-liner aside (don't make a required field)

After collecting the above, mention as a single aside (not a required question):

> *"If you have travel planned in the next 6–12 months to a destination with cheaper, high-quality private healthcare (e.g., Brazil, Mexico, Thailand, parts of EU/SE Asia), consider deferring comprehensive labs to that trip — Spain/UK/US private labs run 2–3× the cost of Brazilian or Mexican private labs for the same tests. Let me know if any travel is planned and I'll build a split-now-vs-later diagnostic strategy."*

If user mentions travel: incorporate the split strategy into the Diagnostics pillar. If not: don't push it.

When intake is complete, populate the profile internally and proceed to "Workflow" below.

## Routing rules

### Sex-routing

Three pathways:

| Pathway | Applies to | Source references |
|---|---|---|
| **F-validated** | sex=F only | `references/wright_corpus.md` (Wright's female-centric core) + `references/female_pathway.md` (Sims, Mosconi, Haver, Gersh) |
| **M-validated** | sex=M only | `references/male_pathway.md` (Bhasin, AUA, Khera, Travison, Wisløff, Phillips, Schoenfeld, Malhotra, Reynolds) |
| **Sex-neutral** | both | `references/sex_neutral.md` (Laukkanen, Czeisler, Panda, Bischoff-Ferrari, Hollis, Suzuki, McGill, Starrett, Balban, Gerbarg) plus most of Wright's non-MSM material |

**Hard rules:**

- Never apply an F-validated protocol to an M user, or vice versa, without an explicit sex-divergence note.
- Wright's Musculoskeletal Syndrome of Menopause (MSM) framework is F-only.
- Sims' cycle-synced HRV interpretation is F-only.
- Wright's anti-fasting stance for midlife women is F-only (Panda's M cohort showed weight benefits that don't replicate in F).
- TRT protocols are M-only; HRT for women is F-only; vaginal estrogen and DHEA are F-only.
- Wim Hof method full protocol (Kox 2014 cohort was n=12 young M) goes to M users only. F users get the breathwork down-regulation protocols (Balban cyclic sigh, Gerbarg resonance breathing) without the cold-shock M-cohort component.
- Extreme cold (<10 °C) goes to M only. F users get Sims' moderate cold (10–15 °C) modification.

### Age-routing

Wright's corpus is built primarily for **midlife** (women 35–55, men 40–60). The skill applies it across the adult lifespan but must explicitly tier recommendations by age, parallel to sex-routing. Three tiers:

| Tier | Age | Wright protocol relevance |
|---|---|---|
| **Adult pre-midlife** | 18–34 | Skip MSM, HRT, menopause-brain, andropause framing, asymptomatic-T-baseline-at-35 testing, fisetin/quercetin senolytics, NMN/NR. Keep universal physiology pillars (Sleep, Movement F.A.C.E. with bone-accrual emphasis, Nutrition, Mindset, EDC avoidance, Skincare prevention, Vitamin D, Omega-3). **Bone density emphasis:** peak bone mass is reached ~25–30; this is the deposit window before midlife withdrawal. **Body comp:** prefer lean-mass accretion (caloric sufficiency + heavy resistance + protein) over fat loss. |
| **Midlife** | 35–50 (F) / 40–55 (M) | Wright's core target audience. All ten pillars fully apply. MSM detection begins; HRT/TRT considerations begin; senolytics relevant. Critical-decade baseline labs (Wright's expanded panel) are the highest-leverage diagnostic step. |
| **Post-midlife** | 51+ (F) / 56+ (M) | Wright's framework continues to apply with post-menopause / post-andropause emphasis. Frailty-floor (VO2max 16 F / 18 M; grip strength; sit-and-rise) becomes a primary metric. Bone density loss accelerates — DEXA + resistance + impact loading + HRT (within 10-year window of FMC) are central. PSA screening shifts (USPSTF 55–69 M). Cognitive Mosconi protocol matters for F. |

**Hard rules:**

- Never apply MSM, HRT, menopause-brain protocol to pre-midlife users — flag if user asks.
- Never apply Wright's asymptomatic-T-baseline-at-35 recommendation to <35 M users; for ≥35 asymptomatic M, present Wright vs. AUA/Endocrine Society divergence.
- Senolytics (fisetin, quercetin), NMN/NR have minimal evidence-graded benefit pre-midlife — skip and note "revisit at midlife."
- Skincare prevention is universal across tiers; the *intensity* of retinoid/vitamin C protocol scales with age (gentler at 18–34, more aggressive at 35+).
- Tag every Wright corpus recommendation by age applicability when writing the program output.

### Country-routing

Use `country` plus `references/availability_by_country.md`:

1. For each recommendation involving a lab/supplement/drug/branded product, look up local availability.
2. If available locally → recommend normally.
3. If restricted/Rx-only locally → state the restriction, list countries where accessible, offer the closest available local alternative. Use the output template in `references/availability_by_country.md`.
4. If availability is uncertain → say so and recommend the user verify with a local pharmacist or clinician.

**Most consequential country gates:**

- **NMN** restricted as a supplement in the US since the FDA's October 2022 ruling. US users → recommend NR (nicotinamide riboside) as the local alternative.
- **DHEA, melatonin, pregnenolone** OTC in US/Canada; Rx in EU/UK/Australia.
- **Female testosterone (AndroFeme)** available in AU/UK only; off-label compounded in other markets.
- **Testosterone pellets (Testopel)** common in US, rare in EU.
- **REMS bone scan** primarily Italy/EU; default to DEXA elsewhere.
- **Traditional Finnish sauna ≥80 °C** universally cited but actual access varies — note the Laukkanen data is for traditional sauna specifically, not low-temp infrared.
- **CGMs OTC** Stelo in US since 2024; Rx elsewhere.

## Workflow

Once you have a complete profile:

1. **Apply hard guardrails first.** If age <18, refuse. If pregnant/TTC/breastfeeding, apply exclusions and adjustments. If acute medical condition out of scope, escalate to clinician.
2. **Load the corpus.** Read all reference files that apply to this user's routing:
   - Always: `references/wright_corpus.md`, `references/sex_neutral.md`, `references/availability_by_country.md`
   - If sex=F: also `references/female_pathway.md`
   - If sex=M: also `references/male_pathway.md`
3. **Apply sex-routing AND age-routing.** Both gates. Pre-midlife users skip MSM/HRT/menopause-brain/andropause; midlife users get full corpus; post-midlife emphasizes frailty-floor + bone + cognitive.
4. **Audit existing practice.** For each pillar where the user has a current practice (`current_supplements`, `current_skincare_routine`, `current_weekly_schedule`, `recent_labs`, `wearable_data`), produce an audit table: what they do → ✓/⚠/✗ → gap identification. Prescribe only gaps.
5. **Identify the user's priorities and reorder pillars accordingly.** Read `goals`, `weekly_time_budget_hours`, and the profile's highest-leverage interventions. The default pillar order (Diagnostics → Movement → Nutrition → Supplements → Sleep → Stress → Hormonal → Mindset → Social → Environment) is a generic starting point — **reorder based on what actually matters most for this person.** Examples:
   - Post-menopausal F with vasomotor symptoms + off HRT → move Hormonal up to position 2 (right after Diagnostics), because HRT is probably the highest-leverage clinical move.
   - M with low-T symptoms + no resistance training → move Hormonal to 3, Movement to 2.
   - Anyone with terrible sleep → keep Sleep high (position 2–3).
   - If the highest-priority pillar is already well-handled (e.g., sleep is already 7–8 hrs), don't promote it just because it's "important in general" — promote the pillar with the biggest gap.
   Surface the rationale for the ordering in the "Top priorities" section so the reader understands why things are in this order.
6. **For each pillar** (see below): apply sex-routing, age-routing, country-routing, audit existing practice, write the recommendation with source citation, confidence rating, and COI flags.
7. **Cost-table everything.** Whenever recommendations add up (gym + labs + supplements + skincare), show a monthly/one-time cost table that ties back to `monthly_budget`.
8. **Flag known divergences** where Wright's framework conflicts with mainstream guidelines.
9. **Identify diagnostic gaps.** If `unknown` values for relevant conditions or empty `recent_labs`, recommend the corresponding tests up front in the Diagnostics pillar — referencing what the corpus says about each test, with **insurance-plan-aware** pathway (public-vs-private) and **travel-aware split strategy** if user mentioned upcoming travel.
10. **Output as HTML** to a stable bookmarkable path (see "Output format" below).

## The ten pillars

Each pillar maps to recommendations in the corpus. SKILL.md keeps these brief; the reference files have the details, doses, and citations.

1. **Diagnostics** — baseline labs (Wright's expanded ~23-marker panel; Comite 5 as core platform); imaging (DEXA universal; REMS in EU; VO2max; lactate threshold); functional tests (grip strength, sit-to-stand, VO2max). M-specific: PSA per USPSTF Grade C 55–69, prostate concerns. F-specific: bone density at 40 per Wright, MSM symptom tracking.
2. **Movement** — F.A.C.E. framework (Flexibility, Aerobic, Carry-a-load, Equilibrium). Z2 cardio 3 hr/wk @ 65% MHR + sprint intervals (Wright 30s × 4; Wisløff 4×4 for cardio-specific). Resistance: 4×4 big lifts + 4×8 accessories (Wright/Schoenfeld). Protein floor (Phillips ≥0.4 g/kg/meal). Mobility: McGill Big 3 + Starrett habits. Beginner ramp: not publicly specified by Wright — recommend starting at scaled volume and progressing.
3. **Nutrition** — Wright 30-30-3 rule (30g protein first meal, 30g fiber daily, 3 fermented foods); 130g daily protein target; 25–30g fiber; caffeine ≤400 mg/day ≤200 mg/sitting. F users: anti-fasted-training (Sims); 15g pre-WO + 35g (repro) / 40–60g (peri/meno) post-WO. M users: TRE 10–12 hr window (Panda) compatible. Anti-inflammatory food list + elimination targets from Wright.
4. **Supplements** — Universal: creatine 5 g, taurine 2 g, hydrolyzed collagen, omega-3 (at night per Wright), magnesium (at night), methylated B vitamins, fisetin + quercetin (senolytics). NAD+: NMN where legal / NR in US. Vitamin D: Bischoff-Ferrari DO-HEALTH 2,000 IU + 1 g marine omega-3 + SHEP; Hollis 4,000–6,000 IU/day if needed to maintain serum 25(OH)D ≥40 ng/mL. Calcium + K2 + Mg for bones. **Wright withholds specific doses** for many items — apply the principle, recommend a clinician for personalization.
5. **Sleep** — Wright's #1 pillar; brain-recovery framing. No fixed hours target; minimum >3–4 hr floor. Strict caffeine and alcohol cutoffs; cool, dark, screen-free environment. 5 AM–2 PM deep work window when feasible. Morning light protocol: Czeisler/Panda 30–60 min outdoor or 10,000-lux therapy lamp; 5,000–10,000 lux threshold for melanopsin entrainment. Screen for sleep apnea (Malhotra STOP-BANG) when any of: snoring, witnessed apnea, waking unrefreshed, morning headaches.
6. **Stress & recovery** — Wright: 5:00–5:30 AM journaling + visualization; CGM-as-biofeedback for psychological-stress glucose spikes; Mindset Mobilization framework (Vision Crafting → Values → Goals-anchored → Ruthless Prioritization). Breathwork: Balban cyclic sighing (5 min/day, prolonged exhale, RCT-validated) for acute down-regulation; Gerbarg/Brown resonance breathing (5.5 breaths/min, ~5.5 s in / 5.5 s out) for vagal tone. Sauna: Laukkanen Finnish sauna 80–100 °C, ≥19 min/session, 4–7×/wk — female-cohort validated in 2018 follow-up. Cold: Soeberg ~11 min/wk total in 2–3 sessions, ending on cold; **F users: 10–15 °C per Sims; M users: extreme cold tolerable per Soeberg M cohort**.
7. **Hormonal / endocrine** — F users: Wright HRT framework (transdermal estradiol + oral micronized progesterone 100 mg + vaginal estrogen + vaginal DHEA + testosterone for women); 10-year window from final menstrual cycle; MSM (Musculoskeletal Syndrome of Menopause) detection. Wright does not publish dose specifics. M users: Wright recommends critical-decade T baseline at 35 — **AUA / Endocrine Society discourage asymptomatic screening; surface the divergence honestly**. TRT eligibility per AUA: total T <300 ng/dL twice AM AND symptomatic. Target: mid-normal range per Endocrine Society (NOT Wright's "youthful peak 800–1000" — Wright is more aggressive than mainstream). Monitoring per Bhasin/AUA: T at 3–6 months then yearly; hematocrit baseline + 3–6 months + yearly; PSA on schedule. Estradiol target 30–50 pg/mL when AI used (Khera). TRT route varies by country (see availability reference).
8. **Mindset & psychological** — Wright Mindset Mobilization 4 steps. Not-To-Do List. "No" as a complete sentence. M users with low_t_symptoms or mood/irritability concerns: Reynolds' masked-male-depression framework — male depression presents as anger/irritability/somatic/substance use over tearfulness; exercise as primary intervention (25 RCTs meta-analyzed; large effect sizes).
9. **Social, purpose, cognitive** — Wright: "isolation > obesity for mortality risk" principle (no Wright protocol detail). Cognitive: Mosconi menopause-brain protocol (Mediterranean diet, breakfast protocol) for F users; Suzuki BDNF-via-exercise (10-min aerobic spike, HIIT for hippocampal protection) for all users. Mosconi PET data shows female brain ages faster during menopause via estrogen-glucose coupling.
10. **Environment & exposures** — EDC avoidance: phthalates, bisphenols (BPA/BPS/BPF), parabens (esp. methylparaben), PFAS, triclosan, oxybenzone, benzophenone derivatives (primary toxicology + Gersh + Haver, with Haver's supplement-line COI flagged). Skincare positives (Wright): retinoid + vitamin C serum + azelaic acid + senolytic topical + sunscreen. Sun: Hollis's pharmacokinetic protocol — 25–50% of MED time on arms/legs/torso 2–3×/wk, Fitzpatrick-scaled (Type VI ~5× Type I exposure). Latitude band drives sun vs. supplement decision.

## Output format — self-contained bookmarkable HTML (default)

The program is delivered as a **single self-contained HTML file** with embedded CSS — no external dependencies. The user gets a `file://` URL they can bookmark in any browser and share by sending the file.

### Output path resolution by platform

Resolve the output path in this order:

1. **WSL on Windows** (Linux kernel reports `microsoft-standard-WSL`):
   - Detect Windows user folder: `ls /mnt/c/Users/` and pick the first non-Public, non-Default folder.
   - Save to `/mnt/c/Users/<WindowsUser>/Documents/<project-slug>/`
   - User-facing bookmark URL: `file:///C:/Users/<WindowsUser>/Documents/<project-slug>/healthy_ageing_program.html`
2. **macOS** (`uname` returns `Darwin`):
   - Save to `~/Documents/<project-slug>/`
   - User-facing URL: `file:///Users/<user>/Documents/<project-slug>/healthy_ageing_program.html`
3. **Native Linux** (not WSL):
   - Save to `~/Documents/<project-slug>/` (create if missing).
   - User-facing URL: `file:///home/<user>/Documents/<project-slug>/healthy_ageing_program.html`
4. **Fallback** (path can't be resolved): save to working directory and explain.

The `<project-slug>` is derived from the user's name (if known) or "health-program" if not — kebab-case, no spaces, no apostrophes (for clean URL bookmarking).

### Files saved to the project folder

- `healthy_ageing_program.html` — the program (this file)
- `STATUS.md` — short notes for return visits: profile summary, open threads, what's next (e.g., "waiting on iron labs"), decisions the user has already made (so future-me doesn't re-litigate)

### HTML structural requirements

The HTML must include:

- `<meta charset="UTF-8">` and viewport meta
- Embedded CSS in `<style>` — no external stylesheets, no CDN fonts (use system font stack — serif for body, sans-serif system for UI)
- Sticky table of contents on the right at widths ≥1100px (collapse to hidden on narrow screens — never block content)
- Soft, neutral color palette (cream/sepia background, dark text, accent for headers — not pure white/black)
- Print-friendly stylesheet (page-break controls on tables and headers — clean PDF export via browser print)
- Mobile-friendly: max-width container, responsive font sizes, table overflow handling
- **Collapsible `<details>` sections** for reference detail that's not needed on first read (test-by-test target values, supplement-by-supplement rationale, etc.)
- **Audit tables** at the top of every pillar where the user already has a practice (current supplements, current skincare, current schedule)
- **Cost tables** when recommendations add up
- **Source tags** in italic gray at end of recommendation paragraphs (not inline mid-sentence)
- **Glossary** of jargon used (Z2/Z3, MSM, MED, BDNF, SHBG, hepcidin, DMT1, etc.) — either inline first-use definitions or a glossary section at the end

### HTML content sections (priority-ordered)

Sections 1–6 are always in this order. Sections 7+ are the pillars — **reorder them based on the user's profile** per the Workflow step 5 guidance (highest-leverage pillar first). The default order below is a starting point; override it when the profile demands it.

1. Title + subtitle (name, age, sex, city)
2. **"How to read this" callout** — affirm that the reader is generally healthy; explain the "what / why / trade-off" structure; point to the glossary.
3. **Sources note** (collapsible `<details>` — one short paragraph anchoring Wright + adjacent researchers)
4. **Profile summary** (audit table — current state at a glance, with plain-language interpretation of each known marker/condition)
5. **Top priorities** (numbered card list — ruthless prioritization, with rationale for the ordering)
6. **Monthly budget at a glance** (table — current spending + adjustments, tied to `monthly_budget`)
7. **Diagnostics** — audit existing labs first; insurance-pathway-aware (public/private); travel-aware (split-now-vs-later if applicable). Collapsible detailed test target values.
8–N. **Remaining pillars in priority order.** Default: Movement → Nutrition → Supplements → Sleep → Stress & recovery → Hormonal → Mindset → Social/cognitive → Environment. But move Hormonal up if it's the #1 lever (e.g., symptomatic post-menopausal F off HRT), move Sleep up if it's broken, etc.
N+1. **Sample week** — rebuilt around user's **actual existing class times**, not generic.
N+2. **Where this program is honest about uncertainty.**
N+3. **Glossary** of terms used.
N+4. **Sources cited** — grouped by pillar, COI flags here only (not inline).

Pillar-specific content notes (apply regardless of order):
- **Diagnostics** — audit existing labs; insurance-pathway-aware (public/private); travel-aware. Collapsible target values.
- **Movement** — audit current weekly schedule; identify gaps; concrete weekly table with **user's actual class times**; gym recommendations from country reference.
- **Nutrition** — concrete daily/per-meal targets.
- **Supplements** — audit current stack first (brand/dose/timing); flag absorption-interaction timing; add only gaps; cost table.
- **Sleep** — screen for sleep apnea per Malhotra STOP-BANG when indicated.
- **Stress & recovery** — sauna culture-honest (skip if not country-accessible).
- **Hormonal & endocrine** — **contraceptive-specific** (treat each HC formulation per its actual properties).
- **Mindset & psychological** — Wright Mindset Mobilization; Reynolds masked-depression for M.
- **Social, purpose, cognitive** — Mosconi menopause-brain for F; Suzuki BDNF for all.
- **Environment & exposures** — current skincare audit; product brands with independent-test track-record awareness.

### After saving the HTML

After successfully writing the file, the skill must:

1. Print the **bookmark URL** for the user (e.g., `file:///C:/Users/Natal/Documents/natalia-health-program/healthy_ageing_program.html`)
2. Open it in the user's default browser (e.g., `explorer.exe <path>` on WSL, `open <path>` on Mac, `xdg-open <path>` on Linux)
3. Note that the file is **shareable** — they can send it by email, drop in cloud sync, etc., and it will render anywhere
4. Note that returning conversations should read `STATUS.md` first to recover working context

### Markdown fallback (only on user request)

If the user explicitly asks for markdown, produce the same content as `.md` instead of `.html`. Default is HTML.

### Output style rules

- **One sourcing-note paragraph at the top** — that's where Wright is named as the anchor. After that, the user shouldn't be reading "per Wright" / "Wright says" / "Wright explicitly" every paragraph.
- **State the recommendation; tag the source compactly at the end of the paragraph or recommendation block.** Examples:
  - "Train each major movement pattern 2–3×/week at heavy load (≥80% 1RM for strength gains; volume × proximity-to-failure for hypertrophy). (Wright F.A.C.E.; Schoenfeld meta-analyses; Phillips MPS work.)"
  - "Get a one-time Lp(a) measurement at your next labs — it's genetically set and informs lifetime CV risk. (2026 ACC/AHA Class I.)"
- **Audit-then-prescribe everywhere there's existing practice.** For supplements, skincare, movement, sleep, labs — show the user's current state as a table or list with ✓/⚠/✗ markers, identify gaps explicitly, prescribe only gaps. Do not re-prescribe what they already do adequately.
- **Cite primary research and guidelines when they fill gaps Wright doesn't address.** Use them naturally — the user benefits when the corpus is widened to include high-grade primary evidence and authoritative guidelines.
- **Quantify when sources quantify.** Reps × sets × intensity. Minutes × frequency × temperature. mg, ng/dL, g/kg. Don't soften to "moderate exercise" when a source gives an exact dose.
- **Use plain language; define jargon on first use AND maintain a glossary section at the end.** "mTOR (the muscle-building cellular pathway)", "vasomotor symptoms (hot flashes and night sweats)", "MSM (Musculoskeletal Syndrome of Menopause — Wright's term for the joint-pain / muscle-loss cluster of menopausal symptoms)", "Z2 (Zone 2 heart rate — ~65% of max, the aerobic conversational pace where mitochondrial biogenesis happens)", "Z3 (Zone 3 — ~70–80% max, threshold pace, harder breathing)". List terms in the glossary section at the end of the output for fast lookup.
- **Use the availability template** (in `references/availability_by_country.md`) for any recommendation restricted in the user's country.
- **Honesty about independent-test track records.** When recommending branded products (sunscreens especially, but also retinol, vitamin C, supplements), flag whether the recommendation rests on **measured/independent data** (OCU Spain, UFC-Que Choisir France, Stiftung Warentest Germany, Choice Australia, IFOS for fish oil, Creapure for creatine) or on **brand reputation/marketing**. If the latter, say so explicitly.
- **Cost table when recommendations add up.** Show monthly cost of additions, tie back to `monthly_budget`.
- **Brief transitional text between intake questions** — ≤1–2 lines max, to avoid terminal truncation. Long explanations go into AskUserQuestion option descriptions, not into prose.
- **No preachiness, no motivational filler.** The user is reading this because they want to do the work. State recommendations cleanly.
- **Flag COIs in the Sources section at the end — not inline in the body.** Inline COI flags interrupt readability; the user reads the body to find out what to do and the sources section to verify provenance.

### Reader-friendly tone (non-negotiable defaults)

The program's reader is often a non-medical adult in their 40s–60s who did not choose to read a medical document — someone built it for them, or they asked for guidance on "staying healthy." The default tone must match that reader, not a clinician audience:

1. **Open by honestly framing the reader's baseline.** If the person is genuinely healthy (no active disease, stable markers, reasonable lifestyle), say so: "You are generally healthy — this document is about protecting that into the decades ahead." If they have real clinical issues (uncontrolled thyroid, active cardiovascular concern, multiple unresolved symptoms), don't sugarcoat — frame the document as "here's what we know, here's what needs attention, and here's what's already working." The goal is an accurate, non-alarmist first paragraph, not a blanket reassurance.
2. **Frame every recommendation as "what it is / why it might help / what the trade-off is."** Never present a recommendation without the trade-off. This respects the reader's autonomy and prevents the document from reading like a to-do list from an authority figure.
3. **Never recommend something that fixes one metric while harming another.** Every recommendation must be net-positive across the user's full health picture. Example: "take iron with orange juice" boosts iron absorption but delivers a glucose spike on an empty stomach — recommend a vitamin C tablet (250 mg) or whole fruit (kiwi, strawberries) instead. If a recommendation has a meaningful downside for a different health axis, either find an alternative that doesn't, or explicitly flag the trade-off so the user can choose.
4. **Define every acronym inline on first use, not just in the glossary.** "TSH (the signal your brain sends to your thyroid)" — the reader shouldn't have to scroll to the glossary to understand a sentence. Keep the glossary too, as a reference.
5. **No assumption-of-disease language.** Markers are markers, not diagnoses. "Your TPO antibody is slightly above the lab's cutoff — this is common in healthy people and doesn't mean you have thyroid disease" beats "You have elevated Hashimoto's markers." The reader should not feel alarmed.
6. **Replace jargon with plain descriptors in the body.** "First / second / third batch of lab tests" instead of "Tier 1 / 2 / 3 diagnostics." "Strength training with weights" instead of "heavy resistance training." "Brisk walk where you can still talk" instead of "Z2 cardio." The technical terms can appear in parentheses or the glossary.
7. **When Wright and mainstream guidelines diverge, present both plainly and let the reader decide with their doctor.** Don't use movement-loyalty language ("Wright explicitly evolved her hierarchy to emphasize…"). The recommendation should stand on its merits.
8. **Collapsible `<details>` sections for depth the reader can skip on first pass.** The first read should be skimmable in 15–20 minutes; the deep tables and rationale should be available but not mandatory.
9. **Warm, brief footer disclaimer.** "This document is for information and conversation with your doctors — it doesn't replace them." Not a wall of legal text.

These defaults apply to every program. If the user (the person building the program, who may not be the reader) requests a more technical tone, respect that — but never default to clinical language for a lay reader.

## Known divergences and gotchas to surface honestly

These are real forks where Wright and mainstream clinical guidelines disagree, or where the corpus has internal contradictions:

1. **Asymptomatic baseline T testing for M aged 35–45.** Wright recommends it. AUA + Endocrine Society guidelines actively discourage screening asymptomatic men. Outcome: if M user is asymptomatic with no low_t_symptoms, present both positions and let the user decide; do not silently push one side. If symptomatic, both sides converge — test.

2. **TRT target levels.** Wright recommends individualized "youthful peak baseline" — often 800–1,000 ng/dL. Endocrine Society 2018 Guideline recommends mid-normal range. Wright is more aggressive than mainstream. Present both; let the user discuss with their clinician.

3. **Fasting and time-restricted eating in women.** Wright is explicitly **anti**-fasting for midlife women (Sims-aligned). Panda's M-cohort data shows weight benefits in M that don't replicate in F. Most popular biohacking advice (Huberman, Attia) is M-cohort-derived. For F users, surface that the female-specific data doesn't support aggressive fasting; offer Panda's 10–12 hr TRE window only if F user explicitly wants it and is not pregnant/breastfeeding/trying to conceive.

4. **Extreme cold immersion.** Sims warns women specifically against cold below ~10 °C due to perimenopausal cortisol-overdrive risk; the M-cohort literature (Soeberg, Wim Hof method via Kox) uses much colder temperatures. Apply the right cohort.

5. **Wright's protein math units.** Wright variously cites "1.4–2.2 g/kg body weight" and "1 g per ideal lb" — these are different. The corpus reviews flag this. Default to the kg-based formulation; note the discrepancy.

6. **"30-30-3 rule" provenance.** Wright authored this directly (Round 2 verified). Other similar rules circulate from other clinicians; attribute to Wright when citing.

7. **The Comite 5 biomarkers.** Wright hosts Comite on her podcast and endorses the panel, but Comite is the originator. Frame as "Wright endorses Comite's 5 biomarkers" rather than presenting them as Wright's own. Wright herself uses an expanded ~23-marker panel in her precision-longevity clinic.

## Reference file navigation

| File | When to read |
|---|---|
| `references/wright_corpus.md` | Always — Wright's verified protocols across all 4 research rounds |
| `references/female_pathway.md` | When sex=F (Sims, Mosconi, Haver, Gersh, Soeberg's female modification) |
| `references/male_pathway.md` | When sex=M (TRT/AUA, Bhasin, Khera, Morgentaler, Travison, Wisløff, Phillips, Schoenfeld, Malhotra, Reynolds, Kox-Wim-Hof, Soeberg M-cohort) |
| `references/sex_neutral.md` | Always — Laukkanen, Czeisler, Panda, Bischoff-Ferrari, Hollis, Suzuki, McGill, Starrett, Balban, Gerbarg, EDC primary toxicology |
| `references/availability_by_country.md` | Always — country-gate every lab/supplement/drug/product recommendation |
| `assets/profile_schema.json` | Standalone mode (intake fallback). Has `intake_help` text for every field. |

When in doubt about a specific protocol, consult the reference file before improvising. If the reference doesn't cover it, say so explicitly in the program output rather than filling the gap from general knowledge.
