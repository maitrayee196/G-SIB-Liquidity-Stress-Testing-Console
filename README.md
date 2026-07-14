[README.md](https://github.com/user-attachments/files/30002389/README.md)
# G-SIB Liquidity Stress-Testing Console

**Live Dashboard:** [https://maitrayee196.github.io/G-SIB-Liquidity-Stress-Testing-Console/](https://maitrayee196.github.io/G-SIB-Liquidity-Stress-Testing-Console/)

An interactive liquidity stress-testing tool for the six U.S. Global Systemically Important Banks (G-SIBs) — **JPMorgan Chase, Bank of America, Citigroup, Wells Fargo, Goldman Sachs, and Morgan Stanley** — built entirely on each bank's own disclosed Basel III regulatory figures, with forward-looking macro stress scenarios layered on top. Everything recalculates live in the browser: a single self-contained HTML file, no backend, no dependencies.

---

## 1. Project Scope & Objective

**What this project does:** builds a liquidity stress-testing model for the six U.S. G-SIBs, using each bank's actual disclosed High-Quality Liquid Assets (HQLA) and 30-day net cash outflows as the baseline, then applies four macro stress scenarios to measure how the Liquidity Coverage Ratio (LCR) — the core Basel III short-term liquidity metric — responds.

**What this project does not do:** it is not a substitute for the Federal Reserve's CCAR/DFAST supervisory stress tests, which use confidential bank-specific behavioral models, granular deposit-mix data, and regulator-defined macro paths. This is a directional, publicly-sourced analytical exercise — the kind of independent framework a Treasury/ALM or FP&A analyst would build to sanity-check disclosed figures against plausible adverse scenarios.

**The three questions this project answers:**

1. How much liquidity buffer does each G-SIB actually hold today — in dollars, and as a percentage cushion above the regulatory minimum?
2. How does that buffer behave under progressively more severe, historically-calibrated stress — from a repeat of the 2022 rate cycle to a repeat of 2008 or 2023?
3. Given where U.S. monetary policy and bank capital regulation stand right now (mid-2026), which scenario is most relevant to actually watch?

---

## 2. Business Problem

Banks fund long-dated, illiquid assets (loans, mortgages, securities) with short-dated, flighty liabilities (deposits, wholesale funding). That maturity mismatch is the core of banking — and the core of banking risk. During a crisis, several adverse events hit simultaneously:

- Large retail and corporate deposit withdrawals
- Wholesale funding markets freeze
- Corporate clients draw down committed credit lines
- Market value of "liquid" assets deteriorates just when they must be sold
- Credit downgrades trigger additional collateral calls

The 2023 collapse of Silicon Valley Bank was a **liquidity failure, not a solvency failure**: the bank was nominally well-capitalized but could not convert assets to cash fast enough to survive a deposit run measured in **hours, not weeks**.

The Basel III **Liquidity Coverage Ratio (LCR)** was designed after the 2008 crisis to prevent exactly this. It requires banks to hold enough HQLA to survive 30 days of severely stressed net cash outflows:

```
LCR = HQLA / 30-Day Net Cash Outflows  ≥  100%
```

But the LCR's stress assumptions are **standardized and regulator-prescribed** — calibrated to a generic severe scenario designed over a decade ago, not to what actually happens in a specific crisis. 2023 proved that uninsured deposit concentration can produce outflow rates the standardized assumptions never anticipated. So this project asks:

> **If we replace the standardized assumptions with stress magnitudes drawn from actual historical episodes (2022 rate cycle, 2008 GFC, 2023 SVB run), how much of each bank's disclosed liquidity buffer survives?**

---

## 3. Macro & Regulatory Context (2022–2026)

### 3.1 The Rate Cycle

The Fed Funds effective rate rose from near-zero (0.08% in January 2022) to a peak of **5.33%** by mid-2023 — the fastest tightening cycle in four decades — in response to post-pandemic inflation. Rates held near peak through most of 2024, then the Fed began cutting in September 2024 and delivered three more cuts in late 2025, landing at the current **3.50–3.75% target range** (effective ~3.63%), held for four consecutive meetings through June 2026.

**What's live right now:** June 2026 FOMC minutes show a genuinely split committee — several participants see a case for **renewed hikes** later in 2026 if inflation stays elevated (citing AI-driven demand, tariff effects, and energy-price risk), while others expect rates to hold or drift lower. This is the first cycle under new Fed leadership that has explicitly moved away from traditional forward guidance, making the path from here less predictable than at any point since 2022.

### 3.2 Bank Capital Rules Are Being Rewritten — Right Now

On **March 19, 2026**, the Federal Reserve, OCC, and FDIC jointly proposed a sweeping rewrite of the **Basel III Endgame** capital framework, formally rescinding the controversial 2023 proposal. The 2023 version would have *raised* aggregate capital requirements at the largest banks by 16–19%; the 2026 re-proposal instead modestly *decreases* them (~4.8% lower required CET1 for G-SIB organizations). Key changes: eliminating the "dual-stack" calculation approach, revising the G-SIB surcharge cliff effect, and removing the internal loss multiplier for operational risk. The comment period closed June 18, 2026; a final rule is pending.

**Why this matters for liquidity, not just capital:** easier capital rules free up balance-sheet capacity, which historically correlates with banks running *tighter* liquidity buffers relative to their minimums — excess HQLA carries a funding cost management prefers to redeploy. A capital-easing cycle arriving at the same moment as renewed rate-hike uncertainty is a combination worth watching.

### 3.3 The 2023 Regional Banking Crisis Still Shapes the Framework

SVB and Signature Bank failed in March 2023 from the combination of long-duration securities losses (driven by the rate hikes above) and highly concentrated, largely uninsured deposit bases that fled within 48 hours. That episode is the direct historical basis for this project's Idiosyncratic Shock scenario, and it is why supervisors now scrutinize uninsured-deposit concentration and securities-portfolio rate risk even as the broader capital framework eases.

---

## 4. Base Data (All Disclosed, Not Estimated)

| Bank | Quarter | HQLA ($B) | Net 30-Day Outflows ($B) | Disclosed LCR | Excess HQLA ($B) |
|---|---|---|---|---|---|
| JPMorgan Chase | 1Q26 | 942.4 | 844.9 | 112% | 97.5 |
| Bank of America | 4Q25 | 666.8 | 595.2 | 112% | 71.6 |
| Citigroup | 1Q26 | 613.9 | 538.5 | 114% | 75.4 |
| Wells Fargo | 1Q26 | 403.2 | 335.5 | 120% | 67.7 |
| Goldman Sachs | 3Q25 / 1Q26 | 494.0 | 386.0 | 128% | 108.0 |
| Morgan Stanley | 1Q26 | 395.1 | 303.9 | 130% | 91.2 |

Every figure comes directly from the banks' SEC 10-Q filings or Pillar 3 LCR public disclosures — nothing is modeled or estimated. (Goldman Sachs combines its Q3 2025 LCR disclosure — the latest standalone LCR percentage publicly available at time of build — with 1Q26 balance-sheet figures; the other five are fully quarter-aligned.)

---

## 5. Methodology

**Stressed LCR:**

```
Stressed LCR = [ HQLA × (1 − haircut add-on) ] ÷ [ Net Outflows × (1 + run-off add-on) ]
```

**Survival horizon (days without market access):**

```
Survival = Stressed HQLA ÷ (Stressed Outflows / 30)
```

**Excess / (shortfall) in dollars:**

```
Excess = Stressed HQLA − Stressed Outflows
```

**Status thresholds:**

| Status | Condition | Meaning |
|---|---|---|
| 🟢 PASS | LCR ≥ 110% | Above internal risk-appetite buffer |
| 🟡 AMBER | 100% ≤ LCR < 110% | Compliant but inside the early-warning zone |
| 🔴 BREACH | LCR < 100% | Below the Basel III regulatory minimum |

Each scenario applies two levers on top of the disclosed baseline: an **outflow run-off add-on** (accelerated deposit flight and credit-line drawdowns beyond what standardized LCR assumptions already capture) and an **HQLA haircut add-on** (market-value deterioration and bid-ask widening on liquid assets under stress, beyond regulatory haircuts already embedded in the disclosed HQLA figure).

---

## 6. Scenario Assumptions & Rationale

| Scenario | Outflow Add-on | HQLA Haircut | Historical Analog | Calibration Rationale |
|---|---|---|---|---|
| **S0 · Baseline** | +0% | +0% | 1Q26 as disclosed | Standardized Basel III run-off and haircut assumptions only — no additional stress |
| **S1 · Moderate** | +20% | +5% | 2022 rate-hike cycle | Deposits migrated industry-wide to money-market funds as short rates rose ~500bps over 18 months; deposit betas rose; unrealized losses widened moderately on fixed-rate HQLA |
| **S2 · Severe** | +60% | +15% | 2008 Global Financial Crisis | Wholesale funding markets effectively closed for weaker credits; committed lines drawn; liquidity hoarding widened bid-ask spreads even on Level 1 / 2A assets |
| **S3 · Idiosyncratic** | +150% | +10% | 2023 SVB / Signature collapse | SVB lost ~25% of deposits in a single day via a digital bank run concentrated among uninsured, correlated depositors — a magnitude the standardized 30-day LCR run-off rates (5–10% retail, 25–40% wholesale) were never designed to capture |

The console also provides **custom sliders** (outflow add-on 0–200%, haircut 0–30%) so any user can construct their own scenario and watch every metric recalculate live.

---

## 7. Results & Key Findings

Headline results by scenario (peer set of six):

| Scenario | Banks PASS | Banks AMBER | Banks BREACH | Weakest LCR | Avg LCR |
|---|---|---|---|---|---|
| S0 · Baseline | 6 | 0 | 0 | 112% (JPM) | 119% |
| S1 · Moderate | 0 | 2 | 4 | 88% (JPM) | 94% |
| S2 · Severe | 0 | 0 | 6 | 59% (JPM) | 63% |
| S3 · Idiosyncratic | 0 | 0 | 6 | 40% (JPM) | 43% |

**Finding 1 — The buffer is real but thin relative to historical stress.** All six banks pass today's standardized regulatory test comfortably. But layering even a moderate, 2022-cycle-magnitude stress on top pushes four of six below 100%. A regulatory pass is necessary but not sufficient evidence of resilience to a genuinely novel shock.

**Finding 2 — Idiosyncratic risk dominates systemic risk.** The SVB-style scenario produces deeper breaches than the 2008-style systemic scenario, despite a lower HQLA haircut — mirroring the actual 2023 lesson that concentrated, digitally-mobile deposit bases can run faster than diversified wholesale funding markets freeze.

**Finding 3 — Regulatory easing and rate uncertainty are colliding.** The March 2026 Basel III re-proposal reduces aggregate capital requirements just as the Fed has left the door open to renewed hikes for the first time since 2023. If liquidity buffers compress from today's levels at the same time deposit costs rise, the moderate-stress breach scenario becomes materially more plausible, not just theoretical.

**Finding 4 — Dollar size ≠ percentage resilience.** JPMorgan holds the largest absolute HQLA buffer ($942B) yet shows the tightest percentage margin under every stress scenario, because its outflow base scales with its enormous deposit franchise. Goldman Sachs and Morgan Stanley — wholesale-funded, less retail-deposit-heavy — retain the widest margins throughout. Buffer size in dollars is not the same as resilience in percentage terms.

---

## 8. Dashboard Features

| Feature | Description |
|---|---|
| **4 preset scenarios** | One-click toggles for Baseline, Moderate, Severe, and Idiosyncratic — each with its historical analog labeled |
| **Custom stress levers** | Sliders for run-off (0–200%) and haircut (0–30%); moving either switches to Custom mode |
| **Per-bank cards** | Stressed LCR headline, PASS/AMBER/BREACH pill, progress bar with the 100% threshold marked, survival horizon in days, and dollar excess/(shortfall) |
| **Drill-down detail** | Click any bank for a base-vs-stressed comparison table across HQLA, outflows, LCR, excess, and survival horizon — with deltas |
| **KPI strip** | Live breach count, amber count, weakest bank, and shortest survival horizon across the peer set |

---

## 9. Limitations

- Stress magnitudes are the author's illustrative calibrations benchmarked to historical episode severity — **not** derived from banks' confidential behavioral run-off models or official Fed CCAR/DFAST parameters.
- The model applies a uniform stress multiplier across each bank's entire outflow base; in reality, deposit concentration, insured-vs-uninsured mix, and funding diversity vary bank-to-bank and would produce differentiated responses this simplified model cannot capture without non-public data.
- This is a point-in-time snapshot (1Q26 baseline); LCR figures move quarter to quarter and the analysis requires refreshing as new disclosures publish.
- The Fed Funds path is reconstructed from FOMC statements and FRED monthly data for illustration; monthly points are approximate averages, not daily effective rates.

**Possible extensions:** deposit-mix granularity (insured vs. uninsured run-off rates), an NSFR (Net Stable Funding Ratio) layer for one-year structural funding, VAR-driven dynamic scenarios where macro shocks propagate into outflow rates over multiple periods, and automated quarterly data refresh from EDGAR.

---

## 10. Tech & Data Stack

- **Frontend:** Vanilla HTML/CSS/JavaScript — single file, zero dependencies, hosted on GitHub Pages
- **Data sources:** SEC EDGAR (10-Q filings), bank Pillar 3 LCR disclosures, FRED (`FEDFUNDS`, `DFEDTARU`), FOMC meeting statements
- **Design:** Custom dark theme (Fraunces / Inter / IBM Plex Mono), responsive down to mobile

---

## ⚠️ Disclaimer

This is an independent, educational scenario-analysis project built entirely from public regulatory disclosures. Stress parameters are the author's own illustrative calibrations benchmarked to historical episodes — they are **not** official Federal Reserve CCAR/DFAST assumptions. Results under stress scenarios are hypothetical simulations, **not** statements about any institution's actual financial condition, safety, or soundness. All six banks currently exceed regulatory liquidity minimums as disclosed. This is not investment advice, and this project is not affiliated with or endorsed by any institution named.

---

## 👩‍💻 Author

**Maitrayee Vishnu** — MS Finance candidate, Stevens Institute of Technology · ex-FP&A Associate, JPMorgan Chase (CIB)

[LinkedIn](https://www.linkedin.com/in/maitrayee-vishnu) · [Portfolio](https://maitrayee196.github.io/Maitrayee_Portfolio/) · [GitHub](https://github.com/maitrayee196)

*Questions, feedback, or ideas for extending the model? Open an issue or reach out.*
