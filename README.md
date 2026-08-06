# observability-through-prediction
# Which Sensors Are Enough?

## Predictability and Observability in Multivariate Time Series

This project uses multivariate time-series forecasting to study a practical question:

> Do the available measurements contain enough information to predict the future behavior of a dynamical system?

We investigate this using two coupled oscillators, incomplete measurements, and a hidden disturbance. The project compares **self**, **conditional**, and **joint prediction** to determine what information each measurement provides.

---

## 1. Physical System

We simulate two coupled oscillators:

\[
\ddot{x}_1+\gamma_1\dot{x}_1+\omega_1^2x_1=u(t),
\]

\[
\ddot{x}_2+\gamma_2\dot{x}_2+\omega_2^2x_2=\kappa x_1.
\]

Here:

- \(x_1\) is the upstream oscillator.
- \(x_2\) is the downstream oscillator.
- \(\kappa\) controls the coupling between them.
- \(u(t)\) is a slowly changing external disturbance.

Only noisy measurements of the oscillator positions are initially available:

\[
y_t^{(1)}=x_1(t)+\epsilon_t^{(1)}, \qquad
y_t^{(2)}=x_2(t)+\epsilon_t^{(2)}.
\]

The disturbance \(u(t)\) is treated as an unmeasured variable. Later, it is added as a candidate sensor to test whether it improves prediction.

---

## 2. Main Idea

A single measurement may not reveal the complete state of the system. However, its recent history can contain information about hidden variables such as velocity.

For a set of measurements \(S\), we construct a history window:

\[
\mathbf h_t^{(S)}
=
[\mathbf y_t^{(S)},\mathbf y_{t-1}^{(S)},\ldots,
\mathbf y_{t-m+1}^{(S)}].
\]

This history acts as a reconstructed state of the system.

The central idea is:

> If the reconstructed state contains the dynamically relevant information, it should support accurate future prediction.

---

## 3. SVD State Reconstruction

Long history windows can be high-dimensional and contain repeated information. We use singular value decomposition (SVD) to compress them.

The training history matrix is decomposed as

\[
H_{\mathrm{train}}=U\Sigma V^{\mathsf T}.
\]

Keeping the first \(r\) SVD modes gives a reduced state:

\[
\mathbf z_t=V_r^{\mathsf T}\mathbf h_t.
\]

The forecasting pipeline is therefore:

```text
measurement histories
        ↓
history matrix
        ↓
SVD compression
        ↓
reconstructed state
        ↓
forecast model
        ↓
future prediction
