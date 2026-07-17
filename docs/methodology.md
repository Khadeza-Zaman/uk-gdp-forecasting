# Methodology

## Data
- Source: ONS, GDP Quarter-on-Quarter growth, CVM SA (series IHYQ)
- ONS confirms this series is comparable back to 1955 (unlike CPIH's 
  pre-2005 data, which ONS itself flags as lower quality)
- Sample restricted to 1993 Q1 onward regardless, to stay within a single 
  monetary policy regime (post-ERM, inflation targeting) rather than mixing 
  in 1970s/80s dynamics from a very different era
- Note: Q2 2020 (COVID) is an extreme single-quarter outlier and will 
  likely dominate model fitting — flagged as a limitation, not removed

## Stationarity
- ADF test on levels: p = 9.87e-20 → stationary, no differencing needed
- Contrasts with CPIH, which required one round of differencing (d=1)
- Makes sense: GDP QoQ growth is already a rate of change, so it doesn't 
  drift the way inflation levels do — it oscillates around a stable mean
- Confirms d = 0 for ARIMA