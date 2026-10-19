# Questions and Answers

This document explains the key decisions and findings of the [Energy Trading Risk Analysis](README.md) in a question-and-answer format. All figures refer to a 10 MW baseload position in the Slovenian day-ahead market, 2021–2025.

---

## Data

**Why use Ember data instead of downloading directly from ENTSO-E?**
The ENTSO-E Transparency Platform interface allows exports of at most seven days at a time, which would require around 260 manual downloads for five years. Ember publishes the same ENTSO-E day-ahead prices as a cleaned hourly dataset under a CC BY 4.0 licence. To validate it, I compared Ember's prices for 1 January 2021 with a sample exported directly from ENTSO-E; the values matched exactly.

**Is the dataset complete?**
Yes. The period 2021–2025 contains exactly 43,824 hours (5 × 8,760 hours plus 24 hours for 29 February 2024), with no missing hours, duplicate timestamps or missing prices. The full Ember file covers 2015–2026 and contains missing values in May and June 2026, but these lie outside the analysed period.

**Why work in UTC rather than local time?**
UTC has no daylight-saving shifts, so every day has exactly 24 hours and calculations are not distorted by 23- or 25-hour days. Local time (Europe/Ljubljana) is used only where market definitions require it, such as peak hours.

**What data quality issue did the time conversion cause?**
Selecting data by UTC and grouping it into days by local time created a "day" on 1 January 2026 containing a single hour, and a first day with only 23 hours. The single-hour day was removed, as its average would have been the price of one hour only. The 23-hour first day was kept, since its effect is negligible, and is listed as a limitation.

**What about the switch to 15-minute prices in October 2025?**
The European day-ahead market moved from hourly to 15-minute products on 1 October 2025. Ember's dataset already aggregates these into hourly prices, so the series is consistent over the whole period.

**What is the difference between base and peak prices?**
Base is the average price over all 24 hours of a day. Peak is the average over weekdays from 08:00 to 20:00 local time, when demand is highest. Most forward contracts in European power markets are traded in these two forms.

---

## Market risk

**Why measure price changes in EUR/MWh instead of percentages?**
Electricity prices can be close to zero or negative. A move from 1 to 10 EUR/MWh would be a 900% return, which is meaningless for risk measurement. Absolute changes also translate directly into profit and loss in euros.

**What does the distribution of daily price changes show?**
Most daily changes are small and concentrated around zero, but there are extreme moves on both sides, from around −160 to +250 EUR/MWh. These "fat tails" are typical for electricity prices and would be practically impossible under a normal distribution. The right tail is longer, meaning sudden price spikes are larger than sudden drops.

**Why are there negative prices, and why does it matter?**
Prices turn negative when supply far exceeds demand, typically on sunny weekends at midday, and some generators (nuclear, subsidised renewables) prefer to pay rather than shut down. The lowest price, −500 EUR/MWh, occurred on Sunday 2 July 2023 at 14:00 and was then the market's minimum price. Negative hours grew from 23 in 2021 to 268 in 2025. For portfolios with solar generation, this means output is highest exactly when prices are lowest.

**What is the Value-at-Risk of the position?**
The historical 95% daily VaR is 10,881 EUR: on 95% of days, the loss on a 240 MWh daily position did not exceed this amount. The 99% VaR is 21,322 EUR.

**Why is a single VaR figure for the whole period misleading?**
Risk varies strongly over time. The 95% VaR was about 20,000 EUR in 2022 and about 7,800 EUR in calm years, a factor of 2.6. A single number overstates risk in calm periods and understates it during a crisis, exactly when it matters most.

**Why does the normal distribution give the wrong VaR?**
Because of fat tails. At 95%, normal VaR (13,115 EUR) is higher than historical VaR (10,881 EUR), because most days are calmer than a normal distribution assumes. At 99%, normal VaR (18,544 EUR) is lower than historical VaR (21,322 EUR), because the most extreme days are worse. A normal model therefore underestimates exactly the rare days that cause the largest losses.

**What was the worst day for the position?**
13 December 2024, with a loss of about 38,700 EUR. It followed a sharp price spike caused by a period of very low wind and solar output (*Dunkelflaute*), after which prices fell steeply.

**How was the VaR model tested?**
By backtesting: each day, VaR was calculated from the previous 250 days only and compared with the next day's actual result. A correct 95% VaR should be exceeded on about 5% of days.

**Why is the shift by one day important in backtesting?**
Without it, each day's VaR would include that day's own result, which would not be known in practice. This error, called look-ahead bias, makes models look better than they are.

**What did backtesting show?**
Overall, VaR was exceeded on 6.4% of days, close to the expected 5%, but the average hides large differences. In late 2021, as the market moved into crisis, exceedances reached 19.3%, because VaR was still based on calm months. In 2023, they fell to 3.0%, because the window still contained the 2022 crisis.

**What is the "ghost effect"?**
In a rolling historical window, an extreme day influences VaR for exactly 250 days and then drops out abruptly. VaR then jumps, not because the market changed, but because an old event left the window. This is visible as sudden steps in the backtesting chart.

**How could the model be improved?**
Models that give more weight to recent observations, such as EWMA or GARCH, react faster to changing volatility and avoid the ghost effect. Expected Shortfall, the average loss beyond VaR, would also better capture the fat tails.

---

## Liquidity risk

**What is variation margin?**
Exchange-traded futures are revalued every day at the current market price. When the price moves against the position, the company must pay the difference to the exchange in cash on the same day, even though the electricity has not yet been delivered.

**Why are the cash amounts so much larger than VaR?**
VaR measures the change in value of one day of delivery (240 MWh). Variation margin is settled on the entire one-year contract (43,800 MWh), which is 182 times more volume. Every 1 EUR/MWh price drop therefore requires a payment of 43,800 EUR.

**What is Liquidity-at-Risk?**
It applies the logic of VaR to cash: how much cash the company needs, with a given confidence, over a given period. For the 5 MW futures position, 30-day Liquidity-at-Risk is 3.78 million EUR at 95% and 8.90 million EUR at 99%.

**When was the cash outflow largest, and why?**
The largest 30-day outflow was 10.2 million EUR, in the period ending 16 October 2022. The position was long, so it paid margin when prices fell. Prices peaked in late August 2022 and then dropped sharply as gas storage filled and demand fell.

**How can a profitable company run out of cash?**
If the futures position hedges fixed-price sales to customers, the company is economically protected: losses on the futures are offset by higher margins on sales. But margin must be paid immediately, while customer payments arrive only at delivery. The company can be profitable and still face a cash shortfall.

**What would happen with a short position?**
The cash flows would be mirrored: a seller of forward electricity, such as a generator hedging future output, pays margin when prices rise. In summer 2022, several European energy companies faced exactly this problem and required emergency liquidity support.

---

## Credit risk

**What is credit exposure in a forward contract?**
If the counterparty defaults, the company must buy the same electricity again at the current market price. Exposure is the replacement cost: max(market price − contract price, 0) × remaining volume. If the market price is below the contract price, a default causes no loss, so exposure is zero.

**Why was exposure highest in December 2021 rather than August 2022?**
Exposure depends on both the price gap and the remaining volume. In August 2022, prices were higher, but more than half of the contract had already been delivered. On 23 December 2021, the full 43,800 MWh was still outstanding and the market price was about three times the contract price of 79.34 EUR/MWh, giving a peak exposure of 8.5 million EUR.

**What is potential future exposure (PFE)?**
Current exposure shows the loss if the counterparty defaulted today. PFE shows how high exposure could rise before the company can react. It was calculated using the 95th percentile of historical 30-day price increases and peaked at 12.5 million EUR.

**How was expected loss calculated?**
Expected loss = probability of default (PD) × loss given default (LGD) × exposure at default (EAD). EAD is the average exposure of 3.7 million EUR, LGD is assumed at 60%, and PD comes from approximate long-run default rates by credit rating.

**How much does counterparty quality matter?**
For the same contract, expected loss is about 3,300 EUR for a BBB-rated counterparty and about 66,700 EUR for a B-rated one, roughly 20 times more. Weaker counterparties therefore typically require collateral (bank guarantees or cash deposits), lower credit limits, or trading through an exchange, where counterparty risk is largely removed.

---

## How the risks connect

**Why analyse the three risks together?**
They are driven by the same price movements. In 2021–2022, rising prices increased market risk, created margin calls for sellers and large credit exposures for buyers, all at the same time. Looking at each risk in isolation would underestimate how quickly a company's position can deteriorate.

**What are the main limitations of the analysis?**
Forward prices are approximated by a 30-day average of day-ahead prices, historical methods assume the past represents the future, and the counterparty is hypothetical. Initial margin, volume risk and portfolio effects are not modelled. The full list is in the [README](README.md#assumptions-and-limitations).
