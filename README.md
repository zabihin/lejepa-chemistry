# A Window into World Models: The Next Generation of AI

*Can AI understand? LeJEPA vs PCA on four simulated chemical measurement systems —
companion code, data and notebooks for the article "A Window into World Models".*

Inspired by **"When Does LeJEPA Learn a World Model?"** — David Klindt, Yann LeCun &
Randall Balestriero, [arXiv:2605.26379](https://arxiv.org/abs/2605.26379), preprint, May 2026.

**The goal: a concrete, checkable use-case of AI *understanding*** — a world model
recovering the hidden state of a real system from its sensors (chemistry here; any
instrumented process in principle). Today's models predict; LeCun argues the next step is
machines that recover the hidden structure of the world. This project makes that concrete on **simulated measurement models
inspired by real technologies**, where every relationship, hidden quantity and constant is
known in advance — so any method's reconstruction can be graded against a key.

- **Read the article:** `index.html` (single file, figures embedded) — or the Medium version.
- **Medium source:** `medium/A_Window_into_World_Models.md` + `medium/figures/`.
- **Reproduce everything:** plain Python, seeded and deterministic.

## Part one — Prediction (`part1_prediction/`)

A fermenting lager tank, four instruments, 42 hours. Two fitted curves (a golden curve per
instrument and a cross-instrument reconstruction) catch two kinds of trouble hours before
the lab test: a **tank fault** (alarm at hour 11, 31 hours left to act) and a **broken
densitometer** (named by leave-one-out consistency, saving an in-spec batch). The honest
limit: it predicts and alarms, but never learns that sugar exists.

## Part two — Understanding (`part2_understanding/`)

Four simulated systems, each with hidden variables and ten sensors; the task is to recover
the hidden state from the sensors alone. Two data-measurable questions place each system on
the paper's grid: is the *sensor map* linear? are the *hidden variables* Gaussian?

| # | system | corner | PCA (worst) | LeJEPA (worst) |
|---|--------|--------|-------------|----------------|
| ① | Tablet press (NIR) | linear · Gaussian | 0.980 | 0.980 |
| ② | Two-sugar saccharimeter | **nonlinear · Gaussian** | **−0.09** | **0.97** |
| ③ | ICP-OES river metals | linear · non-Gaussian | 0.993 | 0.993 |
| ④ | Anaerobic digester | nonlinear · non-Gaussian | 0.917 | 0.712 |

Scores are linear-probe decodability (R², worst hidden variable, held-out data). For the
saccharimeter we additionally verified the stricter claim: recovery under an **orthogonal
Procrustes** map only reaches R² = 0.98 with QᵀQ = I — recovery genuinely *up to a
rotation*, as the theorem states. The digester lies outside the theorem's Gaussian regime;
our compact encoder underperforms PCA there, consistent with the theory.

## Run it as a notebook (recommended)

`notebooks/` contains two executed, self-contained Jupyter notebooks — open them on GitHub
to read with all figures, or run them top-to-bottom on Google Colab (standard runtime, no
setup; the encoder cell in notebook 02 takes a few minutes on CPU):

The notebooks are fully self-contained: they **create their own data** (the five good
tanks, the fault tanks, all four sensor worlds) and export it as CSVs while they run, and
they **write out every fitted relationship** — the density plane
(`density = 1.04 × CO₂ − 0.007 × haze + 1015`, R² = 0.95), all four quadratic
reconstruction formulas with tolerances, and the per-sensor linear laws of all four
systems. No external file is needed.

- `01_prediction_beer_tank.ipynb` — the fermentation, golden curves, Tank A / Tank B,
  the diagnosis audit table, and `extracted_relationships.txt`.
- `02_understanding_four_systems.ipynb` — the four systems, both checks, the trained
  encoder, the spiral, the Procrustes test, the transform-rescue study, and
  `sensor_linear_fits.csv`.

## Reproduce (scripts)

```bash
pip install -r requirements.txt

cd part2_understanding/code
python fourcells.py      # the four systems + linearity/Gaussianity measurements
python cell2_polar.py    # saccharimeter: PCA vs supervised vs trained LeJEPA
python test_rescue.py    # when a log / polynomial rescues PCA (and when nothing does)
python gen_p2_csv.py     # regenerate the per-system data CSVs into ../data

cd ../../part1_prediction/code
python beer2.py          # the fermentation simulator and checks
```

Numbers land in each part's `data/*.json`; figure scripts (`fig_*.py`, `figs_*.py`) rebuild
every plot. All runs are seeded; the saccharimeter result is stable across seeds.

## Research note (`note/`)

- `When_is_a_straight_line_enough.html` — the two separate axes (sensor linearity vs latent
  Gaussianity), the exact PCA criterion (R² = corr², the degree-1 Hermite share), and the
  three regimes of nonlinearity: monotone (a transform suffices), information-destroying
  folds (nothing works), invertible folds (only a learned encoder works).

## Structure

```
index.html                       the article (GitHub Pages homepage)
medium/                          Medium-ready Markdown + extracted figures
notebooks/                       executed educational notebooks (Colab-ready)
part1_prediction/                code/ · data/ · figures/ · workbook/ · Part1_beer.html
part2_understanding/             code/ · data/ · figures/ · Part2.html
note/                            the two-axes research note + its code, data, figures
```

## Honest scope

These are simulations, not industrial data — that is what makes exact grading possible. The
sensor models are idealized (real NIR/ICP-OES/polarimetry need chemometric calibration and
interference handling). The encoder is a compact stand-in for the paper's full SIGReg. The
experiments address latent-**state** recovery, one component of a world model; a full
LeCun-style world model additionally needs action-conditioned dynamics.

## Citing

- D. Klindt, Y. LeCun, R. Balestriero. *When Does LeJEPA Learn a World Model?*
  arXiv:2605.26379 (2026).

## Author

Zahra Zabihinpour · zabihin@gmail.com

*An independent educational companion; all credit for the LeJEPA theory belongs to the
original authors. The explanatory idea and the chemistry framing are the author's; the
text, code, data and figures were built together with AI assistants (Anthropic's Claude;
a final editorial pass by OpenAI's ChatGPT)
and verified against the seeded code in this repository. Nothing here is new science.*
