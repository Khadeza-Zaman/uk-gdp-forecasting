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

## Model order and the COVID problem
- Initial ARIMA(1,0,1) on full sample: ar.L1 not significant (p=0.80), 
  ma.L1 significant (p=0.02), Jarque-Bera=19,597 (extreme non-normality)
- ARIMA(0,0,1) has lower AIC/BIC than (1,0,1) on the full sample
- Removing 2020Q2-2021Q2 (COVID) and refitting ARIMA(1,0,1): ar.L1 becomes 
  highly significant (p<0.001), ma.L1 becomes non-significant (p=0.81), 
  Jarque-Bera drops from 19,597 to 26.84
- Conclusion: the COVID shock was distorting which terms appeared 
  meaningful. The AR(1) term reflects genuine GDP momentum; the MA term 
  in the full-sample model was compensating for the shock rather than 
  capturing real dynamics
- Decision: keep COVID in the main evaluation (removing data would be 
  cherry-picking), but report both versions

## Static evaluation (27-quarter-ahead)
- Test period: 2019 Q3 onward, includes the COVID shock (2020 Q2-Q3)
- Naive: RMSE=5.270, MAE=2.115
- ARIMA(1,0,1): RMSE=5.263, MAE=2.129
- Essentially a tie — both forecasts stay flat and miss the COVID swing 
  entirely, so the RMSE gap between them is noise relative to the shock size

## Rolling one-quarter-ahead evaluation
- Rolling Naive: RMSE=8.630, MAE=3.504
- Rolling ARIMA: RMSE=9.492, MAE=3.695
- Unlike CPIH, rolling evaluation performed worse than the static test for 
  both models — the opposite pattern from the inflation project
- Mechanism: GDP's 2020 shock was V-shaped (sharp crash, sharp bounce-back), 
  not a gradual climb. Naive's forecast whips between copying the crash 
  forward (predicting the bounce-back quarter would also crash) and then 
  copying the bounce-back forward (predicting continued high growth as 
  reality normalized) - wrong in both directions at the turning point
- ARIMA's AR(1) term extrapolates momentum, which made it dip even further 
  than naive at the worst point (-26% vs naive's -20%) - the model's 
  attempt to capture real momentum backfired specifically at a sharp 
  reversal, where "the trend continues" is the worst possible assumption
- Conclusion: rolling short-horizon evaluation is not universally better 
  than static long-horizon evaluation - it depends on the shape of the 
  shock. Gradual shifts (CPIH 2022) favor rolling; V-shaped shocks (GDP 
  2020) can make rolling forecasts worse, since persistence-style models 
  are specifically bad at turning points