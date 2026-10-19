# Energy Trading Risk Analysis: Slovenian Power Market

**Market, liquidity and credit risk of an electricity trading position (2021–2025)**

Selection project prepared as part of my application for the *Quantitative Developer* position at GEN-I, October 2026.

---

## 🇸🇮 Povzetek

Projekt analizira tri ključna tveganja trgovca z električno energijo: tržno, likvidnostno in kreditno. Na podlagi javno dostopnih urnih cen slovenskega dnevnega trga za obdobje 2021–2025 obravnava hipotetično pozicijo nakupa električne energije in ocenjuje, koliko lahko podjetje izgubi zaradi gibanja cen, koliko gotovine potrebuje za borzne varščine in kolikšna je izpostavljenost do nasprotne stranke.

Ključne ugotovitve so povzete v razdelku *Key findings*, celotna analiza pa je v Jupyter zvezkih v mapi `notebooks/`.

---

## Executive summary

Energy trading companies are exposed to three closely linked risks. A single price movement simultaneously changes the value of a position (**market risk**), triggers cash margin payments to the exchange (**liquidity risk**), and changes how much a counterparty owes (**credit risk**). The 2022 energy crisis showed how quickly these risks can escalate together.

This project analyses one hypothetical trading position from all three perspectives, using public data from the Slovenian day-ahead market.

## Position analysed

A trader purchases **10 MW baseload** electricity for delivery in the following year at a fixed price:

| Component | Volume | Contract type | Main risk analysed |
|---|---|---|---|
| Exchange-traded | [npr. 5 MW] | Futures | Liquidity (margining) |
| Bilateral | [npr. 5 MW] | Forward | Credit (counterparty) |
| Total position | 10 MW | | Market |

## Key findings

**Market risk**
1. **Price behaviour:** [povzetek gibanja cen in vrha v letu 2022]
2. **Volatility:** [povzetek nestanovitnosti in razlik med obdobji]
3. **Value-at-Risk:** [ocenjen 95 % in 99 % VaR]
4. **Backtesting:** [število preseganj v primerjavi s pričakovanim]

**Liquidity risk**
5. **Margin requirements:** [največji kumulativni odliv gotovine]
6. **Liquidity-at-Risk:** [potrebna gotovina pri 95 % in 99 %]
7. **Stress period:** [primerjava 2021 in 2022]

**Credit risk**
8. **Exposure:** [najvišja izpostavljenost do nasprotne stranke]
9. **Potential future exposure:** [PFE pri 95 %]
10. **Expected loss:** [pričakovana izguba glede na bonitetno oceno]

![Day-ahead prices](figures/prices.png)

## Data

| | |
|---|---|
| **Source** | [ENTSO-E Transparency Platform](https://transparency.entsoe.eu/) |
| **Market** | Day-ahead prices, Slovenian bidding zone (BZN\|SI) |
| **Period** | 1 January 2021 – 31 December 2025 |
| **Resolution** | Hourly |

Default probabilities are based on publicly available rating agency default statistics. Only publicly available data is used.

## Methodology

### Data preparation
Raw data was checked for missing hours, daylight-saving time shifts (23- and 25-hour days) and outliers. Hourly prices were aggregated into daily base (all hours) and peak (weekdays 08:00–20:00) averages.

### Market risk
- Daily price changes and rolling volatility to identify calm and stressed periods
- Historical **Value-at-Risk** at 95% and 99% confidence levels
- **Backtesting** by comparing the number of VaR exceedances with the expected frequency

### Liquidity risk
- Daily **variation margin** cash flows for the exchange-traded component
- Maximum cumulative cash outflow over different horizons
- **Liquidity-at-Risk**: cash required at 95% and 99% confidence over one week and one month

### Credit risk
- **Current exposure**: positive mark-to-market value of the bilateral contract
- **Potential future exposure (PFE)** at 95% confidence, based on historical price changes
- **Expected loss**: EL = PD × LGD × EAD, with PD from rating agency statistics and an assumed LGD

## Assumptions and limitations

- Free historical futures prices for the Slovenian market are not publicly available; a rolling average of day-ahead prices is used as a proxy for the forward price.
- Historical methods assume that past price changes are representative of future risk, which may not hold during structural changes such as the 2022 crisis.
- The counterparty is hypothetical; probability of default and loss given default are assumptions, not estimates.
- Initial margin, volume risk, liquidity costs and portfolio diversification effects are not modelled.
- [dodatne omejitve, ugotovljene med analizo]

## Repository structure

```
energy-trading-risk/
├── data/               # raw and cleaned price data
├── notebooks/
│   ├── 01_data_preparation.ipynb
│   ├── 02_market_risk.ipynb
│   ├── 03_liquidity_risk.ipynb
│   ├── 04_credit_risk.ipynb
│   └── 05_summary.ipynb
├── figures/            # charts used in this README
├── requirements.txt    # Python dependencies
└── README.md
```

## How to reproduce

1. Clone the repository
2. Install dependencies: `pip install -r requirements.txt`
3. Run the notebooks in `notebooks/` in numerical order

## Tools

Python · pandas · NumPy · matplotlib · Jupyter · Git

## Use of AI

This project was developed with the help of an AI assistant (Claude, Anthropic), which I used to explain Python, statistical and risk management concepts, to review my code, and to suggest the project structure and documentation.

All analytical decisions, interpretation of results and final code are my own. I understand every step of the analysis and can explain it independently.

## Disclaimer

This is an independent project. It is not affiliated with, endorsed by, or based on any internal data or methods of GEN-I or any other company.

## Author

**[Ime Priimek]**
[e-pošta] · [LinkedIn](povezava)
