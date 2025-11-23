## Project overview (cross-asset QIS workflow)

This repository contains three linked notebooks that form a coherent QIS workflow across rates and equity derivatives.  
The unifying idea is to **build arbitrage-free term-structure objects, reduce them to a few state variables, model their dynamics, and translate them into pricing / hedging / trading signals.**

---

### 1) PCA–HJM on U.S. Treasuries  

- **Term-structure construction.** Convert par yields into an instantaneous forward-rate surface. With bond prices $P(t,T)=\exp\left(-\int_t^T f(t,u)\,du\right)$ the instantaneous forward rate is $f(t,T)=-\partial_T \ln P(t,T)$.

- **Dimensionality reduction (PCA).** Apply PCA to changes in the forward curve to obtain data-driven factors: $\Delta f(t,T)\approx \sum_{i=1}^k \beta_i(t)\,\phi_i(T)$ where $\phi_i(T)$ are eigen-shapes (level/slope/curvature-like) and $\beta_i(t)$ are factor time series.

- **No-arbitrage dynamics (HJM).** Embed the PCA factors into an HJM model, $df(t,T)=\alpha(t,T)\,dt+\sum_{i=1}^k \sigma_i(t,T)\,dW^i_t$  
  with the HJM drift restriction enforcing arbitrage-free evolution.

The notebook produces a smooth forward surface, interpretable rates factors, and arbitrage-consistent curve simulations.

---

### 2) OU Entry–Exit Timing  

- **Reduced-form state dynamics.** Model a selected spread/factor as Ornstein–Uhlenbeck mean reversion: $dX_t=\kappa(\theta-X_t)\,dt+\eta\,dW_t$.

- **Systematic timing rules.** Use the OU stationary distribution and speed $\kappa$ to design transparent entry/exit thresholds.

The notebook provides a regime/timing layer indicating when the discounting state is unusually tight/loose.

---

### 3) SVI Volatility Surface on SPY  

- **Forward + discount inference (put–call parity).** For each expiry, infer forward $F(T)$ and discount $D(T)$ from $C(K,T)-P(K,T)=D(T)\,(F(T)-K)$.

- **Black–Scholes IV inversion.** Recover market implied vols $\sigma_{\text{imp}}(K,T)$ by solving $C_{\text{BS}}(F,K,T,D,\sigma_{\text{imp}})=C_{\text{mkt}}$.

- **Smile parametrization (SVI).** Fit total variance slices per expiry using Gatheral’s SVI form: $w(k,T)=a+b\Big(\rho(k-m)+\sqrt{(k-m)^2+\sigma^2}\Big),\qquad k=\ln(K/F)$.

- **Static no-arb diagnostics.** Check positivity of $w(k,T)$ and calendar monotonicity $\partial_T w(k,T)\ge 0$.

- **Desk features.** Extract ATM term structure, skew/convexity term structures, and a variance-swap fair-strike proxy from the surface.

Hence a smooth SPY implied-vol surface and term-structure risk-premia signals.

---

## How the notebooks link

The connection is **discounting regimes → crash-hedging demand → equity-vol skew**:

- The **PCA–HJM** notebook builds an arbitrage-free discount curve. Its factors drive the discount factor $D(T)=\exp\!\left(-\int_0^T r(u)\,du\right)$ which is an input to equity-derivative pricing and to the put–call-parity forwards used in the SVI notebook.

- The **OU Entry–Exit** notebook provides timing on when the discounting state is unusually stressed or loose. Such regimes often align with shifts in equity downside risk appetite.

- In the **SVI volatility-surface** notebook, these regimes show up as changes in the **left-tail of the smile** (more expensive OTM puts), i.e., the market price of crash insurance.

Therefore, the flow is:  
**(i)** construct an arbitrage-free term structure (rates curve or equity-vol surface),  
**(ii)** compress it into a few interpretable state variables (PCA factors / SVI parameters),  
**(iii)** model dynamics (HJM / OU), and  
**(iv)** connect regimes in discounting to pricing of equity tail risk.
