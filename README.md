# IC50 Checker

**Live tool:** https://myagizzavrak.github.io/ic50-checker/

A free, browser-based IC50 calculator for cell viability and other dose-response assays. Paste your data, get a four-parameter logistic (4PL) fit and a plain-language quality report. Your data never leaves your computer.

## What it reports

- Relative IC50 (curve inflection point) with a 95 % confidence interval
- Absolute IC50 (concentration at 50 % viability)
- Hill slope, top and bottom plateaus, R²
- A publication-style plot (SVG or 300 DPI PNG) and a CSV of the results
- A methods sentence you can adapt for your paper

## Quality checks

| Check | Why it matters |
|---|---|
| Curve never drops below 50 % | No IC50 in the tested range: report "> highest concentration" |
| Bottom plateau above 20 % | Partial inhibition: relative and absolute IC50 differ |
| IC50 outside the tested range | Extrapolated value, needs a wider concentration series |
| 95 % CI wider than 10-fold | The data do not pin down the IC50 |
| Bottom plateau not defined by the data | The fit relies on an assumed plateau |
| Very steep Hill slope | Too few points in the transition zone |
| R² below 0.9 | Poor fit: check normalisation and outliers |
| Points more than 3 residual SD from the curve | Possible outlier wells (flagged, not removed) |

## Input format

```
conc   rep1   rep2   rep3
0.01   101.2  98.4   99.7
0.1    93.5   96.8   91.2
...
```

The first column holds the concentration and the other columns hold replicates. Values can be % viability, or raw signal that the tool normalises with vehicle and blank means. Tab, comma, semicolon and space separated data all work, so you can copy straight from Excel.

## Method

y = Bottom + (Top − Bottom) / (1 + 10^((log C − log IC50) × Hill))

The model is fitted by bounded Levenberg–Marquardt least squares on all replicate points. The confidence interval on log IC50 comes from the covariance matrix with the t-distribution. The fit was validated against SciPy `curve_fit` on complete, partial, non-reaching, extrapolated and steep test curves, and parameters agree to three decimals.

## Need more than one curve?

Multiple compounds and cell lines, statistics between treatments, publication-ready figures and an Excel results file are available as a service on Fiverr: https://www.fiverr.com/myagiz_zk

## License

MIT
