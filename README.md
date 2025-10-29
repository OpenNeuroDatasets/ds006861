# Emotion Processing and Regulation Task (Static Stimuli) — tDCS‑EEG Dataset

This repository provides EEG recordings and behavioral data from the **Emotion Processing and Regulation** task conducted with **transcranial direct current stimulation (tDCS)**. 

* **Preregistration:** [https://osf.io/qdp3w](https://osf.io/qdp3w)
* **Preprint:** [https://osf.io/qtm8r](https://osf.io/qtm8r)

---

## Overview

Each participant took part in **two experimental sessions**:

* **`ses-1`** — Sham stimulation
* **`ses-2`** — Active stimulation

> **Exception.** Participant `sub-005` completed only `ses-1` due to a recording error during `ses-2`.

The order of sham/active conditions was **counterbalanced** across participants. 

---

## Participants

* **N = 120** right‑handed, neurologically healthy adults with normal or corrected‑to‑normal vision.

---

## Experimental Task

Participants completed 120 trials per session, split evenly across six conditions defined by stimulus content (social, non-social) and instruction (neutral-watch, negative-watch, negative-reappraise); on each trial, they viewed a static image and either passively watched it or reappraised it as instructed.

---

## tDCS Stimulation

**System:** Starstim 8 (Neuroelectrics, Spain) with **NIC2** software.

### Electrode Montage

Stimulation was targeted to the dorsolateral prefrontal cortex (dlPFC) in two alternative montages:

* **Right dlPFC stimulation**

  * *Anode:* **F4**
  * *Returns:* **FP2, FZ, FC2, FC6**

* **Left dlPFC stimulation**

  * *Anode:* **F3**
  * *Returns:* **FP1, FZ, FC1, FC5**

**Electrode areas**

* Anodal: **8 cm²**
* Return: **π cm²**
* **Ground:** left earlobe

### Stimulation Protocol

* **Active:** **2 mA** for **20 min** (with **30 s** ramp‑up)
* **Sham:** only ramp‑up periods at start and end; **no sustained current**
* **Questionnaires:** After each session, participants completed the **tDCS Sensation Questionnaire** (Polish version: [https://osf.io/ufszr](https://osf.io/ufszr)) to evaluate potential side effects. Additionally after the final session, they indicated whether they believed each session involved *real*, *sham*, or *I don’t know* stimulation to assess **blinding effectiveness**.

---

## EEG Acquisition

* **Cap:** 64‑channel QuickCap (32 **active** EEG electrodes used)
* **Amplifier:** Neuroscan **SynampsRT**
* **Sampling rate:** **1000 Hz**
* **Impedance:** kept **< 10 kΩ**

**Active EEG electrodes (32):**
FP1, FP2, F7, F3, FZ, F4, F8, FT7, FC3, FCZ, FC4, FT8,
T7, C3, CZ, C4, T8, M1, TP7, CP3, CPZ, CP4, TP8, M2,
P7, P3, PZ, P4, P8, O1, OZ, O2

**Additional sensors**

* **EOG:** Horizontal (HEO) and Vertical (VEO) channels were available on the cap but **were not connected** during recording.
* **Physio:** **ECG** and **GSR/EDA** were **recorded via auxiliary channels**.

---

## EEG Preprocessing

All preprocessing was performed in **MATLAB R2020b** using **EEGLAB 2023.0** and **ERPLAB 9.10**. The full, commented pipeline is provided in `code/Preprocessing_EEG.m`.

### Steps (summary)

1. **Band‑pass filter:** 0.1–30 Hz (zero‑phase Hamming‑windowed FIR)
2. **Downsample** to **250 Hz**
3. **Re‑reference** to average mastoids (M1, M2)
4. **Bad‑channel detection** using *clean_rawdata* (autocorrelation criterion = 0.8)
5. **ICA** with *runica*
6. **Automatic IC rejection** using **ADJUST** and **SASICA**
7. **Spherical interpolation** of removed channels
8. **Epoching:** −200 to 5000 ms relative to stimulus onset
9. **Baseline correction:** −200 ms pre‑stimulus
10. **Artifact rejection:** > ±100 µV within a 200 ms moving window (100 ms step)
11. **Condition‑wise averaging** using ERPLAB

---

## Derivatives & Ancillary Data

### `derivatives/processed_erps/`

Averaged ERP files (`.erp`) for each participant and session after preprocessing.

### `derivatives/side_effects_blinding_effectiveness/`

* `side_effects_blinding_effectiveness_english.csv` — _blinding effectiveness and side effects questionnaire
* `side_effects_blinding_effectiveness_data_dictionary.csv` — data dictionary with variable names and value coding

### `code/`

* MATLAB preprocessing script and documentation: `Preprocessing_EEG.m`

---

## Repository Structure

```
project_root/
├── sub-001/
│   ├── ses-1/
│   │   └── eeg/
│   │       ├── sub-001_ses-1_task-EmotionProcessingandRegulation_eeg.set
│   │       ├── sub-001_ses-1_task-EmotionProcessingandRegulation_eeg.fdt
│   │       └── sub-001_ses-1_task-EmotionProcessingandRegulation_eeg.json
│   └── ses-2/
│       └── eeg/
│           ├── sub-001_ses-2_task-EmotionProcessingandRegulation_eeg.set
│           ├── sub-001_ses-2_task-EmotionProcessingandRegulation_eeg.fdt
│           └── sub-001_ses-2_task-EmotionProcessingandRegulation_eeg.json
│
├── code/
│   └── Preprocessing_EEG.m
│
├── derivatives/
│   ├── processed_erps/
│   └── side_effects_blinding_effectiveness/
│       ├── side_effects_blinding_effectiveness_english.csv
│       └── side_effects_blinding_effectiveness_data_dictionary.csv
│
└── README.md
```


