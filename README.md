# FIFA Ticket Resale: Static Bertrand Duopoly Model

This repository contains the code that supports our mathematical modelling project on
**high-priced FIFA World Cup ticket markets**. We study price competition between the
official FIFA resale platform and an unofficial secondary market in a **static Bertrand duopoly**
setting, and use data-driven calibration to estimate the demand system and to perform
comparative-statics analysis.

The code in this repo is not a separate ML project; it is designed to
**fit the linear demand coefficients, compute the Bertrand–Nash equilibrium, and generate
figures that appear in the paper**.

---

## 1. Economic Background

For high-priced FIFA World Cup tickets, demand often far exceeds the initial allocation.
After the official sales phase closes, fans who still want access to the matches rely on
resale channels:

- The **official resale platform** offers verified tickets with lower risk of counterfeits
  and the reputation of FIFA’s brand.
- The **unofficial secondary market** aggregates dispersed ticket sources, bearing
  higher risk but potentially offering lower prices.

This raises a central question in industrial organization:

> How does **brand reliability** compete with **low prices** when the product is
> essentially the same seat in the same stadium?

We model the interaction as a **static Bertrand duopoly** with two firms setting prices
for an identical product (a seat). The key trade-off is:

- FIFA’s brand advantage vs.  
- The secondary market’s low-price advantage.

Our goal is to estimate an **asymmetric linear demand system**, embed it into a static
Bertrand price competition model, compute the equilibrium, and study how high/low
pricing strategies shape revenues in the resale markets.

---

## 2. Model and Code Overview

### 2.1 Linear demand system

We specify linear demand for the official channel \(q_F\) and the secondary channel \(q_S\)
as functions of the official price \(p_F\) and secondary price \(p_S\):

\[
q_F = a_F - b_F p_F + d_F p_S, \qquad
q_S = a_S - b_S p_S + d_S p_F.
\]

The notebook estimates \((a_F, b_F, d_F, a_S, b_S, d_S)\) via **ordinary least squares**
using the normal equation

\[
\theta = (X^\top X)^{-1} X^\top y,
\]

where \(X\) collects prices and an intercept, and \(y\) is either \(q_F\) or \(q_S\).

### 2.2 Static Bertrand Nash equilibrium

Given the estimated demand system, firm \(F\) and firm \(S\) choose prices
to maximize their one-period profits:

\[
\pi_F = p_F q_F(p_F, p_S), \qquad
\pi_S = p_S q_S(p_F, p_S).
\]

The code:

1. Derives the **best-response functions** for each firm.
2. Solves the resulting linear system to obtain the **Bertrand–Nash equilibrium prices**
   \((p_F^\*, p_S^\*)\).
3. Computes equilibrium quantities, revenues, and market shares.

### 2.3 High–low pricing game and comparative statics

Around the equilibrium, we introduce a deviation parameter \(\delta \in [0, 0.5]\) and
define **high** and **low** prices for each firm:

\[
H_F = (1+\delta)p_F^\*, \quad L_F = (1-\delta)p_F^\*,\\
H_S = (1+\delta)p_S^\*, \quad L_S = (1-\delta)p_S^\*.
\]

For each \(\delta\), the notebook evaluates revenues under four pricing states:

1. \((H_F, H_S)\)
2. \((H_F, L_S)\)
3. \((L_F, H_S)\)
4. \((L_F, L_S)\)

and plots how the official and secondary revenues \((R_F, R_S)\) respond as \(\delta\)
changes. These plots help us answer questions such as:

- Can the unofficial market survive by relying on a persistent low-price strategy?
- Are there strategic traps similar to a **price war**, where both firms are worse off?
- How does brand advantage interact with aggressive discounting?

