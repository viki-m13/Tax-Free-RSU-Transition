# Rolling Harvest-and-Replace  
### An Illustrative Method for Transitioning From a Single-Stock Position Into a Diversified ETF **Without Realizing Capital-Gain Tax**

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
   - **Reinvest** all proceeds (loss and gain sales) into a diversified vehicle.  
   - *Pros:* Locks in permanent losses to offset gains; systematically builds a diversified position.  
   - *Cons:* Requires ongoing monitoring; must avoid wash-sale pitfalls.

4. **Advanced Direct-Indexing Manager**  
   Use proceeds from (3) to fund a direct-indexing account (e.g. Parametric) that continually tax-harvests a broad index—generating fresh losses to offset new gains and reinvesting proceeds into the diversified basket.  
   - *Pros:* Hands-off cycle; deeper harvest opportunities.  
   - *Cons:* Management fees; additional complexity.

---

## 1 What This Demonstration Shows

Many employees accumulate large blocks of one company’s stock through RSUs.  
This repository models a rules-based way to **gradually exchange those RSUs for a diversified portfolio** while only generating capital losses—thereby avoiding any immediate capital-gain tax.

> **Important:** This code is purely illustrative. It is **not** a recommendation to implement this strategy in production.

---

## 2 Key Concepts in Everyday Language

| Term                         | Simplified Meaning                                                                        |
|------------------------------|-------------------------------------------------------------------------------------------|
| **RSU (Restricted Stock Unit)** | Shares that vest on a schedule.                                                             |
| **Cost basis**               | Share price on the vest date. Selling above → gain; below → loss.                         |
| **Capital-loss harvesting**  | Selling at a loss to record a deduction you can apply against other gains.                |
| **Wash-sale rule**           | Repurchasing the same stock within 30 days voids the loss—switching into a proxy avoids that. |

---

## 3 How the Algorithm Works

1. **Track vesting**: Log each RSU vest as its own lot with basis = vest price.  
2. **Define transition start**: No sales occur before this date.  
3. **Loss-harvest**: After that date, if a lot trades ≥ X % below its basis → sell it and reinvest proceeds.  
4. **Gain-offset**: Once losses are realized, sell just enough other lots at a gain to offset those losses (net tax ≈ 0) and reinvest those proceeds too.  
5. **Repeat** until all lots are rotated into a diversified portfolio.

---

## 4 How This Differs From Direct-Index or Custom-Index

- **Direct-index/custom-index**  
  - Swap concentrated shares “in kind” into a diversified basket.  
  - Your original low basis moves into the basket → tax deferred until you sell inside the basket.

- **Rolling Harvest-and-Replace**  
  - Only sells when a loss exists → realizes permanent losses that offset gains elsewhere.  
  - No deferred gains remain attached to your new diversified shares.

Both methods build diversification; they differ in how they treat taxes—**deferral** vs. **permanent loss realization**.

---

## 5 Model A: SPY-Only Harvest & Reinvest

**Parameters**  
- Simulation window: 2015-06-13 → 2025-06-13  
- Transition start: 2021-01-01  
- Harvest trigger: 8 % below basis  
- Replacement asset: SPY  

**End-of-Period (2025-06-12)**

| Metric                     | Value                   |
|----------------------------|-------------------------|
| **Portfolio value**        | \$1,835,767.66          |
| **RSU (MSFT) allocation**  | 72.4 %                  |
| **SPY allocation**         | 27.6 %                  |
| **Net tax impact**         | –\$98.04 (≈ 0)          |

---

## 6 Model B: Sector-ETF Harvest & Reinvest

**Parameters**  
- Same as Model A, but proceeds are split equally across 11 sector ETFs:  
  `XLB, XLE, XLF, XLI, XLK, XLP, XLRE, XLU, XLV, XLY, XLC`.

**End-of-Period (2025-06-12)**

| Metric                     | Value                   |
|----------------------------|-------------------------|
| **Portfolio value**        | \$1,193,874.12          |
| **RSU (MSFT) value**       | \$684,305.22            |
| **Sector-ETF value**       | \$509,568.90            |
| **Net tax impact**         | –\$13.52 (≈ 0)          |

---

## 7 When Someone *Might* Explore This

- Large RSU holders seeking **gradual risk reduction**.  
- Individuals prioritizing **no immediate capital-gain tax** over a rapid exit.

---

## 8 Adjustable Parameters

```python
ticker_rsu             # RSU symbol (default "MSFT")
start_date, end_date   # back-test window
transition_start_date  # harvesting begins after this date
harvest_loss_pct       # loss threshold (e.g., 0.08)
benchmark_etf          # replacement asset (default "SPY")
sector_etfs            # list of sector tickers for Model B
enable_gain_offset     # toggle advanced gain-offset logic
```

---

## 9 Detailed Example Output

### Model A: SPY-Only Harvest & Reinvest
```text
RSU ticker symbol [MSFT]                      : 
Simulation start date  YYYY-MM-DD [2015-06-13]: 
Simulation end date    YYYY-MM-DD [2025-06-13]: 
Loss-harvest & conversion begin date [2021-01-01]: 

Downloading price history for MSFT and SPY …

======================= VESTING LOG =======================
      date  shares  vest_price
2015-06-30     100       38.28
2015-09-30     100       38.63
2015-12-31     100       48.75
2016-03-31     100       48.88
2016-06-30     100       45.60
2016-09-30     100       51.65
2017-01-03     100       56.50
2017-03-31     100       59.82
2017-06-30     100       62.97
2017-10-02     100       68.52
2018-01-02     100       79.33
2018-04-02     100       82.08
2018-07-02     100       93.14
2018-10-01     100      108.08
2018-12-31     100       95.37
2019-04-01     100      112.23
2019-07-01     100      128.41
2019-09-30     100      132.02
2019-12-31     100      150.26
2020-03-31     100      150.68
2020-06-30     100      194.98
2020-09-30     100      202.00
2020-12-31     100      214.17
2021-03-31     100      227.55
2021-06-30     100      262.06
2021-09-30     100      273.24
2021-12-31     100      326.56
2022-03-31     100      299.98
2022-06-30     100      250.48
2022-09-30     100      227.62
2023-01-03     100      234.81
2023-03-31     100      283.27
2023-06-30     100      335.33
2023-10-02     100      317.54
2024-01-02     100      366.71
2024-04-01     100      420.58
2024-07-01     100      453.25
2024-09-30     100      427.80
2024-12-31     100      419.89
2025-03-31     100      374.70

================== VESTED SHARES BY YEAR ==================
      total_vested_shares  avg_vest_price
year                                     
2015                  300           41.89
2016                  300           48.71
2017                  400           61.95
2018                  500           91.60
2019                  400          130.73
2020                  400          190.46
2021                  400          272.35
2022                  300          259.36
2023                  400          292.74
2024                  500          417.64
2025                  100          374.70

==================== TRADE LOG ==========================
       date  shares_sold   basis  sale_price      loss     gain  tax_savings  tax_liability  etf_shares_bought
2022-01-13        100.0  326.5625     295.9569  -3060.56       0.0      1132.41            0.00              66.77
2022-01-13         12.0   38.2800     295.9569      0.00    3092.10         0.00         1144.08               8.01
2022-04-12        100.0  299.9841     274.4430  -2554.11       0.0       945.02            0.00              65.42
2022-04-12         11.0   38.2800     274.4430      0.00    2597.77         0.00          961.18               7.20
2022-05-12        100.0  273.2408     248.4543  -2478.65       0.0       917.10            0.00              66.16
2022-05-12         12.0   38.2800     248.4543      0.00    2522.07         0.00          933.16               7.94
2022-06-13        100.0  262.0585     236.2668  -2579.17       0.0       954.29            0.00              65.83
2022-06-13         13.0   38.2800     236.2668      0.00    2573.80         0.00          952.31               8.56
2022-09-30        100.0  250.4764     227.6205  -2285.59       0.0       845.67            0.00              66.02
2022-09-30         12.0   38.2800     227.6205      0.00    2272.06         0.00          840.66               7.92
2022-11-03        100.0  234.80896    209.3933  -1822.72       0.0       674.41            0.00              58.47
2022-11-03         11.0   38.2800     209.3933      0.00    1882.22         0.00          696.42               6.43
2023-09-26        100.0  335.3258     308.0114  -2731.44       0.0      1010.63            0.00              73.77
2023-09-26         10.0   38.2800     308.0114      0.00    2697.29         0.00          998.00               7.38
2024-04-30        100.0  420.5809     385.6721  -3490.89       0.0      1291.63            0.00              77.81
2024-04-30         10.0   38.2800     385.6721      0.00    3473.90         0.00         1285.34               7.78
2024-07-25        100.0  453.2549     415.2166  -3803.84       0.0      1407.42            0.00              77.85
2024-07-25         13.0   38.6300     415.2166      0.00    3992.73         0.00         1477.31               7.01
2025-02-27        100.0  427.7957     391.8107  -3598.50       0.0      1331.44            0.00              67.17
2025-02-27          1.0   38.6300     391.8107      0.00     353.18         0.00          130.68               0.67
2025-03-10        100.0  419.8857     379.4634  -4042.23       0.0      1495.63            0.00              67.89
2025-03-10         12.0   38.6300     379.4634      0.00    4089.99         0.00         1513.30               8.15

============= ANNUAL REALISED LOSS & GAIN ===============
      loss_realised  gain_realised
year                              
2022     -14780.80      14940.02
2023      -2731.44       2697.29
2024      -7294.73      10632.17
2025      -7640.73       4443.17

=========== ANNUAL TAX SAVINGS & LIABILITY ============
      tax_savings  tax_liability
year                            
2022      5468.89        5527.81
2023      1010.63         998.00
2024      2699.05        3933.90
2025      2827.07        1643.97

================= END-OF-PERIOD (as of 2025-06-12) ================
Enhanced Tax-Harvest with Gain-Offset & Reinvestment
  • Portfolio Value    : $1,835,767.66  
  • MSFT Allocation    : 72.4%  
  • SPY Allocation     : 27.6%  
  • Net Tax Impact     : –$98.04 (≈ 0)
```

### Model B: Sector-ETF Harvest & Reinvest
```text
RSU ticker symbol [MSFT]                      : 
Simulation start date  YYYY-MM-DD [2015-06-13]: 
Simulation end date    YYYY-MM-DD [2025-06-13]: 
Loss-harvest & conversion begin date [2021-01-01]: 

Downloading price history for ['MSFT', 'XLB', 'XLE', 'XLF', 'XLI', 'XLK', 'XLP', 'XLRE', 'XLU', 'XLV', 'XLY', 'XLC'] …

======================= VESTING LOG =======================
      date  shares  vest_price
2018-07-02     100       93.14
2018-10-01     100      108.08
2018-12-31     100       95.37
2019-04-01     100      112.23
2019-07-01     100      128.41
2019-09-30     100      132.02
2019-12-31     100      150.26
2020-03-31     100      150.68
2020-06-30     100      194.98
2020-09-30     100      202.00
2020-12-31     100      214.17
2021-03-31     100      227.55
2021-06-30     100      262.06
2021-09-30     100      273.24
2021-12-31     100      326.56
2022-03-31     100      299.98
2022-06-30     100      250.48
2022-09-30     100      227.62
2023-01-03     100      234.81
2023-03-31     100      283.27
2023-06-30     100      335.33
2023-10-02     100      317.54
2024-01-02     100      366.71
2024-04-01     100      420.58
2024-07-01     100      453.25
2024-09-30     100      427.80
2024-12-31     100      419.89
2025-03-31     100      374.70

================== VESTED SHARES BY YEAR ==================
      total_vested  avg_price
year                         
2018           300      98.86
2019           400     130.73
2020           400     190.46
2021           400     272.35
2022           300     259.36
2023           400     292.74
2024           500     417.64
2025           100     374.70

==================== TRADE LOG ==========================
       date ticker_sold  type  shares_sold      price         loss        gain  tax_savings  tax_liability
2022-01-13        MSFT  loss   100.000000 295.956909 -3060.562134    0.000000  1132.407990       0.000000
2022-01-13        MSFT  gain    16.000000 295.956909     0.000000 3245.055542     0.000000    1200.670551
2022-01-21         XLY  loss    16.261434 175.381775  -269.040933    0.000000    99.545145       0.000000
2022-01-21        MSFT  gain     1.000000 287.441437     0.000000  194.300499     0.000000      71.891185
2022-02-11         XLC  loss    46.399411  67.076859  -294.073441    0.000000   108.807173       0.000000
2022-02-11        MSFT  gain     1.000000 286.480194     0.000000  193.339256     0.000000      71.535525
2022-02-23         XLI  loss    37.144239  90.693314  -346.648490    0.000000   128.259941       0.000000
2022-02-23         XLK  loss    25.593307 143.327682  -353.401783    0.000000   130.758660       0.000000
2022-02-23        MSFT  gain     4.000000 272.701355     0.000000  718.241669     0.000000     265.749417
2022-03-01         XLF  loss   116.751107  34.982937  -369.974142    0.000000   136.890432       0.000000
2022-03-01        MSFT  gain     2.000000 286.984924     0.000000  387.687973     0.000000     143.444550
2022-03-08         XLB  loss    60.038098  74.637291  -396.668529    0.000000   146.767356       0.000000
2022-03-08        MSFT  gain     3.000000 268.400757     0.000000  525.779457     0.000000     194.538399
2022-04-12        MSFT  loss   100.000000 274.443024 -2554.110718    0.000000   945.020966       0.000000
2022-04-12        MSFT  gain    14.000000 274.443024     0.000000 2538.229202     0.000000     939.144805
2022-04-26         XLC  loss    74.446297  58.330860  -453.631678    0.000000   167.843721       0.000000
2022-04-26        MSFT  gain     2.000000 262.922791     0.000000  339.563705     0.000000     125.638571
2022-05-06         XLY  loss    32.657036 153.969833  -495.928325    0.000000   183.493480       0.000000
2022-05-06        MSFT  gain     3.000000 267.310944     0.000000  522.510017     0.000000     193.328706
2022-05-09        XLRE  loss   214.730787  38.498859  -908.248315    0.000000   336.051876       0.000000
2022-05-09        MSFT  gain     6.000000 257.434998     0.000000  985.764359     0.000000     364.732813
2022-05-12        MSFT  loss   100.000000 248.454346 -2478.649902    0.000000   917.100464       0.000000
2022-05-12        MSFT  gain    16.000000 248.454346     0.000000 2485.014526     0.000000     919.455375
2022-06-13        MSFT  loss   100.000000 236.266800 -2579.170227    0.000000   954.292984       0.000000
2022-06-13         XLK  loss    80.954238 122.168213  -923.371445    0.000000   341.647435       0.000000
2022-06-13        MSFT  gain    24.000000 236.266800     0.000000 3435.020691     0.000000    1270.957656
2022-06-16         XLB  loss   147.377386  70.485420  -983.916064    0.000000   364.048944       0.000000
2022-06-16         XLF  loss   399.405550  29.193937 -1079.490960    0.000000   399.411655       0.000000
2022-06-16        MSFT  gain     8.000000 238.909790     0.000000 1166.150818     0.000000     431.475803
2022-06-16        MSFT  gain     6.000000 238.909790     0.000000  861.250534     0.000000     318.662698
2022-07-14         XLE  loss   276.823691  61.217335 -1611.660741    0.000000   596.314474       0.000000
2022-07-14        MSFT  gain    11.000000 247.794418     0.000000 1676.690224     0.000000     620.375383
2022-09-21         XLC  loss   226.339005  48.985664 -1056.531422    0.000000   390.916626       0.000000
2022-09-21        MSFT  gain     7.000000 233.533371     0.000000  967.157356     0.000000     357.848222
2022-09-27        XLRE  loss   341.015168  32.958725 -1088.437070    0.000000   402.721716       0.000000
2022-09-27        MSFT  gain     8.000000 231.050949     0.000000 1085.463318     0.000000     401.621428
2022-09-30        MSFT  loss   100.000000 227.620499 -2285.588074    0.000000   845.667587       0.000000
2022-09-30        MSFT  gain    18.000000 227.620499     0.000000 2380.544357     0.000000     880.801412
2022-10-12         XLU  loss   396.750171  56.830700 -2586.961149    0.000000   957.175625       0.000000
2022-10-12        MSFT  gain    20.000000 220.632584     0.000000 2505.290985     0.000000     926.957664
2022-11-03        MSFT  loss   100.000000 209.393280 -1822.721863    0.000000   674.407089       0.000000
2022-11-03        MSFT  gain    16.000000 209.393280     0.000000 1824.403931     0.000000     675.029454
2022-12-22         XLY  loss   153.013894 126.048813 -1861.282889    0.000000   688.674669       0.000000
2022-12-22        MSFT  gain    14.000000 233.446640     0.000000 1933.100479     0.000000     715.247177
2023-09-26        MSFT  loss   100.000000 308.011414 -2731.442261    0.000000  1010.633636       0.000000
2023-09-26        MSFT  gain    14.000000 308.011414     0.000000 2798.986572     0.000000    1035.625032
2023-10-02         XLU  loss   164.951865  53.572048 -1064.884931    0.000000   394.007424       0.000000
2023-10-02        MSFT  gain     5.000000 317.543610     0.000000 1047.299042     0.000000     387.500645
2024-04-30        MSFT  loss   100.000000 385.672089 -3490.890503    0.000000  1291.629486       0.000000
2024-04-30        MSFT  gain    12.000000 385.672089     0.000000 3331.059448     0.000000    1232.491996
2024-07-25        MSFT  loss   100.000000 415.216583 -3803.836060    0.000000  1407.419342       0.000000
2024-07-25        MSFT  gain    13.000000 415.216583     0.000000 3992.726166     0.000000    1477.308681
2025-02-27        MSFT  loss   100.000000 391.810699 -3598.495483    0.000000  1331.443329       0.000000
2025-02-27        MSFT  gain    13.000000 391.810699     0.000000 3688.449677     0.000000    1364.726380
2025-03-10        MSFT  loss   100.000000 379.463379 -4042.230225    0.000000  1495.625183       0.000000
2025-03-10        MSFT  gain    14.000000 379.463379     0.000000 3799.314087     0.000000    1405.746212

============= ANNUAL LOSS & GAIN HARVEST ===============
      loss_realised  gain_realised
year                              
2022    -29860.07      29970.56
2023     -3796.33       3846.29
2024     -7294.73       7323.79
2025     -7640.73       7487.76

=========== ANNUAL TAX SAVINGS & LIABILITY ============
       tax_savings  tax_liability
year                            
2022   11048.23      11089.11
2023    1404.64       1423.13
2024    2699.05       2709.80
2025    2827.07       2770.47

================= END-OF-PERIOD (as of 2025-06-12) ================
Sector ETF Tax-Harvest & RSU Gain-Offset Strategy
  • Portfolio Value    : $1,193,874.12  
  • MSFT Value         : $684,305.22  
  • Sector-ETF Value   : $509,568.90  
  • Net Tax Impact     : –$13.52 (≈ 0)
```

---

## 10.1 Charts Model A: SPY-Only Harvest & Reinvest

![image](https://github.com/user-attachments/assets/cb396933-8a4c-441a-a874-a4fe2b2a3ff4)
![image](https://github.com/user-attachments/assets/bfbf3ec7-39c0-46ca-b2bc-0583360bff61)
![image](https://github.com/user-attachments/assets/f3ab1299-d461-4566-9f3d-535a9948296c)
![image](https://github.com/user-attachments/assets/d83b8b20-c474-4f21-a600-21b8a0a77317)
![image](https://github.com/user-attachments/assets/f93cee03-b9f2-40c1-ad51-362fa594f678)
![image](https://github.com/user-attachments/assets/c84d2cd3-9c67-428c-a585-012eccd1cc77)


## 10.2 Charts Model B: Sector-ETF Harvest & Reinvest
![image](https://github.com/user-attachments/assets/b98f3a5c-847c-43d2-859e-db2bb8eae7fa)
![image](https://github.com/user-attachments/assets/f8c4f6bd-ff93-4413-8243-a0f3730d38e3)
![image](https://github.com/user-attachments/assets/9f0b4eb0-d190-4fe3-9d71-194c152434f6)
![image](https://github.com/user-attachments/assets/2fb47d17-fe20-4d30-bd16-175a29d943a6)
![image](https://github.com/user-attachments/assets/f5a37dd2-aa74-48b1-84a8-a9c40fec61d5)
![image](https://github.com/user-attachments/assets/5f5356e2-361a-48d5-9549-f4930f9e57b0)


---

## Professional & Legal Notice
This repository is educational. It does not encourage, solicit, or recommend execution of any strategy.
Tax law (e.g., wash-sale rules, loss carry-forwards) is complex and individual circumstances vary.
Consult qualified tax, legal, and investment professionals before making decisions.
