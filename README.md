# Quantifying observability through predicatibility
# 
![CoreQuestion](Figures/M1.jpg)

## Predictability and Observability in Multivariate Time Series

This project uses multivariate time-series forecasting to study a practical question:

> Do the available measurements contain enough information to predict the future behavior of a dynamical system?

We investigate this using two coupled oscillators, incomplete measurements, and a hidden disturbance. The project compares **self**, **conditional**, and **joint prediction** to determine what information each measurement provides.

---

---

## 3. SVD State Reconstruction

Long history windows can be high-dimensional and contain repeated information. We use singular value decomposition (SVD) to compress them.

The training history matrix is decomposed as

$$
H_{\mathrm{train}}=U\Sigma V^{\mathsf T}.
$$

Keeping the first \(r\) SVD modes gives a reduced state:

$$
\mathbf z_t=V_r^{\mathsf T}\mathbf h_t.
$$

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
