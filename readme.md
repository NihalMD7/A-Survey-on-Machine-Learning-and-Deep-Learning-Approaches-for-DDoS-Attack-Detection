# DDoS Attack Detection with Deep Learning
  
  Multi-class classification of network flows into 12 DDoS attack families plus
  benign traffic, trained on the CICDDoS2019 dataset and served behind a REST API.

  **Results:** 88.3% accuracy / 0.91 weighted F1 across 13 classes on 430k+ flows.

  ---

  ## Background

  I co-authored a literature survey on ML/DL approaches to DDoS detection
  (course project, Cleveland State University):
  [`docs/ddos-survey.pdf`](docs/ddos-survey.pdf).

  Its main finding motivated this repo: reported detection accuracy in the
  literature falls from **99%+ in binary settings** to **91% under nine-class
  evaluation** and **71% under adversarial (PGD) attack**. Benchmark numbers
  overstate real-world performance, so this project evaluates on the full
  multi-class problem rather than the easier benign-vs-attack split.

  ---

  ## Dataset

  [CICDDoS2019](https://www.unb.ca/cic/datasets/ddos-2019.html) — Canadian
  Institute for Cybersecurity.

  | | |
  |---|---|
  | Flows used | 430,000+ |
  | Features | 77 |
  | Classes | 12 attack families + benign |
  | Class skew | ~100x between majority and rarest class |

  The raw dataset is not included in this repo. Download it from the link above
  and place the CSVs in `data/raw/`.
  
  ---

  ## Approach

  **Preprocessing.** Dropped socket-identifier columns to prevent leakage,
  handled infinite/NaN values, and standardized features.

  **Class imbalance.** The rarest attack families were nearly undetectable with
  naive training (<5% recall). A hybrid **SMOTE + RandomUnderSampler** pipeline
  lifted minority-class recall to **>60%** without collapsing majority-class
  precision.

  **Models.** A feed-forward network and a 1D-CNN over the flow feature vector,
  tuned with `GridSearchCV` (+15% F1 stability across folds). Both were
  implemented in TensorFlow/Keras and PyTorch to cross-check framework parity.

  **Serving.** A FastAPI service exposing single and batch prediction, loading
  versioned `joblib`/JSON artifacts so the API and training code stay decoupled.

  ---

  ## Results

  | Model | Accuracy | Weighted F1 |
  |---|---|---|
  | Feed-Forward NN | 88.3% | 0.91 |
  | 1D-CNN | — | — |

  <!-- TODO: fill in the CNN row and add a per-class table if you have it -->

  ---

  ## Quickstart

  ```bash
  git clone https://github.com/<your-username>/ddos-detection.git
  cd ddos-detection
  pip install -r requirements.txt

  Train:
  python src/train.py --model ffn

  Serve:
  uvicorn src.api:app --reload

  The API is then available at http://localhost:8000, with interactive docs at
  /docs.

  <!-- TODO: verify these paths and commands match your actual file layout -->
  
  ---

  API

  POST /predict — classify a single flow.

  { "features": [0.0, 1.5, 42.0, "... 77 values" ] }

  { "label": "DrDoS_DNS", "confidence": 0.94 }

  POST /predict/batch — same, for a list of flows.

  ---

  Repository layout

  ├── data/raw/          # CICDDoS2019 CSVs (not tracked)
  ├── docs/
  │   └── ddos-survey.pdf
  ├── notebooks/         # EDA and model experiments
  ├── src/
  │   ├── preprocess.py
  │   ├── train.py
  │   └── api.py
  ├── models/            # versioned joblib / JSON artifacts
  └── requirements.txt

  ---
  
  Tech stack

  Python · TensorFlow/Keras · PyTorch · scikit-learn · imbalanced-learn · FastAPI

  ---

  Author

  Nihal Baba Mohammad — M.S. Computer Science, Cleveland State University
  LinkedIn (https://linkedin.com/in/nihalbabamohammad) ·
  GitHub (https://github.com/NihalMD7)

  Survey co-authored with Manognya Jupally.

