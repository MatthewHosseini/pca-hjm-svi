The unifying idea with the notebooks in this repo is to **build arbitrage-free term-structure objects, reduce them to a few state variables, model their dynamics, and translate them into pricing / hedging / trading signals.**

---

## Project overview

The spine is **discounting regimes → crash-hedging demand → equity-vol skew**, with an IRRBB-motivated interpretation:

- The **PCA–HJM** notebook builds an arbitrage-free discount curve. Its factors drive the discount factor
$D(T) = \exp\left(-\int_0^T r(u)\,du\right)$
  which is an input to:
  - banking-book EVE/NII revaluation; and  
  - equity-derivative pricing and the put–call-parity forwards used in the SVI notebook.

- The **OU Entry–Exit** notebook provides a low-dimensional, mean-reverting state that can be interpreted as an **IRRBB state variable** (e.g., duration gap or regime indicator). It delivers simple timing rules for when the IRRBB profile is “too stretched” and should trigger structural hedging or repositioning.

- In the **SVI volatility-surface** notebook, these discounting/IRRBB regimes show up as changes in the **left tail of the smile** (more expensive OTM puts), i.e., the market price of crash insurance and cross-asset hedging cost.

---

Therefore, the flow is:  
**(i)** construct an arbitrage-free term structure (rates curve or equity-vol surface),  
**(ii)** compress it into a few interpretable state variables (PCA factors / SVI parameters / OU state),  
**(iii)** model dynamics (HJM / OU), and  
**(iv)** connect regimes in discounting and IRRBB to the pricing of equity tail risk and cross-asset hedging costs.

Out of the box, the notebooks use U.S. Treasury curves and SPY index options, but the architecture is intended to be dropped in behind a bank’s IRRBB data stack (internal curves, behavioural models, and banking-book cash-flows) as a transparent QIS / R&D sandbox.
