# A Window into World Models: The Next Generation of AI

*Can AI understand? LeJEPA vs PCA on four simulated chemical measurement systems —
companion code, data and notebooks for the article "A Window into World Models".*

Inspired by **"When Does LeJEPA Learn a World Model?"** — David Klindt, Yann LeCun &
Randall Balestriero, [arXiv:2605.26379](https://arxiv.org/abs/2605.26379), preprint, May 2026.

Today's models predict; LeCun argues the next step is machines that recover the hidden
structure of the world. This project makes that concrete on **simulated measurement models
inspired by real technologies**, where every relationship, hidden quantity and constant is
known in advance — so any method's reconstruction can be graded against a key.

## The notebooks

Two executed, self-contained Jupyter notebooks — open them on GitHub to read with all
figures, or run them top-to-bottom on Google Colab (standard runtime, no setup; the encoder
cell in notebook 02 takes a few minutes on CPU). They **create their own data** and **write
out every fitted relationship** — e.g. the density plane
(`density = 1.04 × CO₂ − 0.007 × haze + 1015`, R² = 0.95) — so no external file is needed.

- **`01_prediction_beer_tank.ipynb` — Prediction.** A fermenting lager tank, four
  instruments, 42 hours. Two fitted curves catch a **tank fault** (alarm at hour 11, 31
  hours left to act) and a **broken densitometer** (named by leave-one-out consistency).
  The honest limit: it predicts and alarms, but never learns that sugar exists.
- **`02_understanding_four_systems.ipynb` — Understanding.** Four simulated systems, each
  with hidden variables and ten sensors; the task is to recover the hidden state from the
  sensors alone.

| # | system | corner | PCA (worst) | LeJEPA (worst) |
|---|--------|--------|-------------|----------------|
| ① | Tablet press (NIR) | linear · Gaussian | 0.980 | 0.980 |
| ② | Two-sugar saccharimeter | **nonlinear · Gaussian** | **−0.09** | **0.97** |
| ③ | ICP-OES river metals | linear · non-Gaussian | 0.993 | 0.993 |
| ④ | Anaerobic digester | nonlinear · non-Gaussian | 0.917 | 0.712 |

Scores are linear-probe decodability (R², worst hidden variable, held-out data). For the
saccharimeter the notebook also verifies the stricter claim: recovery under an **orthogonal
Procrustes** map reaches R² = 0.98 with QᵀQ = I — recovery genuinely *up to a rotation*, as
the theorem states. The digester lies outside the theorem's Gaussian regime; our compact
encoder underperforms PCA there, consistent with the theory.

## Honest scope

These are simulations, not industrial data — that is what makes exact grading possible. The
sensor models are idealized. The encoder is a compact stand-in for the paper's full SIGReg.
The experiments address latent-**state** recovery, one component of a world model; a full
LeCun-style world model additionally needs action-conditioned dynamics.

## Citing

- D. Klindt, Y. LeCun, R. Balestriero. *When Does LeJEPA Learn a World Model?*
  arXiv:2605.26379 (2026).

## Author

Zahra Zabihinpour · zabihin@gmail.com

*An independent educational companion; all credit for the LeJEPA theory belongs to the
original authors. The explanatory idea and the chemistry framing are the author's; the
text, code, data and figures were built together with AI assistants (Anthropic's Claude;
a final editorial pass by OpenAI's ChatGPT) and verified against the seeded code in the
notebooks. Nothing here is new science.*
