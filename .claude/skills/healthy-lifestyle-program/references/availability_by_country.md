# Availability of recommendations by country

**Last verified: 2026-05-18**
**Reference file for the healthy-lifestyle-program skill.** Used to filter and adapt recommendations to the user's `country`. The skill must always close availability claims with: *"Verify with your local pharmacist or clinician — regulations change."*

## How the skill should use this file

For every recommendation that involves a lab test, supplement, prescription drug, or specific branded product:

1. Look up the item below.
2. Match against the user's `country`.
3. If **available locally** → recommend normally.
4. If **restricted locally but available elsewhere** → state the restriction, list countries where it's accessible, and offer the closest available alternative.
5. If **availability uncertain** → say so and recommend the user verify with a local pharmacist/clinician.

Status codes:

- ✅ **Universal OTC** — over-the-counter in essentially every developed market
- 💊 **Universal Rx** — prescription-only in essentially every market that approves it
- ⚠️ **US-restricted** — restricted/banned in the US; freely available elsewhere
- 🇪🇺 **EU/UK-restricted** — Rx-only or restricted in EU/UK; OTC in US
- 🌍 **Region-specific** — clearly more available in some regions than others
- 📦 **Direct-to-consumer with shipping** — sold by the company directly; ships internationally
- ❓ **Verify locally** — too jurisdiction-variable to call confidently

---

## Lab tests and biomarker panels

### Standard panels (✅ universal at any clinical lab in developed countries)

- CBC, comprehensive metabolic panel, lipid panel, fasting glucose, HbA1c, fasting insulin
- TSH, free T3, free T4 (free T3 sometimes only ordered with specialist referral in some EU systems)
- 25-OH vitamin D
- Ferritin, hsCRP
- Total testosterone, free testosterone, estradiol, FSH, LH, SHBG, DHEA-S, progesterone, AMH
- PSA (men)

> Note: continental EU labs often report glucose/cholesterol in mmol/L and vitamin D in nmol/L instead of the US conventions (mg/dL, ng/mL). Orchestrator already handles unit conversion.

### Bone density imaging

- **DEXA scan (DXA)** — ✅ Universal at major hospitals and many radiology clinics. Coverage and copay vary; in some countries (e.g., UK NHS) it requires GP referral and clinical indication. Wright recommends baseline at age 40 — earlier than most national guidelines authorize free coverage, so users may need to pay out-of-pocket.
- **REMS (Radiofrequency Echographic Multi Spectrometry)** — 🌍 Italy-developed; available in Italy, parts of Spain, scattered EU centers, and a small but growing number of US clinics. **If user is not in Europe**, the skill should default to DEXA and note REMS as an emerging alternative.

### Functional / performance testing

- **VO2max test** — Available at sports-medicine clinics, performance labs, and many cardiopulmonary exercise testing (CPET) centers in major cities globally. Cost and accessibility vary; not typically covered by insurance for non-symptomatic adults.
- **Lactate threshold test** — Same network as VO2max; usually paired.
- **Polysomnography / home sleep test** — Universal Rx. Home sleep tests (HSAT) are a cheaper alternative for uncomplicated suspected OSA.
- **Coronary artery calcium (CAC) score / CIMT** — Available at major medical centers globally; coverage varies.

### Direct-to-consumer advanced biomarker tests (📦 ship internationally — but note local regulatory differences)

- **GlycanAge** (glycan-IgG biological age) — UK-based; ships internationally. Mention as available worldwide via mail-in.
- **TruDiagnostic / DunedinPACE** (epigenetic age clocks) — US-based; ships internationally.
- **Jinfiniti NAD+ assay** (intracellular NAD+ measurement, Wright-endorsed) — US-based; ships internationally.
- **DUTCH Test** (4-point cortisol pattern, dried urine) — US-based; ships internationally.
- **Galleri** (multi-cancer early detection) — US-launched, expanding to UK/Australia.

> For all DTC tests: skill should note "your sample ships from your home; results delivered digitally — no clinical referral required in most jurisdictions, but discuss results with your clinician." Some EU countries restrict DTC genetic and epigenetic tests; ❓ verify locally.

### Continuous glucose monitors

- **Stelo (Dexcom, OTC)** — ✅ in US since August 2024 (no Rx needed). 💊 Rx-only in most other countries.
- **Lingo (Abbott, OTC)** — Available OTC in UK; rolling out in US OTC.
- **Levels / Nutrisense** (services that bundle prescription CGMs with software) — US-only.
- **Freestyle Libre** — 💊 Universal Rx for diabetics; off-label use for non-diabetics varies.

### Hard-to-access tests

- **EEG / Nestry brain mapping** (Wright's clinic) — 🌍 Wright's Florida clinic only. The skill should note EEG mapping is part of Wright's precision longevity service, not a standard offering. General-purpose neurofeedback clinics offer roughly comparable but non-protocolized services.
- **Free PSA / 4Kscore / PHI** — US-leaning; limited availability in EU/UK as add-on tests when total PSA is borderline.

---

## Supplements

### Universal OTC (✅ available essentially everywhere)

Creatine monohydrate · Taurine · Hydrolyzed collagen peptides · Omega-3 fish oil (and algal EPA/DHA for vegans) · Vitamin D3 · Vitamin K2 (MK-4 and MK-7) · Magnesium (glycinate, threonate, citrate, malate, oxide) · Quercetin · Fisetin (newer; standardization varies by brand) · Methylated folate (5-MTHF, L-methylfolate) · Methyl-B12 (methylcobalamin) · P-5-P (active B6) · Boron · Strontium citrate (note: not the same as Rx strontium ranelate, which is restricted in EU due to CV risk) · Vitamin C · Zinc · Selenium · Berberine · Inositol · NAC

### Universal OTC but with notable regulatory variation

- **Melatonin** — ✅ OTC in US/Canada; 💊 prescription in most of EU and UK (varying doses and indications).
- **DHEA (oral)** — ✅ OTC in US/Canada; 💊 prescription in EU, UK, Australia.
- **NR (Nicotinamide Riboside)** — ✅ OTC globally as of last verification.
- **Pregnenolone** — ✅ OTC in US/Canada; 💊 Rx in EU, UK.

### ⚠️ US-restricted (banned/restricted in US, freely available elsewhere)

- **NMN (Nicotinamide Mononucleotide)** — As of 2026-05-10, the FDA's October 2022 position remains that NMN cannot be marketed as a dietary supplement in the US because it is being investigated as a drug. Some US retailers still sell it; legality is uncertain. **Available OTC in:** EU, UK, Switzerland, Japan, China, Korea, Australia. **Workaround for US users who want NAD+ precursor support:** switch to NR (nicotinamide riboside), which is fully OTC in the US and serves the same upstream pathway. ❓ Always verify current FDA position — this is a fast-moving area.

### Wright-endorsed branded products (📦 US-based, ship internationally)

- **Unbreakable Bone Health Formula** (Wright's own — MCHA + collagen + Vit D + K2 + Mg + Zn + Cu + Boron + Strontium + Silica) — Ships from `shop.drvondawright.com` to most countries; check shipping list.
- **Kion Essential Amino Acids** — US-based; ships globally.
- **Orion Sleep Systems** (biometric cooling) — US-based; ships to most developed markets but limited service support outside US/Canada.
- **Suji Technologies** (compression/blood flow restriction) — US-based; ships internationally.

---

## Hormone replacement (HRT / TRT) and related Rx

All items below are 💊 prescription. Availability and prescriber willingness vary substantially.

### For women

- **Transdermal estradiol patch** (e.g., Climara, Vivelle-Dot, Estradot) — Available in essentially every market that approves HRT. Brand names vary.
- **Oral micronized progesterone (Prometrium / Utrogestan)** — Available in most markets; brand names vary by region (Utrogestan in EU/UK, Prometrium in US).
- **Vaginal estrogen** (cream / tablet / ring — Premarin, Vagifem, Estring) — Available in most markets.
- **Vaginal DHEA (prasterone, Intrarosa)** — 💊 Approved and available in: US, Canada, EU. ❓ Less common in UK NHS prescribing (specialty menopause clinics). Australia: limited availability.
- **Testosterone for women** (compounded cream, AndroFeme in some markets, off-label use of male formulations at very low doses) — Highly jurisdiction-variable. **AndroFeme** (1% testosterone cream, female-dose) approved in Australia/UK; off-label workaround elsewhere. US: typically off-label use of compounded cream or low-dose male products. ❓ Verify locally — testosterone for women is Wright-endorsed but prescriber willingness varies enormously.
- **Compounded bioidentical hormones** — ❓ Highly jurisdiction-variable. US: widely available via compounding pharmacies. UK: very restricted post-2020 NHS scrutiny. EU: varies by country.

#### Russia-specific HRT brands

Russian pharmacies carry standard European HRT formulations under these brand names:

| Form | Russian brand | International equivalent | Notes |
|---|---|---|---|
| Estradiol patch | **Климара (Climara)**, Дерместрил (Dermestril) | Climara (Bayer) | Twice-weekly patch. First-line transdermal option. |
| Estradiol gel | **Дивигель (Divigel)**, **Эстрожель (Estrogel)** | Divigel (Orion), Estrogel | Daily gel. Fine dose adjustment. |
| Oral micronized progesterone | **Утрожестан (Utrozhestan)** | Utrogestan / Prometrium | One capsule at bedtime. Required with uterus. |
| Oral combined (E + P) | **Фемостон (Femoston)** | Femoston (Abbott) | Convenient but oral → liver first-pass → TBG concern. Second choice if transdermal doesn't suit. |
| Vaginal estrogen | **Овестин (Ovestin)** | Ovestin (Organon) | Cream or suppositories. Ultra-low-dose, local only. Safe even when systemic HRT is contraindicated. |

All are available by prescription from any gynecologist. Prices roughly 1,500–3,500 ₽/month depending on formulation. The skill should recommend transdermal (Климара or Дивигель/Эстрожель) as first-line for any user with thyroid antibody elevation or clotting-risk concern — oral forms raise TBG and clotting factors via hepatic first-pass.

### For men

- **Testosterone cypionate IM injection** — Schedule III controlled substance in US; comparable scheduling in many countries. Available via prescription where TRT is recognized.
- **Testosterone enanthate** — More common in EU than US.
- **Testosterone gel (AndroGel, Testim, Testogel)** — Universal Rx where TRT is approved.
- **Testosterone pellets (Testopel)** — Common in US; rare in EU/UK.
- **Oral testosterone undecanoate (Jatenzo, Tlando, Andriol)** — Andriol available in EU/UK/Australia; Jatenzo/Tlando are newer US-only formulations.
- **Nasal testosterone (Natesto)** — US-Canada primarily.
- **HCG (human chorionic gonadotropin)** — 💊 Rx; restricted/controlled in many countries due to off-label athletic use. Used in Wright/AUA fertility-preserving TRT protocols.
- **Clomiphene / enclomiphene** — 💊 Rx. Off-label TRT alternative use highly jurisdiction-variable; mainstream prescribing only for fertility indications.
- **Anastrozole** — 💊 Rx (oncology drug); off-label TRT use for estradiol management is jurisdiction-variable.

> Skill should always couple TRT recommendations with: "TRT decisions require a clinician familiar with male endocrinology in your country. Diagnostic threshold per AUA: total testosterone <300 ng/dL on two early-morning tests AND symptomatic. Wright's individualized-baseline approach (target 800–1000 ng/dL) is more aggressive than most national guidelines and may not be supported by mainstream prescribers in your jurisdiction."

---

## Equipment and access-dependent practices

- **Weighted vest** — ✅ Universal commercial.
- **Mini-trampoline / rebounder** — ✅ Universal commercial.
- **Foam roller** — ✅ Universal commercial.
- **10,000-lux therapy lamp** — ✅ Universal commercial. Used as substitute when outdoor morning light isn't accessible.
- **HRV monitors:** Whoop, Oura Ring, Polar H10 — Available globally, though some markets have shipping/warranty limitations.
- **Traditional Finnish sauna** — 🌍 Common in Finland (national fixture), Estonia, Sweden, Russia, Korean jjimjilbang. Less common as home installation in US/UK/Australia/Asia outside Korea but available at gyms, spas, and bathhouses. **If user has no access**, the skill should note: "the Laukkanen mortality data is specifically for traditional Finnish sauna ≥80 °C; infrared substitutes have weaker evidence at much lower temperatures."
- **Cold plunge / cold-water immersion** — Specialized commercial units exist globally but expensive. Cold shower is the universal substitute and Sims' modification (10–15 °C cool, not freezing) makes a cold shower a fine compromise.

---

## How to phrase availability findings to the user

When the skill outputs a recommendation that's restricted in the user's country, use this template:

> **Recommendation:** [the recommendation]
> **Availability in [user's country]:** [restricted / requires prescription / available OTC / not approved]
> **Where it's accessible:** [list of countries]
> **Closest local alternative:** [if applicable]
> **Verify with your local pharmacist or clinician** — regulations change.

Example:

> **Recommendation:** Consider NMN supplementation to support intracellular NAD+ levels (Wright protocol).
> **Availability in United States:** Restricted as a dietary supplement since FDA's October 2022 position; legality of US sales is contested.
> **Where it's accessible:** EU, UK, Switzerland, Japan, Korea, Australia (OTC).
> **Closest local alternative:** Nicotinamide riboside (NR), fully OTC in the US, supports the same NAD+ precursor pathway.
> **Verify with your local pharmacist or clinician** — FDA position may have evolved since this reference was last updated (2026-05-10).

---

## Notes on regulatory volatility

These items are unstable enough that the skill should always recommend live verification:

- **NMN US status** (regulatory action ongoing)
- **Compounded hormone availability** (UK/EU regulatory tightening)
- **CGM OTC rollout** (rapidly expanding 2024–2026)
- **Approved prescriber lists for HRT** (varies year-to-year by national health system)
- **Telemedicine prescribing** (jurisdictional rules in flux)

For these, the skill outputs current best-guess + explicit "verify locally before acting" rather than asserting confidently.

---

## Gym chains by country (for resistance-training recommendations)

For every Movement-pillar resistance-training recommendation, suggest 1–3 specific gym chains the user can actually walk into, with rough monthly pricing. Verify the specific branch has a proper free-weight zone (squat rack + Olympic barbell + plate sets + benches + dumbbells up to 30 kg+) before signing — many budget chains' free-weight setups are location-dependent.

### Spain

Sundays-open is a real constraint in Spain — many stores/services close Sunday but the big gym chains stay open.

| Chain | Typical € / month | Sundays open | Notes |
|---|---|---|---|
| **Basic-Fit** | 25–45 | ✓ | Premium tier has 24/7 access. Locations in every major city. Free-weight zone is branch-dependent — verify. |
| **McFIT** | 25–30 | ✓ (24/7) | Budget chain, basic but functional free-weight zones in central locations. |
| **Altafit** | 30–40 | ✓ | Spanish chain with decent rack setup. Strong Madrid/Valencia presence. |
| **Synergym** | 35–45 | ✓ | Spanish chain, often includes sauna — worth checking if sauna access matters. |
| **Forus** | 35–50 | ✓ | Manages many municipal sports centers. Usually pool + sauna. Equipment varies by location. |
| **Anytime Fitness** | 50–65 | ✓ (24/7) | Premium budget. |
| **Holiday Gym / Ágora** | 70–100 | ✓ | Premium chains with pool, sauna, more amenities. |

### United Kingdom

| Chain | Typical £ / month | Notes |
|---|---|---|
| **PureGym** | 20–35 | Budget, 24/7, free-weight zones reliable in larger branches. |
| **The Gym Group** | 20–30 | Budget, 24/7, similar to PureGym. |
| **JD Gyms** | 25–35 | Budget-mid, good free-weight setup. |
| **Nuffield Health** | 60–80 | Premium, pool + sauna often, full equipment. |
| **David Lloyd** | 100–200 | Premium, full-service. |

### United States

| Chain | Typical $ / month | Notes |
|---|---|---|
| **Planet Fitness** | 10–25 | Budget; deprioritized — has "no lunk alarm" policy that discourages heavy free-weight training. |
| **Crunch / Crunch Signature** | 30–60 | Mid-tier, decent free-weight setup. |
| **24 Hour Fitness** | 30–60 | Mid-tier. |
| **Lifetime Fitness** | 100–250 | Premium, full-service. |
| **Equinox** | 200–400 | Premium, full-service. |
| **CrossFit affiliates** | 100–250 | Highly variable; serious strength + community focus. |

### Germany

| Chain | Typical € / month | Notes |
|---|---|---|
| **McFIT** | 25–35 | Budget, ubiquitous. |
| **FitX** | 30–40 | Budget-mid, free-weight zones reliable. |
| **Clever fit** | 25–35 | Budget. |
| **Kieser Training** | 60–100 | Strength-only, science-driven, no cardio. |
| **Holmes Place** | 80–150 | Premium. |

### France

| Chain | Typical € / month | Notes |
|---|---|---|
| **Basic-Fit** | 25–35 | Budget. |
| **Fitness Park** | 30–40 | Mid, French chain. |
| **L'Orange Bleue** | 30–45 | Mid. |
| **Neoness** | 25–40 | Budget-mid. |
| **Club Med Gym (CMG)** | 60–100 | Premium. |

### Italy

| Chain | Typical € / month | Notes |
|---|---|---|
| **Virgin Active** | 60–100 | Premium, common. |
| **McFIT** | 25–35 | Budget. |
| **Fit Active** | 30–45 | Mid, Italian chain. |
| **Get Fit Italia** | 40–60 | Mid. |

### Brazil

Brazilian gym scene is dominated by mid-tier "academias" — typical R$80–250/month (≈ €14–45). Common chains:

| Chain | Typical R$ / month | Notes |
|---|---|---|
| **Smart Fit** | 80–130 | Budget, ubiquitous, Sunday open, free-weight zones reliable. |
| **Bio Ritmo / Bodytech** | 200–400 | Mid-premium. |
| **Selfit** | 80–150 | Budget-mid. |

For Florianópolis specifically: Smart Fit has multiple branches; Bodytech in the higher-end areas.

### Russia

| Chain | Typical ₽ / month | Notes |
|---|---|---|
| **FitnessHouse** | 2,500–3,500 | Largest network in St. Petersburg. Budget, solid basics. Free-weight zones vary by branch — verify before signing. |
| **X-Fit** | 3,000–5,000 | Better-equipped mid-tier, good free-weight zones. Multiple SPb branches. |
| **DDX Fitness** | 2,500–4,500 | Modern budget-to-mid chain, growing fast. |
| **World Class** | 5,000–8,000 | Premium. Often includes banya and pool — worth the premium if user will use both. |
| **Municipal sports centers (бассейн с тренажёрным залом)** | 1,500–2,500 | Cheapest option; equipment quality varies significantly by branch. |
| **Alex Fitness** | 2,000–3,500 | Budget chain with decent locations in Moscow and SPb. |

Before signing: verify the branch has dumbbells to at least 20 kg, leg press, lat pulldown, and chest press machines. Some budget branches max out at 10 kg dumbbells.

### Other markets

When the user's country isn't listed here, the skill should: (1) recommend the user search for major budget chains (Basic-Fit, McFIT, Smart Fit are global), (2) flag that municipal sports centers often have good equipment at low cost, (3) check if any sauna/pool access matters and route to a premium chain if so.

---

## Lab chains by country (for diagnostic recommendations)

For every Diagnostics-pillar recommendation, point to specific lab chains. **Always recommend the public route first when applicable** (free or low-cost via national insurance), then list private chains as the top-up route.

### Spain

- **Public route:** Sanitat Valenciana (Valencia), Servicio Andaluz de Salud (Andalusia), CatSalut (Catalonia), Madrid SaludMadrid, SES (Extremadura), etc. Free if registered, but GPs may push back on comprehensive panels for asymptomatic patients. Usually covers: CBC, CMP, lipid panel, TSH, vitamin D, ferritin. Often won't cover: thyroid antibodies, free T3, full iron studies, fasting insulin, Lp(a), homocysteine.
- **Private chains (walk-in or doctor-requested):**
  - **Synlab** — multiple cities, broad menu.
  - **Megalab / Eurofins-Megalab** — multi-region.
  - **Analiza** (analiza.com) — direct-to-consumer, online ordering, no prescription required for most tests. Excellent self-pay route.
  - **Cerba Internacional** — premium private.
  - **Hospital private labs:** Quirónsalud, Hospital 9 de Octubre (Valencia), IMED, Hospital Ruber Internacional (Madrid), etc.
- **Approximate private prices (2025–2026 reference):** see "Lab pricing reference" below.

### United Kingdom

- **Public route:** NHS via GP referral. Free, but covers a narrower menu than continental EU; thyroid antibodies, free T3, fasting insulin, Lp(a), homocysteine typically not covered unless specific indication.
- **Private chains:**
  - **Medichecks** — direct-to-consumer mail-in (finger-prick or phlebotomy partners). Strong menu.
  - **Thriva** — DTC subscription model.
  - **Nuffield Health** / **Bupa** / **Spire** — private hospital labs with broader menus.
  - **The Doctor's Laboratory (TDL)** — premium.

### United States

- **Public/insurance route:** highly variable by plan. Most insured users get standard labs (CBC, CMP, lipids, TSH, vitamin D, ferritin) covered. Advanced tests (Lp(a), free T3, antibodies, fasting insulin) often require justification.
- **Self-pay direct-to-consumer chains:**
  - **Quest Diagnostics** — direct walk-in via questhealth.com.
  - **LabCorp** — walk-in via OnDemand.
  - **Ulta Lab Tests / Walk-In Lab / Private MD Labs** — DTC aggregators.
  - **Function Health** / **InsideTracker** — subscription comprehensive panels.

### Brazil

- **Public route:** SUS (Sistema Único de Saúde) — free for residents and citizens, narrower menu, longer wait.
- **Private (walk-in friendly, no prescription often required, ISO-certified, world-class quality, 40–60% cheaper than EU/US):**
  - **Fleury** — national premium chain.
  - **DASA / Alta Diagnósticos** — national premium.
  - **Hermes Pardini** — national premium.
  - **Sabin** — national.
  - **Florianópolis specific:** Laboratório Médico Santa Luzia (local), Laboratório São Marcos (local), plus Sabin and DASA branches.

### Germany

- **Public route (gesetzliche Krankenkasse — TK, Barmer, AOK, etc.):** broad coverage if Hausarzt orders. Strict on indication.
- **Private (Privatpraxis or IGeL self-pay):** Synlab, Bioscientia, Limbach, Labor Berlin, Sonic Healthcare.

### Russia

- **Public route (ОМС — обязательное медицинское страхование):** Free for residents via поликлиника with GP referral. Covers CBC, basic biochemistry, TSH, fasting glucose, lipids. Narrow menu — thyroid antibodies, free T3, Lp(a), fasting insulin, DEXA typically not covered without specialist referral. Long waits for specialists. Many users prefer to pay privately for speed and breadth.
- **Private chains (walk-in, no referral required, self-pay):**
  - **Инвитро (Invitro)** — largest national chain. Broad menu, reliable quality. Branches in every major city. Online ordering available.
  - **Хеликс (Helix)** — strong St. Petersburg presence (founded there). Comparable menu to Invitro.
  - **Гемотест (Gemotest)** — national, competitive pricing.
  - **КДЛ (KDL / Клинико-диагностические лаборатории)** — national, mid-tier pricing.
  - **Геномтест (Genomtest)** — newer, competitive.
  - **CMD (Центр молекулярной диагностики)** — national, associated with ЦНИИ Эпидемиологии Роспотребнадзора.
- **Private clinics (imaging, specialist visits, DEXA, echocardiography):**
  - **МЕДСИ** — national premium network.
  - **Скандинавия** — strong St. Petersburg presence, menopause specialists.
  - **СМ-Клиника** — national mid-tier.
  - **СОГАЗ** — corporate-origin but accepts self-pay.
  - **EuroMed** — St. Petersburg premium.
- **Specialist search platforms:** Profi.ru, DocDoc — patient reviews help identify menopause-experienced gynecologists, endocrinologists, cardiologists.
- **No referral needed for private labs.** Walk-in self-pay model. Results typically 1–3 days. Some tests (DEXA, echo, specialist visits) require appointment.

### Other markets

When user's country isn't listed: skill should ask user to name their public system (insurance_plan field) and search for local Quest/LabCorp/Synlab equivalents.

---

## Lab pricing reference (2025–2026 approximate, all in EUR for cross-country comparison)

Use this table to give users honest cost-comparison data. Prices vary by chain and region; verify on actual lab websites before committing.

| Test | Spain private | UK private | US self-pay | Brazil private | Germany private | Russia private | Notes |
|---|---|---|---|---|---|---|---|
| CBC + comprehensive metabolic panel | 25–45 | 60–100 | 30–80 | 8–15 | 30–60 | 15–30 | |
| Lipid panel + chol/HDL ratio | 15–25 | 30–60 | 20–50 | 7–12 | 20–40 | 10–20 | |
| Lp(a) — one-time test | 25–40 | 40–80 | 30–80 | 12–20 | 30–60 | 15–30 | 2024 ACC/AHA Class I universal. |
| Full thyroid panel (TSH + fT3 + fT4 + anti-TPO + anti-Tg) | 60–90 | 80–150 | 100–250 | 25–45 | 70–120 | 35–55 | |
| Ferritin alone | 8–15 | 20–35 | 20–60 | 5–9 | 15–30 | 7–15 | |
| Full iron studies (ferritin + serum iron + TIBC + transferrin sat) | 25–40 | 40–80 | 50–120 | 15–28 | 35–70 | 15–28 | |
| 25-OH vitamin D | 20–35 | 30–50 | 40–100 | 9–18 | 25–45 | 15–25 | |
| B12 + folate | 25–40 | 40–70 | 40–100 | 11–22 | 30–55 | 15–30 | |
| Homocysteine | 25–40 | 40–70 | 40–100 | 12–22 | 30–55 | 15–25 | |
| HbA1c + fasting insulin | 20–30 | 40–60 | 40–100 | 12–22 | 30–50 | 15–25 | HOMA-IR calculated. |
| hs-CRP | 12–25 | 25–45 | 20–60 | 6–12 | 20–35 | 8–15 | |
| RBC magnesium + zinc | 20–40 | 30–60 | 40–100 | 15–25 | 30–50 | 15–30 | |
| Estradiol + FSH + SHBG (F panel) | 50–80 | 60–120 | 80–200 | 20–40 | 50–90 | 25–45 | |
| Total + free testosterone + SHBG (M panel) | 50–80 | 60–120 | 80–200 | 20–40 | 50–90 | 25–45 | |
| DEXA bone density scan | 80–150 | 100–200 | 150–400 | 40–80 | 100–200 | 25–50 | REMS scan more common in Italy. |
| VO2max gas-exchange test | 60–120 | 100–200 | 150–400 | 50–100 | 100–200 | 50–100 | Skill should discount when wearable VO2max available. |
| Cardiologist + ECG + echo | 80–200 | 200–400 | 200–600 | 40–100 | 150–300 | 50–100 | |
| Gynecologist (menopause specialist) | 60–120 | 100–250 | 150–400 | 40–80 | 80–150 | 30–60 | |
| Mammogram | 40–80 | 50–100 | 100–300 | 25–50 | 60–120 | 25–50 | |
| CAC score (CT) | 100–200 | 150–300 | 100–400 | 50–100 | 100–200 | 60–120 | |

Brazil and Russia are consistently cheaper than Spain/UK/Germany; Spain is cheaper than UK/US for most tests; US self-pay highest. Russia's private lab pricing is comparable to Brazil for blood tests, though imaging is slightly higher.

---

## Sauna culture by country (for the Stress & recovery pillar)

The Laukkanen mortality data is specifically for **traditional Finnish sauna at 80–100 °C, ≥19 min/session, 4–7×/week**. Many countries don't have native sauna culture; the skill must be honest about this rather than push the protocol generically.

| Country | Sauna culture | Realistic access | What the skill should say |
|---|---|---|---|
| **Finland / Estonia / Sweden** | Native — homes/saunas widespread | Excellent | Apply Laukkanen protocol fully. |
| **Russia / Latvia / Lithuania** | Native (banya / pirts) | Excellent | Apply protocol; banya is high-temp Finnish-equivalent. |
| **Germany / Austria / Switzerland** | Strong sauna culture | Good (many gyms + dedicated spas) | Apply protocol. |
| **Netherlands** | Good | Decent | Apply protocol. |
| **South Korea** | Strong (jjimjilbang) | Excellent | Korean dry sauna chambers can match Laukkanen temps; jjimjilbangs are accessible. |
| **Japan** | Strong (super-sento, traditional inns) | Good | Sentos increasingly include sauna. |
| **US / Canada / Australia / UK** | Weak native; gym-dependent | Moderate (some gyms have ≥80 °C dry saunas) | Verify specific gym branch; many "saunas" in budget gyms are infrared at 50–60 °C — not the protocol. |
| **Spain / Italy / Portugal / France / Greece** | Weak | Limited — hammams more common (35–50 °C, not the protocol) | Be honest: hammams are relaxation, not the Laukkanen protocol. Recommend the user (a) join a gym with verified dry sauna, or (b) skip the sauna pillar. |
| **Mexico / Brazil / Argentina** | Weak | Limited; some academias have small dry saunas | Check the gym's sauna temp before relying on it for the protocol. |
| **MENA region** | Hammam culture (lower temp) | Limited | Same as Spain — be honest. |
| **South Asia / SE Asia** | Weak in most countries | Limited; growing in Thailand wellness scene | Skip or verify access. |
| **Sub-Saharan Africa** | Minimal | Very limited | Skip the pillar honestly. |

For users where sauna access is limited, the skill should explicitly say "skip this pillar" rather than pushing infrared/hammam alternatives as substitutes — the evidence base for those is much weaker.

---

## Hormonal contraceptive (HC) brand reference

Most users say "I'm on hormonal contraception" without specifics. The skill must ask for the specific brand/method and adjust the program accordingly. Here's a reference for the most common brands and what they imply for the program.

| Brand / generic | Method type | Hormones | Route | Bone density impact | Nutrient depletion magnitude | SHBG effect | Notes |
|---|---|---|---|---|---|---|---|
| **Combined oral pills** (Yaz, Yasmin, Microgynon, Diane-35, Levlen, Loestrin, many more) | Combined oral | EE + various progestins | Oral | Minimal/none (estrogen is bone-protective) | Highest of HC types (hepatic first-pass) — deplete B6, B12, folate, magnesium, zinc, selenium, vit C, vit E | Significant ↑ SHBG → ↓ free T | Most-studied HC class. |
| **NuvaRing / Ornibel / Etoring** | Combined vaginal ring | Etonogestrel + EE | Vaginal | Minimal/none | Lower than oral COCs (avoids first-pass) but still real | Moderate ↑ SHBG | Avoids hepatic first-pass. |
| **Evra / Xulane patch** | Combined transdermal | Norelgestromin + EE | Transdermal | Minimal/none | Lower than oral; still some | Moderate ↑ SHBG | Higher VTE risk than oral. |
| **Cerazette / Slynd (drospirenone-only)** | Progestin-only pill (POP) | Drospirenone or desogestrel | Oral | Minimal/none | Lower than combined | Minimal ↑ SHBG | No estrogen — different profile. |
| **Mirena** | Hormonal IUD | Levonorgestrel | Intrauterine | Minimal/none (local effect) | Negligible — minimal systemic absorption | Minimal | Lasts 5–8 yrs. |
| **Kyleena / Skyla / Jaydess** | Hormonal IUD (lower-dose) | Levonorgestrel (lower) | Intrauterine | Minimal/none | Negligible | Minimal | Lower hormone dose; lasts 3–5 yrs. |
| **Copper IUD (Paragard, Mona Lisa, T-Safe)** | Non-hormonal IUD | None | Intrauterine | None | None | None | No hormonal effect. |
| **Nexplanon / Implanon** | Subdermal implant | Etonogestrel | Subdermal | Minimal/none | Minimal | Minimal | Lasts 3 yrs. |
| **Depo-Provera / DMPA** | Depot injection | Medroxyprogesterone acetate | IM injection q3 months | **⚠ DOCUMENTED bone density loss** — FDA black-box warning for long-term use | Variable, less studied | Different profile | The one HC method with clear bone-density concern. Flag in program. |

**Hard rules for the program:**

- **DMPA users get a bone-density flag** — DEXA earlier than usual; calcium + vitamin D + K2 + resistance training emphasized; consider DMPA-alternative conversation with gynecologist if user is at higher osteoporosis risk.
- **Combined oral pill users get B-complex emphasis** — B6/B12/folate/magnesium depletion most documented in this class.
- **Vaginal ring + transdermal patch users get B-complex but with milder framing** — depletion is real but less than oral.
- **IUD users (hormonal or copper) get the universal program** — minimal hormonal/nutrient implications.
- **All HC users with estrogen component (combined methods): bone density NOT a primary concern** from HC itself.

---

## Common iron supplement formulations by country

For users on iron supplementation, the skill should know typical elemental iron content of common local brands to give accurate dose calibration.

### Spain (and broader EU)

| Brand | Form | Elemental iron per dose | Notes |
|---|---|---|---|
| **Ferro-Gradumet** | Slow-release ferrous sulfate, 525 mg tablet | ~105 mg | Most common Spanish Rx. |
| **Tardyferon** | Slow-release ferrous sulfate, 256.3 mg | ~80 mg | Common in EU. |
| **Ferplex 40** | Ferric protein succinylate (liquid) | 40 mg | OTC liquid, gentler on gut. |
| **Glutaferro** | Ferric glycinate (liquid) | 30 mg | OTC liquid. |
| **Ferro-Sanol duodenal** | Ferrous glycine sulfate | 100 mg | German Rx. |

### United States

| Brand | Form | Elemental iron per dose |
|---|---|---|
| Generic ferrous sulfate 325 mg | Ferrous sulfate | ~65 mg |
| Slow Fe | Slow-release ferrous sulfate | ~50 mg |
| Feosol | Carbonyl iron / ferrous sulfate | ~45–65 mg |
| Ferralet | Ferrous gluconate | ~38 mg |
| Bisglycinate brands (Thorne, Pure Encapsulations) | Iron bisglycinate | 25–50 mg | Gentlest on gut; recommended when standard ferrous sulfate causes constipation/nausea. |

### Brazil

| Brand | Form | Elemental iron per dose |
|---|---|---|
| **Noripurum** | Iron polymaltose | 100 mg | Common Brazilian Rx, gentle. |
| **Fer-In-Sol** | Ferrous sulfate (drops) | ~25 mg/mL | Pediatric/adult liquid. |
| **Combiron** | Multiple formulations | varies | Common OTC. |

### Russia

| Brand | Form | Elemental iron per dose | Notes |
|---|---|---|---|
| **Сорбифер Дурулес (Sorbifer Durules)** | Ferrous sulfate slow-release | ~100 mg | Most common Russian Rx for iron deficiency. |
| **Ферлатум (Ferlatum)** | Ferric protein succinylate (liquid) | 40 mg | OTC liquid, gentle on gut. |
| **Тардиферон (Tardyferon)** | Slow-release ferrous sulfate | ~80 mg | EU formulation available in Russia. |
| **Мальтофер (Maltofer)** | Iron polymaltose (drops/tablets) | 50–100 mg | OTC, gentle. Common pediatric/adult choice. |
| Bisglycinate brands (Solgar, Now Foods, Doctor's Best — available on iHerb or Ozon) | Iron bisglycinate | 25–36 mg | Gentlest on gut; good when standard ferrous sulfate causes constipation/nausea. |

Note: iHerb.com ships to Russia and is widely used for international supplement brands (Solgar, Now Foods, Doctor's Best, Thorne, etc.) that may not be stocked in local аптеки. Delivery 2–3 weeks. Ozon and Wildberries also carry many of these brands domestically.

### Stoffel et al. every-other-day dosing

Across all formulations: recent research (Stoffel et al. 2017, 2020) shows that **daily iron supplementation paradoxically reduces fractional absorption** — taking iron elevates hepcidin (the iron-regulating hormone) for ≥24 hours, blocking the next day's dose. **Every-other-day dosing can absorb 2–3× more iron per mg supplemented.** When recommending iron-dose calibration, mention this — particularly when ferritin is in the 30–80 ng/mL range (where the user is "moving up but slowly").

---

## Independent SPF testing references

For Skincare/SPF recommendations, the skill must distinguish between **brand reputation** and **independent test data**. The SPF industry has a real measured-vs-stated SPF problem; popular brands are not automatically reliable.

### Independent test sources (link these in output)

| Source | Country | Notes |
|---|---|---|
| **OCU** (Organización de Consumidores y Usuarios) | Spain | The most relevant source for Spanish users. Published annual SPF tests with named winners/losers. |
| **UFC-Que Choisir** | France | Annual SPF tests. |
| **Stiftung Warentest** | Germany | Annual SPF tests; very rigorous. Often surfaces budget brands that outperform premium. |
| **Choice** | Australia | Annual SPF tests. Australia has the highest UV exposure and strictest SPF testing protocols globally. |
| **Consumer Reports** | US | Annual SPF tests, less comprehensive than EU/AU counterparts. |
| **Which?** | UK | Annual SPF tests. |

### SPF brand track record (independent testing, cross-referenced 2020–2025)

Generally STRONG track record (multiple independent tests passed across multiple countries):

- **La Roche-Posay Anthelios line** — most consistent independent-test record. The newer UVMune 400 formulation uses Mexoryl 400 filter (long-UVA, 380–400 nm) — addresses the gap most products miss.
- **Avène Sunsimed / Avène Solaires** — generally passes.
- **Bioderma Photoderm line** — generally passes.
- **Eucerin Sun** — generally passes.
- **Nivea Sun** (budget) — surprisingly consistent performer, often outperforms premium brands in Stiftung Warentest.
- **Garnier Ambre Solaire** (budget) — generally passes consumer tests.
- **Cetaphil Sun** — generally passes.

Generally WEAKER track record OR lacking independent verification:

- **ISDIN Fotoprotector line** — has failed OCU testing in some rounds. Flag as "popular pharmacy brand without strong recent independent test record."
- **Heliocare 360°** — heavily marketed but hasn't been part of major comparative consumer-testing rounds. Flag as "marketed extensively but lacking independent comparative test data."
- **Many "organic / natural / clean" SPF brands** — frequently fail SPF testing badly because the avoidance of standard filters compromises measured SPF.
- **High-end luxury brands** (e.g., La Mer, etc.) — rarely tested independently; the price doesn't track performance.

### Tinted SPF for melasma prevention

For users with melasma risk factors (hormonal contraception + Fitzpatrick II–IV + sunny climate): recommend a **tinted SPF** specifically. The iron oxides in tinted SPF block visible/HEV light, which is a major driver of melasma — and untinted SPF doesn't protect against this even at SPF 50.

**Tinted SPF options with strong independent records:**

- **La Roche-Posay Anthelios UVMune 400 Fluid Tinted SPF 50+** (~28–32 €) — Mexoryl 400 + iron oxides. Strongest independent record.
- **Bioderma Photoderm Cover Touch Mineral SPF 50+** or **Nude Touch SPF 50+** (~25–30 €) — fully mineral option + iron oxides.

**Be explicit about evidence:** for melanoma prevention, tinted ≈ untinted (UV protection is from filters, not tint). For melasma/pigmentation prevention, tinted is meaningfully better (iron oxides block visible/HEV light).

---

## Wearable independent validation reference

For the `wearable_data` intake field. Independent ECG-vs-wearable validation (Dial et al., The Physiological Society 2025, 536 nights):

- **Oura Ring 4:** CCC = 0.99 (highest accuracy for nocturnal HRV).
- **WHOOP 5.0:** CCC = 0.94.
- **Apple Watch:** moderate.
- **Garmin / Polar chest strap:** chest straps highest for HR; HRV varies.
- **Ultrahuman Ring AIR:** less independent validation but uses similar finger-arterial method as Oura.

For VO2max estimates from wearables: typically within ±5–10% of lab gas-exchange test. Useful directionally; not a substitute when the absolute number is critical (e.g., medical clearance for HIIT in older adults with cardiac history).
