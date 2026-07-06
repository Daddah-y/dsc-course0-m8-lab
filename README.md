# Aviation Accident Analysis


## Project Overview

This project analyzes commercial and passenger aircraft safety on behalf of a fictional airline/aircraft insurer client. Using U.S. aviation accident records spanning **1948–2023**, the goal is to identify aircraft makes and models associated with **low rates of total destruction** and **low likelihood of fatal or serious passenger injury**, and to surface the operating conditions that most influence accident severity.

The client is specifically interested in:
- Makes/models that are professional builds and could plausibly still be in active service (assumed 40-year retirement lifespan, i.e. manufactured/active from **1983 onward**)
- **Separate recommendations** for small aircraft vs. larger passenger aircraft
- Statistically robust comparisons (minimum sample sizes enforced throughout)
- The relationship between accident severity and at least two contextual factors (e.g. weather, engine type)

## Dataset

- **Source file:** `Aviation_Accidents.csv`
- **Records:** 82,805 accidents
- **Columns:** 37, including identifiers, location, aircraft make/model, injury counts, weather condition, engine type, phase of flight, and purpose of flight
- Injury columns (`Total.Fatal.Injuries`, `Total.Serious.Injuries`, `Total.Minor.Injuries`, `Total.Uninjured`) contain missing values, which are treated as zero when constructing derived metrics.

## Methodology

### Derived Metrics
- **`Total_Aboard`** — sum of fatal, serious, minor, and uninjured occupant counts per accident
- **`Injury_Fraction`** — fraction of occupants fatally or seriously injured (`fatal / Total_Aboard`)
- **`Is_Destroyed`** — binary flag derived from `Aircraft.damage` indicating whether the aircraft was destroyed

### Small vs. Large Aircraft
Aircraft are split using a **20-occupant threshold**:
| Category | Definition | Records |
|---|---|---|
| Small aircraft | ≤ 20 total aboard | 80,629 |
| Large aircraft | > 20 total aboard | 2,176 |

### Statistical Robustness
Every grouped comparison (by make, by model, by weather, by engine type) is **filtered to groups with at least 10 recorded accidents** before ranking or plotting, to avoid drawing conclusions from small, unstable samples.

## Key Findings

### By Make
Injury and destruction rates were computed per make (`acft_make`) for each aircraft size class, filtered to makes with ≥10 records.

**Best small aircraft makes** (top 5 by injury rate): American Legend Aircraft, Barnes, Head Balloons, Powrachute, Lindstrand Balloons. Best 5 by destruction rate: American Legend Aircraft, Progressive Aerodyne, Evektor-Aerotechnik AS, Aerostar International, Remos Aircraft GmbH.
- **American Legend Aircraft** is the only make appearing in the top 5 of both metrics for small aircraft.

**Best large aircraft makes** (top 5 by injury rate): Saab-Scania AB (Saab), Cessna, Aerospatiale, Canadair, Short Brothers. Top 5 by destruction rate: Saab-Scania AB (Saab), Aerospatiale, Short Brothers, Bombardier, Airbus Industrie.
- **Short Brothers, Aerospatiale, and Saab-Scania AB (Saab)** appear in the top 5 of both metrics for large aircraft.

### By Model
Grouping by Make + Model (≥10 records per group) surfaced specific low-risk aircraft, e.g.:
- **Small:** Maule M-7-235B/MX-7-180/MX7, American Legend Aircraft AL3, Aviat Aircraft A-1B — all with a 0.0 mean injury fraction and 0% destruction rate.
- **Large:** Airbus Industrie A320-232, and numerous Boeing models (727, 717, 757, 767, 777) — all showing a 0.0 mean injury fraction across the qualifying sample.

### Weather Condition
| Condition | Mean Injury Fraction | Count | Destruction Rate |
|---|---|---|---|
| VMC (visual) | 0.1376 | 74,616 | 17.5% |
| UNK (unknown) | 0.5360 | 792 | 51.8% |
| IMC (instrument) | 0.5449 | 5,704 | 55.6% |

Accidents occurring in **instrument meteorological conditions (IMC)** are roughly **4x** more likely to result in fatal/serious injury and **3x** more likely to result in aircraft destruction than those in clear visual conditions (VMC).

### Engine Type
| Engine Type | Mean Injury Fraction | Count | Destruction Rate |
|---|---|---|---|
| Turbo Fan | 0.0623 | 2,270 | 7.8% |
| Unknown | 0.1289 | 738 | 23.1% |
| Turbo Jet | 0.1675 | 689 | 19.9% |
| Reciprocating | 0.1724 | 68,992 | 20.9% |
| Turbo Shaft | 0.1760 | 3,526 | 24.4% |
| Turbo Prop | 0.2144 | 3,305 | 23.6% |

**Turbo fan** engines (used almost exclusively on larger commercial jets) show the lowest injury and destruction rates, consistent with the finding that large aircraft overall are markedly safer than small aircraft (see below). **Reciprocating** engines — the dominant engine type in the small-aircraft fleet — show injury and destruction rates roughly 2–3x higher.

### Overall Summary Statistics
| Metric | All Aircraft | Small (≤20) | Large (>20) |
|---|---|---|---|
| Accidents | 82,805 | 80,629 | 2,176 |
| Mean injury fraction | 0.1733 | 0.1773 | 0.0286 |
| Mean destruction rate | 0.2052 | — | 0.0437 |

Large passenger aircraft show a mean injury fraction roughly **6x lower** and a destruction rate roughly **4.7x lower** than small aircraft, reflecting stronger regulatory oversight, professional crews, and more robust airframes/engines in the large-aircraft category.

## Recommendations

**For small aircraft:** favor **American Legend Aircraft**, which is the only make with both low injury and low destruction rates among makes with sufficient sample size. At the model level, the Maule M-7-235B/MX-7-180/MX7 family and the Aviat Aircraft A-1B stand out with zero recorded fatal/serious injuries or destruction in their qualifying samples.

**For large aircraft:** favor **Short Brothers, Aerospatiale, and Saab-Scania AB (Saab)**, all consistently ranking in the top 5 for both injury and destruction outcomes. At the model level, most modern Boeing narrow/wide-body models (727, 717, 757, 767, 777) and the Airbus A320-232 show a 0.0 mean injury fraction across their qualifying samples.

**Operating conditions matter as much as aircraft choice:** flights conducted under IMC (instrument) weather conditions carry roughly 4x the injury risk of VMC (visual) conditions, and aircraft with turbo fan engines show substantially better safety outcomes than reciprocating-engine aircraft — reinforcing that large, turbo-fan-powered, professionally operated aircraft represent the lowest-risk profile overall.

## Repository Contents

- `Aviation_Accidents_Cleaning.ipynb` — data cleaning and preparation notebook
- `Aviation_Accidents_Data_Analysis.ipynb` — exploratory analysis, visualizations, and recommendations
- `Aviation_Accidents.csv` — cleaned dataset used as input to the analysis notebook
- `README.md` — this file


