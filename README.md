# Slovenian Power Price Risk

**Market risk analysis of Slovenian day-ahead electricity prices (2021–2025)**

Selection project prepared as part of my application for the *Quantitative Developer* position at GEN-I, October 2026.

---

## 🇸🇮 Povzetek

Projekt analizira tržno tveganje na slovenskem dnevnem trgu električne energije. Na podlagi javno dostopnih urnih cen za obdobje 2021–2025 obravnava gibanje cen, njihovo nestanovitnost in tveganje preproste trgovalne pozicije, merjeno z metodo Value-at-Risk (VaR). Zanesljivost ocen je preverjena z backtestingom.

Ključne ugotovitve so povzete v razdelku *Key findings*, celotna analiza pa je v Jupyter zvezkih v mapi `notebooks/`.

---

## Executive summary

Energy trading companies are exposed to market, liquidity and credit risk. Electricity prices are particularly challenging to model: they are highly volatile, seasonal and prone to sudden spikes, as demonstrated by the 2022 energy crisis.

This project measures the **market risk** of a simple position in the Slovenian day-ahead market and evaluates how well a standard risk measure, historical Value-at-Risk, performs when applied to electricity prices.

## Key findings

1. **Price levels:** [povzetek gibanja cen, npr. povprečna cena po letih in vrh v letu 2022]
2. **Volatility:** [povzetek nestanovitnosti in razlik med obdobji]
3. **Extreme events:** [opis cenovnih skokov in "debelih repov" porazdelitve]
4. **Value-at-Risk:** [ocenjen 95 % in 99 % VaR za izbrano pozicijo]
5. **Backtesting:** [število preseganj v primerjavi s pričakovanim in kaj to pomeni]

![Day-ahead prices](figures/prices.png)

## Data

| | |
|---|---|
| **Source** | [ENTSO-E Transparency Platform](https://transparency.entsoe.eu/) |
| **Market** | Day-ahead prices, Slovenian bidding zone (BZN\|SI) |
| **Period** | 1 January 2021 – 31 December 2025 |
| **Resolution** | Hourly |

Only publicly available data is used.

## Methodology

**Data quality.** Raw data was checked for missing hours, daylight-saving time shifts (23- and 25-hour days) and outliers before analysis.

**Descriptive analysis.** Prices were aggregated into daily base (all hours) and peak (weekdays 08:00–20:00) averages and analysed by month and season.

**Volatility.** Daily price changes were calculated and rolling volatility was used to identify calm and stressed market periods.

**Value-at-Risk.** Historical VaR at 95% and 99% confidence levels was estimated for a position of [opis pozicije, npr. 10 MW base for one day].

**Backtesting.** VaR estimates were compared with actual outcomes by counting exceedances against the expected frequency.

## Assumptions and limitations

- Historical VaR assumes that past price changes are representative of future risk, which may not hold during structural changes such as the 2022 crisis.
- The analysis covers a single, simplified position and does not include volume risk, liquidity costs or portfolio effects.
- [dodatne omejitve, ugotovljene med analizo]

## Repository structure
