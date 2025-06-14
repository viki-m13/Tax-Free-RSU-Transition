# Rolling Harvest-and-Replace  
### An Illustrative Method for Transitioning From a Single-Stock Position Into a Diversified ETF **Without Realising Capital-Gain Tax**

---

## Options for Concentrated RSU Positions

Concentrated stock positions acquired via RSUs generally have only a few ways to reduce or eliminate tax liability:

1. **Immediate Vest-and-Sell**  
   Sell each RSU lot as soon as it vests at its cost basis.  
   - *Pros:* No taxable gain or loss; proceeds can be reinvested in SPY or held as cash.  
   - *Cons:* You forgo any upside from the stock—effectively turning your RSUs into a cash bonus.

2. **Index-Fund Private Placement**  
   Contribute shares “in kind” into a pooled index fund (often with a 5–7 year lock-up). Your cost basis carries into the fund, and taxes are deferred until you liquidate.  
   - *Pros:* Maintains upside exposure; defers tax.  
   - *Cons:* No immediate loss realization; basis remains embedded until exit.

3. **Tactical Tax-Harvesting & Direct Indexing** *(this strategy)*  
   - **Loss-harvest**: Sell RSU lots only when they trade below a threshold (e.g. 8 % under basis), realizing a deductible loss.  
   - **Gain-offset**: Simultaneously sell just enough other lots at a gain to fully absorb those losses (net tax ≈ 0).  
   - **Reinvest** all proceeds (loss and gain sales) into SPY, building diversification.  
   - *Pros:* Locks in permanent losses to offset gains; systematically builds a diversified position.  
   - *Cons:* Requires ongoing monitoring; must avoid wash-sale pitfalls.

4. **Advanced Direct-Indexing Manager**  
   Use proceeds from (3) to fund a direct-indexing account (e.g. Parametric) that continually tax-harvests a broad index—generating fresh losses to offset new gains and reinvesting proceeds into the diversified basket.  
   - *Pros:* Hands-off cycle; deeper harvest opportunities.  
   - *Cons:* Management fees; additional complexity.

---

## 1 What This Demonstration Shows

Many employees accumulate large blocks of one company’s stock through RSUs.  
This repository models a rules-based way to **gradually exchange those RSUs for the S&P 500 ETF (SPY) while only generating capital losses**—thereby avoiding any immediate capital-gain tax.

> **Important:** This code is purely illustrative. It is **not** a recommendation to implement this strategy in production.

---

## 2 Key Concepts in Everyday Language

| Term                         | Simplified Meaning                                                                        |
|------------------------------|-------------------------------------------------------------------------------------------|
| **RSU (Restricted Stock Unit)** | Shares that vest on a schedule.                                                             |
| **Cost basis**               | Share price on the vest date. Selling above → gain; below → loss.                         |
| **Capital-loss harvesting**  | Selling at a loss to record a deduction you can apply against other gains.                |
| **Wash-sale rule**           | Repurchasing the same stock within 30 days voids the loss—switching into SPY avoids that. |

---

## 3 How the Algorithm Works

1. **Track vesting**: Log each RSU vest as its own lot with basis = vest price.  
2. **Define transition start**: No sales occur before this date.  
3. **Loss-harvest**: After that date, if a lot trades ≥ X % below its basis → sell it and buy SPY.  
4. **Gain-offset**: Once you have realized losses, sell just enough other lots at a gain to offset those losses (net tax ≈ 0), and buy SPY with those proceeds too.  
5. **Repeat** until all lots are rotated into SPY.

---

## 4 How This Differs From Direct-Index or Custom-Index

- **Direct-index/custom-index**  
  - Swap concentrated shares “in kind” into a diversified basket.  
  - Your original low basis moves into the basket → tax deferred until you sell inside the basket.

- **Rolling Harvest-and-Replace**  
  - Only sells when a loss exists → realizes permanent losses that offset gains elsewhere.  
  - No deferred gains remain attached to your new SPY shares.

Both methods build diversification; they differ in how they treat taxes—**deferral** vs. **permanent loss realization**.

---

## 5 Default MSFT → SPY Back-Test

| Parameter            | Value                   | Explanation                      |
|----------------------|-------------------------|----------------------------------|
| Simulation window    | 2015-06-13 → 2025-06-13 | Ten years of history.            |
| Transition-start     | 2021-01-01              | When to begin harvesting.        |
| Harvest trigger      | 8 % below basis         | Sale threshold per lot.          |

**End-of-Period (2025-06-12)**

| Metric               | Rolling Harvest     | One-Day Bulk Sale   |
|----------------------|---------------------|---------------------|
| **Portfolio value**  | \$1,843,452.91      | \$1,025,372.18      |
| **Net tax impact**   | \$12,005.64 saved   | \$363,350.81 owed   |

---

## 6 Enhanced Model (Gain-Offset & Reinvestment)

Adds **gain-offset logic** and reinvests **all** proceeds (loss and gain sales) into SPY.  
Same parameters, just enable `enable_gain_offset = True` in code.

**End-of-Period (2025-06-12)**

| Metric               | Enhanced Model      |
|----------------------|---------------------|
| **Portfolio value**  | \$1,835,767.66      |
| **MSFT allocation**  | 72.4 %              |
| **SPY allocation**   | 27.6 %              |
| **Net tax impact**   | –\$98.04 (≈ 0)      |

---

## 7 When Someone *Might* Explore This

- Large RSU holders seeking **gradual risk reduction**.  
- Individuals prioritising **no immediate capital-gain tax** over a rapid exit.

---

## 8 Adjustable Parameters

```python
ticker_rsu             # RSU symbol (default "MSFT")
start_date, end_date   # back-test window
transition_start_date  # harvesting begins after this date
harvest_loss_pct       # loss threshold (e.g., 0.08)
benchmark_etf          # replacement asset (default "SPY")
enable_gain_offset     # toggle advanced gain-offset logic
```
---

## 9 Detailed Example Output

```text
RSU ticker symbol [MSFT]                      : 
Simulation start date  YYYY-MM-DD [2015-06-13]: 
Simulation end date    YYYY-MM-DD [2025-06-13]: 
Loss-harvest & conversion begin date [2021-01-01]: 

Downloading price history for MSFT and SPY …

======================= VESTING LOG =======================
(date, shares, vest_price)… see full log in `results/vest_log.csv`

================== VESTED SHARES BY YEAR ==================
(year, total_vested_shares, avg_vest_price)… see `results/vest_summary.csv`

==================== TRADE LOG ==========================
(date, shares_sold, basis, sale_price, loss, gain, tax_savings, tax_liability, etf_shares_bought)… see `results/trade_log.csv`

============= ANNUAL REALISED LOSS & GAIN ===============
(year, loss_realised, gain_realised)… see `results/annual_pl.csv`

============= ANNUAL TAX SAVINGS & LIABILITY ============
(year, tax_savings, tax_liability)… see `results/annual_tax.csv`

================= END-OF-PERIOD (as of 2025-06-12) ================
Enhanced Tax-Harvest with Gain-Offset & Reinvestment
  • Portfolio Value    : $1,835,767.66  
  • MSFT Allocation    : 72.4%  
  • SPY Allocation     : 27.6%  
  • Net Tax Impact     : –$98.04 (≈ 0)
```

---

## 10 Charts

![image](https://github.com/user-attachments/assets/cb396933-8a4c-441a-a874-a4fe2b2a3ff4)
![image](https://github.com/user-attachments/assets/bfbf3ec7-39c0-46ca-b2bc-0583360bff61)
![image](https://github.com/user-attachments/assets/f3ab1299-d461-4566-9f3d-535a9948296c)
![image](https://github.com/user-attachments/assets/d83b8b20-c474-4f21-a600-21b8a0a77317)
![image](https://github.com/user-attachments/assets/f93cee03-b9f2-40c1-ad51-362fa594f678)
![image](https://github.com/user-attachments/assets/c84d2cd3-9c67-428c-a585-012eccd1cc77)

---

## Professional & Legal Notice
This repository is educational. It does not encourage, solicit, or recommend execution of any strategy.
Tax law (e.g., wash-sale rules, loss carry-forwards) is complex and individual circumstances vary.
Consult qualified tax, legal, and investment professionals before making decisions.
