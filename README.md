# Scalp seizure annotation: inter-rater agreement for seizure onset and contralateral spread

This repository contains de-identified expert annotations of scalp EEG seizures and the code used to
produce the main and supplementary figures on inter-rater agreement for seizure onset laterality,
onset channels, contralateral spread, and onset/spread timing.

- 79 seizures from 48 patients (identified only by a study ID, `PennEPI`)
- 5 annotators; each seizure was annotated by 3 of the 5
- For each seizure, each annotator marked the onset time and onset channels, and the time and
  channels of contralateral spread

## Repository contents

```
dataset/
  Annotation_PennEPIID.csv     Seizure-level annotations from all annotators (one row per seizure)
  Participants_PennEPIID.csv   Seizure-level clinical information (one row per seizure)
code/
  main_figure.ipynb            Code for Figures 1B–4B
  supplement_figure.ipynb      Code for Supplementary Figures 1 and 4–8
requirements.txt               Python package versions used for the analyses
```

EEG signals are not included in this repository. `edf_file_name` identifies the de-identified EDF
recording each annotation refers to.

## Data dictionary

### `dataset/Annotation_PennEPIID.csv`

| Column | Description |
|---|---|
| `PennEPI` | Study patient ID |
| `seizure_number` | Study seizure ID (unique across the dataset) |
| `edf_file_name` | De-identified EDF file for the seizure (`PennEPI{N}_seizure{M}.edf`) |
| `Your_onset_time (s)_i` | Seizure onset time marked by annotator *i*, in seconds from the start of the EDF file; `no seizure` if the annotator judged the event not to be a seizure |
| `Your_onset_channel_i` | Bipolar channel(s) at onset marked by annotator *i* (`DIFFUSE` is stored as the expanded list of channels) |
| `Your_contralateral_spread_time (s)_i` | Time of contralateral spread marked by annotator *i*, in seconds from the start of the EDF file; empty if no contralateral spread |
| `Your_contralateral_spread_channel_i` | Bipolar channel(s) at contralateral spread marked by annotator *i* |
| `seizure_off` | Seizure offset, in seconds from the start of the EDF file |
| `recording_off` | End of the EDF recording (60 s after `seizure_off`, or earlier if the source recording ended sooner), in seconds from the start of the file |
| `Group_i`, `Majority_Group`, `Agreed_Annotators` | Seizure type per annotator, majority type, and the annotators in the majority, as exported. The notebooks recompute these from the onset/spread columns above. |

Annotator columns are empty (`NaN`) for the 2 annotators who were not assigned a given seizure.

### `dataset/Participants_PennEPIID.csv`

| Column | Description |
|---|---|
| `PennEPI`, `seizure_number` | Study patient and seizure IDs (match `Annotation_PennEPIID.csv`) |
| `gender`, `age` | Patient sex and age |
| `scalp_localization` | Scalp EEG seizure localization (temporal, frontal, multifocal; empty if unknown) |
| `Surgery received` | Surgical intervention (resection, laser ablation, RNS, VNS, DBS, or no intervention received)* |
| `Engel` | 2-year Engel classification, where available* |
| `surgical_outcome` | `Good` (Engel class I) or `Poor/Device/No Surgery` (Engel class II–IV, device implantation, or no intervention), using the 2-year Engel classification or the 1-year classification when the 2-year assessment is unavailable* |

\* Surgery and outcome columns (`Surgery received`, `Engel`, `surgical_outcome`) are provided only for the 47 seizures (30 patients)
whose scalp EEG admission started before the surgical intervention; they are empty for all other
seizures.

## Figures

**`code/main_figure.ipynb`**

| Figure | Content |
|---|---|
| 1B, 1C | Gwet's AC1 for onset laterality and contralateral spread (inter-rater and agreement with the majority-vote consensus) |
| 2A | Channel-level agreement (Cohen's κ) for onset and spread, seizure level |
| 2B | Channel-level agreement with consensus, by annotator |
| 3A | Onset time error (median absolute deviation, MAD) |
| 3B | Spread time error (MAD) |
| 3C | Onset MAD vs. spread MAD (paired Wilcoxon signed-rank test) |
| 4A, 4B | Gwet's AC1 for temporal vs. non-temporal seizures |

**`code/supplement_figure.ipynb`**

| Figure | Content |
|---|---|
| S1 | Gwet's AC1, patient level |
| S4A, S4B | Pairwise annotator agreement tables and 5 × 5 percentage agreement matrices (onset laterality; contralateral spread) |
| S5 | Channel-level agreement (Cohen's κ), patient level |
| S6 | Spread time MAD for 3/3 vs. 2/3 agreement |
| S7 | Gwet's AC1 for temporal vs. non-temporal seizures, patient level |
| S8A, S8B | Spread speed [1/(1 + t)] by surgical outcome in the 47 seizures recorded before surgery (patient level; seizure level) |

## How to run

```bash
pip install -r requirements.txt
pip install jupyter
jupyter notebook
```

Open a notebook in `code/` and run all cells from top to bottom. The notebooks read the CSV files
from `../dataset/`, so they must be run with `code/` as the working directory (the Jupyter default
when a notebook is opened from that folder). Each notebook is self-contained; the first cell
(`SETUP`) must be run first.

The analyses were run with Python 3.8.20 and the package versions in `requirements.txt`.
