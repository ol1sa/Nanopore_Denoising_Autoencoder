**Nanopore Signal Denoising with Convolutional Autoencoders**

This project investigates convolutional autoencoders for reconstructing and denoising Oxford Nanopore raw current signals. Using windowed nanopore reads, I compare a standard autoencoder with a denoising autoencoder trained under structured signal corruption.

The goal is to evaluate the trade-off between reconstruction fidelity on clean signals and robustness under realistic signal dropout.

**Dataset**

- Raw Oxford Nanopore FAST5 files
- Signals extracted and converted to picoampere-scale current
- MAD normalisation applied per read
- Signals windowed into fixed-length segments (4096 samples)

**Methods**

**Baseline Autoencoder**
- 1D convolutional encoder–decoder
- Trained to reconstruct clean input signals
- Optimised with mean squared error (MSE)

**Denoising Autoencoder**
- Same architecture as baseline
- Trained on **masked inputs** with contiguous dropout
- Target remains the clean signal

**Evaluation Protocol**

Models are evaluated on a held-out test set under two conditions:

1. **Clean-input reconstruction**
2. **Masked-input reconstruction** (structured dropout)

This allows direct comparison of fidelity vs robustness.

| Condition | Baseline AE MSE | Denoising AE MSE |
|--------|-----------------|------------------|
| Clean input | **0.00111** | 0.00228 |
| Masked input | 0.08572 | **0.07958** |

The denoising autoencoder achieves a **~7% reduction in MSE under structured signal dropout**, indicating improved robustness to missing or corrupted signal segments. However, this comes at the cost of reduced reconstruction fidelity on clean inputs.

### Interpretation

Training with corrupted inputs introduces a bias that degrades performance on clean signals, but improves generalisation when the signal contains dropouts. This reflects a **robustness–fidelity trade-off**: the denoising model sacrifices peak accuracy in order to better handle structured noise and missing data, which are common in nanopore sequencing.


**Repository Structure**

- `notebooks/` – step-by-step experiments

**Running the Code**

```bash
pip install -r requirements.txt
