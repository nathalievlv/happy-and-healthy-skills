# Happy & Healthy

Personalized health and well-being programs built by AI from credentialed research — not generic advice.

These are [Claude Code skills](https://docs.anthropic.com/en/docs/claude-code) that produce a bookmarkable HTML program tailored to your situation. They ask about your current habits, audit what you already do, and prescribe only the gaps — sourced from named studies with sample sizes, not LLM general knowledge.

## What's inside

### Healthy Lifestyle Program
Physical health across 10 pillars: movement, nutrition, supplements, sleep, stress & recovery, hormones, diagnostics, skincare, mindset, and environment. Sex-routed (different protocols for men and women based on validated cohorts), age-tiered, and country-aware (checks supplement/lab availability where you live).

**Research anchor:** Dr. Vonda Wright's healthy-ageing methodology + credentialed adjacent researchers (Stacy Sims, Lisa Mosconi, Jari Laukkanen, Stuart Phillips, Brad Schoenfeld, and others).

### Well-Being Program
Psychological well-being: gratitude, social connection, meaning, flow, kindness, meditation, savoring. Identifies what might be working against you (poor sleep, social isolation, screen overconsumption), then builds practices around the gaps using Person-Activity Fit logic.

**Research anchor:** Dr. Laurie Santos' Science of Well-Being (Yale PSYC 157) + primary investigators she cites (Lyubomirsky, Seligman, Csikszentmihalyi, Kahneman, Fredrickson, and others).

### Happy & Healthy Program (combined)
Orchestrates both skills into one program. Runs a single unified intake, splits your time budget (~60% physical / ~40% psychological), executes both frameworks, and synthesizes into one combined HTML.

## How it works

1. The skill asks you questions in rounds (about 10-15 minutes)
2. It audits what you already do — supplements, training, sleep, existing well-being practices
3. It identifies gaps and things working against you
4. It builds a personalized program as a self-contained HTML file you can bookmark and reopen anytime

Every recommendation is source-attributed to a named study. No generic advice, no unsourced claims.

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (CLI, desktop app, or IDE extension)
- **Claude Opus model recommended** — switch with `/model opus` before running. The skills work on Sonnet but produce significantly better results on Opus.

## Setup

1. Clone this repo into your project directory:
   ```bash
   git clone https://github.com/nathalievlv/happy-and-healthy-skills.git
   cd happy-and-healthy-skills
   ```

2. Open Claude Code in the repo directory:
   ```bash
   claude
   ```

3. Ask for your program:
   ```
   Build me a health and well-being program
   ```
   Or run just one half:
   ```
   Build me a healthy lifestyle program
   ```
   ```
   Build me a well-being program
   ```

Claude will detect the skills automatically and walk you through intake.

## Output

You get a self-contained HTML file — no dependencies, opens in any browser. Bookmark it and come back whenever you want to review your program.

The program includes:
- Audit of what you already do (with research validation)
- Prioritized recommendations with study citations and sample sizes
- A weekly schedule that fits your time budget
- Sources section with full attributions and conflict-of-interest flags

## Important notes

- **Adults only (18+).** The research these skills draw on was validated on adult cohorts.
- **Not for pregnancy, breastfeeding, or trying to conceive.** These situations require specialized clinical guidance.
- **Not a replacement for medical care.** If you're under treatment for any condition, these programs complement your clinician's plan — they don't replace it.
- **Every recommendation is traceable.** If something doesn't have a named study behind it, it's not in the program.

## License

MIT — use it however you want, just keep the copyright notice.
