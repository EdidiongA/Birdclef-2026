# BirdCLEF+ 2026 — Late-Entry Solution & Ensemble-Diversity Study

A solo, late-entry submission to [BirdCLEF+ 2026](https://www.kaggle.com/competitions/birdclef-2026)
(multi-taxon passive-acoustic species identification in the Brazilian Pantanal),
and the code behind an accepted CLEF 2026 working note.

> **Paper:** *Leveraging Public Resources as a Late Entrant in BirdCLEF+ 2026: An
> Empirical Study of Embedding-Space Ensemble Diversity and Its Limits.*
> CLEF 2026 Working Notes (LifeCLEF / BirdCLEF+), CEUR-WS Proceedings.
> _Link added on publication._

---

## TL;DR

- Entered **late (~2 weeks)**, solo, on **free-tier compute**.
- Leveraged public resources to move from a from-scratch baseline (public ROC-AUC
  **0.787**) to a competitive public-pipeline fork (**0.949**).
- Tested a **custom MLP head on Perch v2 embeddings** as an ensemble member and
  documented a **controlled negative result**: it did not improve the ensemble in
  any configuration.
- Final: **private ROC-AUC 0.94063, rank 1,313 / 4,092 (top ~32%)**, a 119-place
  climb from the final public leaderboard.

This repo holds my **own** contributions — the embedding extraction, the head
training, and the blend experiment. It does **not** redistribute competition data,
derived artifacts, or the public pipeline I forked (see notes below).

---

## What's mine vs. what I built on

**Mine (in this repo):** extracting Perch v2 embeddings over all 10,658 unlabelled
soundscapes; training a custom MLP head with a pseudo-labelling scheme; and the
ensemble ablation that produced the negative result.

**Not mine (credited, not re-hosted):** the competitive baseline is a fork of
Derek's public pipeline (Kaggle
[@sunderekkiz](https://www.kaggle.com/code/sunderekkiz/birdclef-2026-exp019-eos4-rank-power-06)),
which supplies the Perch + ProtoSSM + SED models and the rank-percentile blend. It
is linked, not copied. Full credit to all contributors is in the paper's
acknowledgements and in the header of notebook `03`.

---

## Key finding

The head looked like a good ensemble candidate on every standalone metric
(validation macro-AUC 0.852; median Spearman correlation of only 0.58 with the raw
Perch logits), yet every blend weight *reduced* the score, monotonically. The
interpretation: when multiple downstream models consume the **same** frozen
foundation-model representation, their outputs can decorrelate freely while their
access to label-relevant information is already bottlenecked upstream. Apparent
diversity lives in noise, not complementary signal. The productive path is
**representational orthogonality** (a different input view / backbone), not
architectural variety on one embedding.

---

## Repository structure

```
.
├── README.md
├── requirements.txt
├── .gitignore
├── LICENSE
└── notebooks/
    ├── 01_embedding_extraction.ipynb          # Perch v2 embeddings over all soundscapes (my work)
    ├── 02_head_training.ipynb                 # MLP head + pseudo-labelling (my work)
    └── 03_ensemble_blend_experiments.ipynb    # my head as an ensemble member + blend (my cells only)
```

Notebooks 01 and 02 are self-contained. Notebook 03 contains **only my additions**
to Derek's forked pipeline; to run the full blend, fork Derek's public notebook and
insert these cells (see the notebook's header).

## Data & artifacts (not included)

- **Competition data is not included.** BirdCLEF+ 2026 data is **CC BY-NC-SA** and
  may not be redistributed. Get it from the
  [official competition page](https://www.kaggle.com/competitions/birdclef-2026).
- **Derived artifacts are not included.** The embedding cache (~127,896 windows)
  and the trained head weights are derivatives of the competition data **and** are
  reserved for a planned follow-up study. The extraction/training methodology is
  documented in the notebooks and the paper in enough detail to reproduce them.

## Citation

```bibtex
@inproceedings{anwanane2026birdclef,
  author    = {Anwanane, Edidiong-Abasi},
  title     = {Leveraging Public Resources as a Late Entrant in {BirdCLEF+} 2026:
               An Empirical Study of Embedding-Space Ensemble Diversity and Its Limits},
  booktitle = {Working Notes of CLEF 2026 -- Conference and Labs of the Evaluation Forum},
  series    = {CEUR Workshop Proceedings},
  year      = {2026},
  publisher = {CEUR-WS.org},
  note      = {Volume and pages to be added upon publication}
}
```

## Acknowledgements

Builds on publicly shared Kaggle work — full credit to Derek (@sunderekkiz) and the
other contributors named in the paper, and to the Perch team for the foundation
model. This project extends, rather than replaces, their work.

## License

The **code** here is under the [MIT License](LICENSE). This does not cover the
competition data (CC BY-NC-SA, not included) or third-party resources the notebooks
reference.

---

*Edidiong-Abasi Anwanane · Kaggle: [didianwanane](https://www.kaggle.com/didianwanane) · Independent Researcher, Lagos, Nigeria*
