# Robust LBP Face Recognition Experiments

Local Binary Pattern (LBP) face recognition code and the Colab notebook used to test how recognition changes under lighting and rotation. The notebook compares the original descriptor with rotation-compensated matching, illumination normalization, and their combination.

## Contents

- `notebooks/robust_lbp_experiments.ipynb`: executed Colab notebook with the experiment tables.
- `lbpface/`: LBP descriptor, distance measures, data loading, preprocessing, and evaluation code.
- `experiments/`: ORL, sweep, and FERET experiment entry points.
- `tests/`: unit and integration tests.
- `docs/original-colab-notes.txt`: the original notes that were embedded in the downloaded `requirements.txt`. The root `requirements.txt` contains installable dependencies.

## Results in the notebook

The notebook uses the ORL dataset with the first five images of each subject as gallery images and the other five as probes. Its recorded results include:

| Probe condition | Baseline | Final robust method |
| --- | ---: | ---: |
| Normal | 97.5% | 97.5% |
| Lighting ramp 0.8 | 70.5% | 96.5% |
| Rotation 20° | 63.0% | 88.0% |

These are results from the stored notebook run on one gallery/probe split. They are not a claim of general performance on other datasets.

## Run locally

Use Python 3.11 or a compatible version. From the repository root:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements-dev.txt
python -m pytest -q -k "not bit_exact_reproduction_of_the_stored_run"
```

The ORL dataset is not included. Place it at `data/ORL` with subject folders `s1` through `s40`, or set `ORL_DIR` to a copy of the dataset. The original notebook fetched a public ORL copy from [Face-Recognition-MLP](https://github.com/saeid436/Face-Recognition-MLP); check that source's terms before using or redistributing its images.

With ORL data in place, run the paper-style experiment:

```bash
python -m experiments.orl --data data/ORL --seed 0 --out results/orl_seed0.json
```

The notebook was downloaded from Colab after execution. Its first cells assume an uploaded `/content/LBP.zip` containing the `LBP` source folder, so upload that ZIP before rerunning it in Colab. Then follow the cells in order. The stored notebook outputs can be viewed without rerunning it.

The stored-run regression test expects `expected/orl_seed0.json`, which was not present in the downloaded source folder. It is excluded from the default test command above.
