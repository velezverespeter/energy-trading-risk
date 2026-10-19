# Energy Trading Risk Analysis: Slovenian Power Market

**Market, liquidity and credit risk of an electricity trading position (2021–2025)**

Selection project prepared by **Peter Velez Vereš** as part of the application for the *Quantitative Developer* position at GEN-I, October 2026.

---

## Povzetek (SI)

Projekt analizira tri ključna tveganja trgovca z električno energijo: tržno, likvidnostno in kreditno. Na podlagi javno dostopnih urnih cen slovenskega dnevnega trga za obdobje 2021–2025 obravnava hipotetično pozicijo nakupa električne energije in ocenjuje, koliko lahko podjetje izgubi zaradi gibanja cen, koliko gotovine potrebuje za borzne varščine in kolikšna je izpostavljenost do nasprotne stranke.

Celotna analiza je dokumentirana v Jupyter zvezkih v mapi `notebooks/` in jo je mogoče ponoviti od surovih podatkov do končnih rezultatov.

---

## Executive summary

Energy trading companies are exposed to three closely linked risks. A single price movement simultaneously changes the value of a position (**market risk**), triggers cash margin payments to the exchange (**liquidity risk**), and changes how much a counterparty owes (**credit risk**). The 2022 energy crisis showed how quickly these risks can escalate together.

This project analyses one hypothetical trading position from all three perspectives, using public data from the Slovenian day-ahead market.

## Position analysed

A trader purchases **10 MW baseload** electricity for delivery in the following year at a fixed price:

| Component | Volume | Contract type | Main risk analysed |
|---|---|---|---|
| Exchange-traded | 5 MW | Futures | Liquidity (margining) |
| Bilateral | 5 MW | Forward | Credit (counterparty) |
| Total position | 10 MW | | Market |

## Analysis scope

**Market risk**
- How did Slovenian day-ahead prices behave between 2021 and 2025, and how extreme was the 2022 crisis?
- How does price volatility differ between calm and stressed periods?
- What is the historical Value-at-Risk of the position at 95% and 99% confidence?
- How reliable are these VaR estimates when tested against actual outcomes?

**Liquidity risk**
- How large are the daily variation margin payments on the exchange-traded component?
- What is the maximum cumulative cash outflow over one week and one month?
- How much cash would be required at 95% and 99% confidence (Liquidity-at-Risk)?

**Credit risk**
- How large is the exposure to the bilateral counterparty over time?
- What is the potential future exposure at 95% confidence?
- What is the expected loss for counterparties with different credit ratings?

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
Raw data is checked for missing hours, daylight-saving time shifts (23- and 25-hour days) and outliers. Hourly prices are aggregated into daily base (all hours) and peak (weekdays 08:00–20:00) averages.

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
- **Expected loss**: EL = PD × LGD × EAD, with PD from rating agency statistics and an assumed LGD of 60%

## Assumptions and limitations

- Free historical futures prices for the Slovenian market are not publicly available; a rolling average of day-ahead prices is used as a proxy for the forward price.
- Historical methods assume that past price changes are representative of future risk, which may not hold during structural changes such as the 2022 crisis.
- The counterparty is hypothetical; probability of default and loss given default are assumptions, not estimates.
- Initial margin, volume risk, liquidity costs and portfolio diversification effects are not modelled.

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
├── figures/            # charts used in the analysis
├── requirements.txt    # Python dependencies
└── README.md
```

## How to reproduce

```bash
git clone https://github.com/velezverespeter/energy-trading-risk.git
cd energy-trading-risk
pip install -r requirements.txt
```

Then run the notebooks in `notebooks/` in numerical order.

## Tools

Python · pandas · NumPy · matplotlib · Jupyter · Git · VS Code

## Use of AI

This project was developed with the help of AI tools:

- **GitHub Copilot** in VS Code for code suggestions and autocompletion
- **Claude (Anthropic)** for explanations of Python, statistical and risk management concepts, code review, and help with project structure and documentation

All analytical decisions, interpretation of results and final code are my own. I understand every step of the analysis and can explain it independently.

## Disclaimer

This is an independent project. It is not affiliated with, endorsed by, or based on any internal data or methods of GEN-I or any other company.

## Author

**Peter Velez Vereš**
[peter.velez.veres@gmail.com](mailto:peter.velez.veres@gmail.com) · [LinkedIn](https://www.linkedin.com/in/velezverespeter) · [GitHub](https://github.com/velezverespeter)
