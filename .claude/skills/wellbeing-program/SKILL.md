---
name: wellbeing-program
description: Produces personalized psychological well-being programs sourced from Dr. Laurie Santos' Science of Well-Being framework (Yale PSYC 157, Coursera, The Happiness Lab) and the primary researchers she draws on (Lyubomirsky, Seligman, Csikszentmihalyi, Kahneman, Fredrickson, Dunn, Epley, Keltner, Neff, Emmons, Oettingen, and others). Audit-then-prescribe — identifies what the user already does, flags things working against you (poor sleep, social isolation, digital overconsumption), then prescribes only the gaps. Use this skill when the user asks for a happiness program, a well-being plan, practices for psychological health, how to be happier, stress reduction through positive psychology, help with meaning/purpose/loneliness/burnout, a Santos-based program, or anything involving evidence-based subjective well-being recommendations. Also trigger when an orchestrator skill passes structured intake data and asks for a well-being program.
---

# Well-being program

This skill produces a personalized psychological well-being program for one user, sourced from the Santos/positive-psychology research corpus (see `references/`). It runs in two modes:

- **Profile mode** — invoked with structured intake data (from orchestrator or JSON). Skip intake and produce the program.
- **Standalone mode** — invoked without intake data. Run interactive intake, then produce the program.

**Default output format: self-contained HTML** saved to a stable, bookmarkable path. Markdown available on request.

## First message — standalone mode only

> *"This skill builds a personalized well-being program from credentialed positive psychology research (Santos, Lyubomirsky, Seligman, Kahneman et al.). It works best on **Opus** — if you're not already on it, switch with `/model opus` before answering intake.*
>
> *I'll ask a few questions to understand your current situation — what you already do, what's working, what's not. Then I'll build a program around the gaps.*
>
> *The final output is a self-contained HTML you can bookmark and reopen anytime."*

In profile mode, skip this preamble.

## Hard guardrails

1. **Age guard: refuse if under 18.** Say:
   > *"The research this skill draws on was validated on adult cohorts. I don't have evidence-graded well-being recommendations for minors. Talk to a school counselor or therapist who works with your age group."*

2. **Clinical screening.** If user reports active depression, anxiety disorder, or trauma under professional care:
   - Do NOT refuse to build the program.
   - Add a prominent disclaimer at the top of the output (see "Disclaimer template" below).
   - Frame every practice as "complementary to your professional care, not a replacement."
   - Never suggest reducing or stopping therapy in favor of these practices.

## Audit-then-prescribe principle

Same as the health skill: don't prescribe in a vacuum. For every area where the user already has a practice (meditation, journaling, volunteering, etc.):

1. Acknowledge what they currently do.
2. Assess it: is the dosing/frequency aligned with research? Is it still working or has it gone stale?
3. Prescribe only the gaps or adjustments.

If someone already meditates 10 min/day, don't prescribe meditation — refine it (add loving-kindness variation, suggest variety).

## Core constraints

1. **Every recommendation must be source-attributed.** Source tiers:
   - **Primary anchor:** Santos' platform (Coursera course, Yale PSYC 157, The Happiness Lab podcast, her published work) as pedagogical framework.
   - **Secondary:** Primary investigators cited in `references/research_corpus.md` — these are the actual researchers behind the findings Santos teaches.
   - **NOT allowed:** unsourced protocols, pop-psychology sites, life coaches without research credentials, LLM general knowledge.

2. **Tone: scaffolding, not prescriptions.** This is the most important constraint for this skill.
   - Frame doses as habit scaffolding: "Research found clustering kind acts on one day helps them stand out from your routine — you actually notice what you did. Once it feels natural, the structure becomes invisible."
   - **Never frame any practice in terms of happiness output.** Don't say "this will make you happier." Say "people who do this tend to feel more connected" or "this helps you notice good things you'd normally miss."
   - Content comes from the person, structure comes from the research. The research says *when* and *how often*. The user decides *what*.
   - **Never include happiness self-assessment mid-practice.** No "rate how you feel after your kindness day." The only assessment is Person-Activity Fit (does this feel natural and enjoyable?), not happiness monitoring.
   - Include this warning in the output: "The one thing that reliably kills these practices is checking whether they're working. Do them because they matter to you, not because you're running an experiment on yourself."

3. **Money framing.** When the money-happiness topic is relevant:
   - Lead with the correlation: r = 0.09 — the overall income-happiness link is almost negligible.
   - Describe the threshold functionally: "Happiness stops rising once financial stress is eliminated — you can cover rent, food, healthcare, and handle surprise expenses without panic." No dollar figures.
   - Pivot to spending: earning more barely moves your happiness, but spending what you have on time (outsourcing disliked chores), experiences, health infrastructure (gym, mattress, meal prep), and other people is one of the best investments you can make.

4. **Culture: no nationality gating.** Do NOT route practices based on nationality or cultural background. Instead:
   - Flag which practices have inconsistent cross-cultural results (gratitude journaling and letters triggered guilt in South Korean samples; acts of kindness worked universally).
   - Normalize stopping: "If a practice consistently feels like obligation rather than warmth after 2-3 weeks, drop it and pick a higher-fit alternative. That's not failure, that's the system working."

5. **Confidence and COI flags.** Every recommendation carries a confidence note based on sample size and replication. COI flags (e.g., Neff & Germer's commercial MSC program) go in the Sources section, not inline.

6. **No Santos name-dropping in the body.** Same pattern as health skill with Wright. One sourcing paragraph at the top, then state recommendations directly. The user is reading their program, not a lecture about Santos.

## Intake

### Standalone intake (when no profile is provided)

After the first-message orientation, ask in rounds. Keep transitional text between rounds to 1-2 lines max.

#### Round 1: Gates

1. **Age** — first, because the <18 guard must fire before continuing.
2. **Clinical screening** — "Are you currently working with a therapist or doctor for depression, anxiety, or trauma?" Yes/No. If yes, apply clinical gate (no refusal, just disclaimer + framing adjustment).

#### Round 2: What might be working against you

3. **Sleep** — "How many hours of sleep do you typically get? Would you rate your sleep quality as good, okay, or poor? What's your phone situation at night?" Options: (a) phone charges outside the bedroom, (b) phone is in the bedroom but on plane mode and I don't use it before bed or after waking, (c) phone is in the bedroom and I use it before bed or first thing in the morning. Only option (c) triggers the Tier 1 sleep flag — options (a) and (b) are both fine. *(Orchestrator pre-fills if available.)*
4. **Social situation** — "Do you live alone or with others? Do you have close friends or family nearby? Roughly how often do you see people face-to-face outside of work — daily, a few times a week, weekly, less?"
5. **Leisure screen time** — "Outside of work hours, roughly how much time do you spend on social media, scrolling, or streaming per day?" Clarify: this is leisure only — working on a screen all day doesn't count.

#### Round 3: Existing practices

6. **Current practices** — multi-select: "Do you currently do any of these regularly?" Options: meditation, journaling, volunteering, therapy/counseling, creative hobbies (art, music, writing), regular time in nature, religious or spiritual practice, none of these.
7. **Tried and dropped** — "Have you tried any well-being practices before but stopped? What didn't work about them?" This reveals both failed practices and what doesn't appeal — no separate "what doesn't appeal" question needed.

#### Round 4: Meaning and time

8. **Work meaning** — "Does your work feel meaningful to you, or does it mostly just pay the bills?"
9. **Free time** — "After work and chores, do you feel like you have free time most days, or does your schedule feel constantly packed?"

#### Round 5: Capacity and preference

10. **Time budget** — "How many hours per week could you realistically put toward new practices?"
11. **Starting preference** — "What appeals to you more as a starting point?" Options: reflective practices (journaling, meditation), social practices (kindness, volunteering, deeper conversations), outdoor/physical (awe walks, nature time), creative (flow activities, using personal strengths). This is a starting-point signal, not a gate.

#### Orchestrator handoff

When invoked by the orchestrator with pre-filled data (age, sleep, exercise, schedule), skip rounds 1-2 and ask only rounds 3-5.

## Workflow

Once intake is complete:

1. **Apply guardrails.** If age < 18, refuse. If clinical flag, set disclaimer mode.
2. **Load the corpus.** Read `references/research_corpus.md`.
3. **Identify what's working against you.** From intake round 2:
   - Sleep < 7 hours or poor quality or phone in bedroom AND actively used before bed/after waking → sleep is Tier 1 priority. Phone in bedroom on plane mode with no before-bed/morning use is NOT a leak.
   - Face-to-face social contact < weekly → social connection is Tier 1 priority
   - Leisure screen time > 2 hours/day → digital hygiene is Tier 1 priority
4. **Audit existing practices.** From intake round 3: what they already do, what they tried and dropped (avoid re-prescribing dropped practices unless the failure mode was dosing-related and fixable).
5. **Assess meaning and time.** From intake round 4:
   - Work not meaningful + no values-aligned activity → meaning/purpose is a priority
   - Schedule packed → time affluence framing, outsourcing recommendation
6. **Select practices using Person-Activity Fit logic.** Combine intake preference (round 5) with gap analysis. The number of new practices depends on how many genuine gaps exist — do not pad to fill a quota:
   - Tier 1 (fix negatives): only the leaks that apply to this user
   - Tier 2-3 (add positives): based on preference and gaps, biased toward what they haven't tried yet
   - **High-functioning users:** If the audit reveals no Tier 1 leaks and 4+ existing practices already in place, the user's real gaps may be few or none. In this case the program's primary job is to explain *why* the user's current practices work (with research attribution and sample sizes) and to flag hedonic-adaptation risks on long-running habits. New practices (1-3 max) go in a separate "If you want to try something new" section framed as optional exploration, not prescriptions. Do not invent gaps to fill — a short, honest program is more valuable than a padded one.
7. **Exercise-as-mood-medicine (standalone mode only).** If this skill is running standalone (not invoked by the orchestrator alongside the health skill), add a brief exercise recommendation to the Tier 1 section: 30 minutes of aerobic activity, 3x/week. Don't program the training — just flag it as a mood foundation and suggest the user look into it. If the orchestrator invoked this skill, skip this — the health skill covers exercise comprehensively.
8. **Build the program HTML.** Follow the output structure below.

## Output format

Self-contained HTML (embedded CSS, no external dependencies). Same technical pattern as the health skill.

### Output structure

1. **Disclaimer** (if clinical flag is set):
   > "These are preventative well-being practices, not clinical treatments. You're working with a professional — these practices complement that work, they don't replace it. If any practice increases your distress, stop it and discuss with your therapist."

2. **Sourcing note** (always, one paragraph):
   > "This program draws on Dr. Laurie Santos' Science of Well-Being framework (Yale) and the primary researchers she cites — Lyubomirsky, Seligman, Kahneman, Fredrickson, Keltner, Neff, and others. Every recommendation below traces to a named study. Full citations are in the Sources section at the end."

3. **Why your brain works against you** — Brief (3-4 paragraphs max). Cover only the biases relevant to this user's situation:
   - If they're chasing a raise/promotion → hedonic adaptation + focusing illusion
   - If heavy social media → reference points + social comparison
   - If they tried practices and quit → GI Joe Fallacy (knowing isn't enough)
   - If something is actively working against the user → negativity bias principle: fixing what's broken matters more than adding new things
   - If the user has long-running practices (2+ years) → hedonic adaptation: their brain may have stopped registering the benefits, variety is the antidote

4. **What you already do well** (include when the user has 3+ existing practices) — For each existing practice the user reported, briefly explain why it works according to the research: cite the key study, sample size, and what the research says about their current dosing/frequency. Flag any hedonic-adaptation risks on long-running practices (e.g. 4 years of the same meditation format) and suggest variety or rotation. This section validates the user's existing habits and gives them the research context they're missing. Keep it warm and concise — one short paragraph per practice, not a lecture.

5. **Fix these first** — Only the Tier 1 issues that apply. Skip this section entirely if everything checks out. For each:
   - What the research says (plain language, with sample size)
   - What to change (concrete, specific)
   - The scaffolding framing (why this structure helps)

6. **Your practices** — For users with genuine gaps: the 3-5 personalized practices from Tiers 2-3. For high-functioning users with few gaps: 1-3 optional practices under a header like "If you want to try something new" — framed as exploration, not prescriptions. For each:
   - What it is and why it matters (in warm, human language — NOT "this boosts happiness by X%")
   - The research scaffolding (when, how often, with sample size)
   - How to start (first-week version, low bar)
   - What makes it go stale (hedonic adaptation warning + variety suggestion)
   - One known failure mode to avoid
   - If the practice has inconsistent cross-cultural results (gratitude journaling/letters), add: "If this feels like obligation rather than warmth after 2-3 weeks, swap it for [alternative]. That's normal."

7. **Making it stick** — Three tools:
   - WOOP template (personalized to one of their practices). The if-then plan must respond to the obstacle, not to a neutral event. "If I notice it's Saturday afternoon and I haven't done any kind acts" (obstacle: forgetting) is correct. "If I sit down for breakfast on Saturday" (neutral trigger, not an obstacle) is wrong.
   - Environmental design tip (personalized to their situation)
   - The variety reminder: "Alternate how you do each practice every couple of weeks. Same practice, different method."
   - The anti-monitoring warning: "The one thing that reliably kills these practices is checking whether they're working."

8. **Your weekly rhythm** — A concrete schedule grid mapping all practices into their available time budget. Keep it realistic — if they have 3 hours/week, don't schedule 5 hours of practices.

9. **The money question** — Include only if relevant (user mentioned financial stress, career goals, or work dissatisfaction):
   - r = 0.09 framing
   - Functional threshold description
   - Pivot to smart spending

10. **Reassessment** — "In 4-6 weeks, revisit which practices feel natural and which feel forced. Drop the forced ones, keep the natural ones, and consider adding one new practice from a different category. The Person-Activity Fit principle: if it doesn't score high on Natural, Enjoyable, and Valuable — it's not your practice."

11. **Sources** — Full researcher attributions, study names, sample sizes, years. COI flags here (Neff/Germer MSC commercial tie). No inline citations in the body. Wrap in a `<details><summary>Sources</summary>...</details>` so the section is collapsed by default — users can expand it if they want to check the research.

### HTML requirements

- Self-contained (embedded CSS, no CDN links)
- Clean, readable typography (Georgia/serif, ~860px max-width)
- Print-friendly (break-inside: avoid on cards)
- Warm color palette (greens/earth tones, not clinical blues)
- Cards for practices, callouts for warnings
- Dose badges for key numbers (frequency, duration)
- Sticky or linked TOC for navigation

### File path

Save to a stable path the user can bookmark:
- Determine the user's OS from the environment
- On WSL/Windows: save to `/mnt/c/Users/[username]/Documents/` and provide a `file:///C:/Users/[username]/Documents/` link
- On Mac: save to `~/Documents/`
- On Linux: save to `~/Documents/`

Filename: `wellbeing_program.html`

## Practice reference (quick lookup)

The full research corpus is in `references/research_corpus.md`. Here is the practice menu the skill selects from:

### Tier 1: Fix what's working against you
| Practice | Trigger from intake | Key number |
|---|---|---|
| Sleep optimization | < 7 hrs, poor quality, or phone in bedroom | 909 women — sleep is strongest mood predictor |
| Social connection | Face-to-face < weekly | 308,000 people — isolation = smoking 15 cigs/day |
| Digital hygiene | Leisure screen > 2 hrs/day | 500,000+ — well-being peaks at 0.5-2 hrs |

### Tier 2: Active practices
| Practice | Best for | Key number |
|---|---|---|
| Gratitude journal | Reflective preference | 24,804 across meta-analyses; once/week not daily |
| Gratitude visit | Deep relationships | 411 adults — largest immediate boost in Seligman's trial |
| Acts of kindness | Social preference | 117 people — cluster on 1 day, not spread across week |
| Meditation | Stress, mind-wandering | 2,250 adults — minds wander 47% of time |
| Savoring | Present-moment awareness | Varies — warn against self-monitoring (Mauss, 255 people) |
| Awe walk | Outdoor/nature preference | 60 older adults — small but well-measured study |

### Tier 3: Deeper design
| Practice | Best for | Key number |
|---|---|---|
| Signature strengths | Creative preference, engagement | 411 adults — effects lasted 6 months |
| Flow design | Creative preference, boredom | Csikszentmihalyi — diverse global populations |
| Intrinsic goals | Work dissatisfaction | Kasser — materialism correlates with lower well-being |
| Time affluence | Schedule packed | 6,000+ people — buying time > buying things |
| Meaning & purpose | Work not meaningful | 1,183 people — presence of meaning predicts life satisfaction |
| Self-compassion | Self-critical, perfectionist | Neff — strong correlation with lower depression/anxiety |

## What this skill does NOT cover

- Physical health (exercise programming, nutrition, supplements, labs, skincare, hormones) — that's the health skill's domain. In standalone mode (no orchestrator), this skill briefly recommends exercise-as-mood-medicine (30 min aerobic, 3x/week) as a Tier 1 foundation but does not program training. When running via the orchestrator alongside the health skill, skip the exercise mention entirely.
- Clinical treatment for depression, anxiety, PTSD, eating disorders — this skill is preventative, not therapeutic.
- Pharmacological interventions (SSRIs, anxiolytics, etc.) — refer to clinician.
