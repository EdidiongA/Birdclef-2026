# BirdCLEF+ 2026: Late-Entry Solution and Ensemble-Diversity Study

[![Paper](https://img.shields.io/badge/CLEF%202026-Working%20Note%20accepted-1f6feb)](paper/Anwanane_BirdCLEF2026_WorkingNote.pdf)
[![Kaggle](https://img.shields.io/badge/Kaggle-top%2032%25%20of%204%2C092%20teams-20BEFF)](https://www.kaggle.com/competitions/birdclef-2026)
[![Code license](https://img.shields.io/badge/code-MIT-green)](LICENSE)
[![Paper license](https://img.shields.io/badge/paper-CC%20BY%204.0-lightgrey)](https://creativecommons.org/licenses/by/4.0/)

A solo, late-entry submission to [BirdCLEF+ 2026](https://www.kaggle.com/competitions/birdclef-2026),
the multi-taxon passive-acoustic species-identification challenge set in the Brazilian
Pantanal, together with the code behind an accepted CLEF 2026 working note.

> **Paper.** *Leveraging Public Resources as a Late Entrant in BirdCLEF+ 2026: An
> Empirical Study of Embedding-Space Ensemble Diversity and Its Limits.*
> CLEF 2026 Working Notes (LifeCLEF / BirdCLEF+), CEUR Workshop Proceedings, pp. 4388–4402.
>
> **[Read the paper (PDF)](paper/Anwanane_BirdCLEF2026_WorkingNote.pdf)** ·
> Official CEUR-WS link to follow once the volume number is assigned.

## Summary

Entered with roughly two weeks left, working alone on free-tier compute. Forking a
publicly shared pipeline closed most of the gap to the leaders at negligible cost;
the paper's contribution is a controlled test of the most natural next step, a custom
classifier head trained on the same foundation-model embeddings, and a documented
negative result.

| Stage | Public ROC-AUC |
|---|---|
| From-scratch EfficientNet-B0 baseline | 0.787 |
| Fork of the public Perch v2 + ProtoSSM + SED pipeline (exp019) | 0.949 |
| Same pipeline plus a custom MLP head (best of three blends) | 0.948 |
| **Final private leaderboard** | **0.94063, rank 1,313 of 4,092 (top 32.1%)** |

The final standing includes a 119-place climb from the public leaderboard after the
private-test shake-up.

## Key finding

Adding a well-trained MLP head to an already strong ensemble did not improve the
public score, even though the head reached competitive AUC on its own. The paper
argues this is a redundancy effect: every member of the ensemble, including the new
head, was reading the same frozen Perch v2 embedding space, so their errors were
correlated in exactly the places where the ensemble already failed. Diversity in
architecture is not the same as diversity in information, and blending cannot recover
signal that no member has access to. The result is small and specific, but it is
measured carefully and it points at a question worth studying properly, which is the
subject of ongoing follow-up work.

## What is mine and what I built on

**Mine.** The from-scratch EfficientNet-B0 baseline; the Perch v2 embedding
extraction over all soundscapes; the MLP head, its training, and its
pseudo-labelling loop; the blend experiments that test the head as an ensemble
member; the analysis and the paper.

**Built on (credited, not re-hosted).** The public Perch v2 + ProtoSSM + SED
inference pipeline shared on Kaggle by Derek ([@sunderekkiz](https://www.kaggle.com/sunderekkiz)),
which I forked as exp019 and used unchanged as the base ensemble. That code is not
reproduced here; the third notebook contains only my own cells, with a header pointing
to the original. Foundation-model embeddings come from Google's
[Perch](https://github.com/google-research/perch) family.

## Repository structure

```
.
├── paper/
│   └── Anwanane_BirdCLEF2026_WorkingNote.pdf   # accepted working note (CC BY 4.0)
├── notebooks/
│   ├── 01_embedding_extraction.ipynb           # Perch v2 embeddings over all soundscapes
│   ├── 02_head_training.ipynb                  # MLP head and pseudo-labelling
│   └── 03_ensemble_blend_experiments.ipynb     # the head as an ensemble member (my cells only)
├── requirements.txt
├── LICENSE
└── README.md
```

## Getting started

```bash
pip install -r requirements.txt
```

The notebooks were written for the Kaggle environment and expect the competition data
at `/kaggle/input/competitions/birdclef-2026`. Open them in order: `01` produces the
embedding cache, `02` trains the head on top of it, and `03` blends the head with the
base ensemble. Paths at the top of each notebook can be edited for a local run.

## Data and artefacts

The competition data is released under CC BY-NC-SA and is **not** included in this
repository; obtain it from the [competition page](https://www.kaggle.com/competitions/birdclef-2026).
Derived artefacts (the embedding cache and trained head weights) are likewise not
published, as they are being reused in follow-up work.

## Citation

```bibtex
@inproceedings{anwanane2026birdclef,
  author    = {Anwanane, Edidiong-Abasi},
  title     = {Leveraging Public Resources as a Late Entrant in {BirdCLEF+} 2026:
               An Empirical Study of Embedding-Space Ensemble Diversity and Its Limits},
  booktitle = {Working Notes of CLEF 2026 -- Conference and Labs of the Evaluation Forum},
  series    = {CEUR Workshop Proceedings},
  publisher = {CEUR-WS.org},
  year      = {2026},
  pages     = {4388--4402},
  note      = {Volume number to be added on publication}
}
```

## Acknowledgements

Derek ([@sunderekkiz](https://www.kaggle.com/sunderekkiz)) for openly sharing the
pipeline that this work builds on, and the other Kaggle participants whose public
notebooks and discussion threads made a late entry viable. The BirdCLEF+ 2026
organisers and annotators for the dataset, and the Perch team at Google for releasing
the embedding models.

## Licence

- **Code** in this repository: [MIT](LICENSE).
- **Paper** in `paper/`: © 2026 the author, [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
- **Competition data and third-party resources** are not covered by either licence and are not included.

---

Edidiong-Abasi Anwanane · Independent Researcher, Lagos, Nigeria ·
Kaggle [didianwanane](https://www.kaggle.com/didianwanane) · GitHub [EdidiongA](https://github.com/EdidiongA)
