# Rolling Harvest-and-Replace  
### An Illustrative Method for Transitioning From a Single-Stock RSU Position Into a Diversified ETF **Without Realising Capital-Gain Tax**

---

## 1 | What This Demonstration Shows  
Many employees accumulate large blocks of one company’s stock through RSUs.  
This example models a rules-based way to **gradually exchange those RSUs for the S&P 500 ETF (SPY) while only crystallising capital losses**—thereby avoiding any immediate capital-gain tax.  

> **Important:** This repository is purely an illustration of how such a rule set might work in Python.  
> It is **not** a recommendation to implement the approach.

---

## 2 | Key Concepts in Everyday Language  

| Term | Simplified Meaning |
|------|--------------------|
| **RSU (Restricted-Stock Unit)** | Shares that become yours on a schedule (they “vest”). |
| **Cost basis** | The share price on the vest date.  Selling above that price triggers a gain; below it triggers a loss. |
| **Capital-loss harvesting** | Intentionally selling at a loss so the IRS records a deduction you can use to offset current or future gains. |
| **Wash-sale rule** | Buying back the identical stock within 30 days voids the loss; switching into SPY avoids that issue. |

---

## 3 | How the Algorithm Works

1. **Log each vest** as a separate lot with its own basis.  
2. **Set a transition-start date**—before this date nothing is sold.  
3. **After that date, monitor every lot.**  
   * If the current price falls more than *X %* (default 8 %) below its basis → **sell the lot**.  
4. **Immediately reinvest** all proceeds into SPY.  
5. **Repeat** until every lot has rotated; each sale realises a loss, not a gain, so no tax is due.

---

## 4 | How This Differs From Direct-Index or Custom-Index Solutions  

* **Direct-index or custom-index accounts** let you swap a concentrated stock for a diversified basket, typically by contributing shares “in kind.”  
  * Your original low cost basis *moves* into that basket; capital-gain tax is therefore **deferred** until you later sell positions inside the basket.  

* **Rolling Harvest-and-Replace** systematically sells only when a loss exists.  
  * Capital-losses are **realised**, permanently offsetting gains elsewhere; no deferred gain remains attached to the new SPY shares.  

Both paths diversify a portfolio; they simply treat taxes differently—**deferral versus permanent loss realisation**.

---

## 5 | Default Microsoft → SPY Back-Test  

| Parameter | Value | Explanation |
|-----------|-------|-------------|
| Simulation window | **2015-06-13 → 2025-06-13** | Ten years of historical data. |
| Transition-start | **2021-01-01** | Date chosen to begin diversification. |
| Harvest trigger | 8 % below basis | Threshold for selling a given lot. |

| Metric (as of 2025-06-12) | Rolling Harvest | One-Day Bulk Sale |
|---------------------------|-----------------|-------------------|
| Portfolio value | **\$1.84 M** | \$1.03 M |
| Net tax impact | **\$ 12 K tax saved** | \$ 363 K tax owed |

---

## 6 | When Someone *Might* Explore This (Hypothetically)

* Large RSU holders seeking gradual risk reduction.  
* Individuals prioritising **no immediate capital-gain tax** over rapid exit.  

---

## 7 | Adjustable Parameters in the Code

ticker_rsu             # stock symbol (default "MSFT")
start_date / end_date  # back-test window
transition_start_date  # harvesting begins after this date
harvest_loss_pct       # loss threshold (e.g., 0.08)
benchmark_etf          # diversified replacement asset (default "SPY")


## End-of-Period (as of 2025-06-12)

### Tax-Loss-Harvest Strategy
- **Portfolio Value:** $1,843,452.91  
- **MSFT Allocation:** 75.3%  
- **SPY Allocation:** 24.7%  
- **Net Tax Impact:** $12,005.64 saved  

### All-at-Once Bulk Sale
- **Gross Proceeds:** $1,388,722.99  
- **Net Tax Impact:** $363,350.81 owed  
- **Net Proceeds:** $1,025,372.18  


![image](https://github.com/user-attachments/assets/2087ee09-843b-4ce8-9209-0af9ed148507)

![image](https://github.com/user-attachments/assets/ee417501-b1ff-46de-882b-19513a217850)

![image](https://github.com/user-attachments/assets/c1fbb49c-b617-47f0-93b5-fcd829ff172e)

![image](https://github.com/user-attachments/assets/0f25cb7f-8af4-4774-a3a8-f5b9c6070577)

![image](https://github.com/user-attachments/assets/d7ea234b-cf73-4df0-9f53-65c75286e45c)

![image](https://github.com/user-attachments/assets/420c4b51-c434-415e-b66b-69476d9af116)










Professional & Legal Notice
This repository is educational. It does not encourage, solicit, or recommend the execution of any strategy.
Tax law (e.g., wash-sale rules, loss carry-forwards) is complex, and individual circumstances vary.
Consult qualified tax, legal, and investment professionals before making decisions.
