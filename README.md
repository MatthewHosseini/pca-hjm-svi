This draft repo describes both rates and equity-derivatives, with code and figures in the notebooks:

Rates term-structure construction: Convert daily U.S. Treasury par yields into a smooth instantaneous forward-rate surface $f(t,T)$.

Factor extraction: Run PCA on forward-rate changes to obtain data-driven level/slope/curvature factors.

No-arbitrage dynamics: Embed the PCA factors in a Heath–Jarrow–Morton (HJM) framework to simulate yield-curve evolution under no-arbitrage.

Signal design: Use mean-reverting (OU-style) dynamics of selected factors/spreads to derive systematic entry/exit timing rules.

Equity-volatility complement: Build an implied-volatility surface for SPY options using put–call-parity forwards, Black–Scholes IV inversion, and per-expiry SVI calibration; extract variance, skew, and convexity term structures plus a variance-swap fair-strike proxy.

Together, the notes show a desk-style approach, i.e. compress noisy term-structure data into a small set of state variables, model their dynamics with transparent stochastic tools, and translate them into pricing/hedging or systematic strategy signals across asset classes.
