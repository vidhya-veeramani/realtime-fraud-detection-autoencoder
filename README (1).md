# 🍬 Real-Time Fraud Detection Dashboard

A real-time anomaly detection system that flags suspicious e-commerce transactions **as they happen**, using an unsupervised autoencoder — with a live dashboard that translates model output into estimated business impact ($ saved), not just abstract metrics.

![Dashboard Demo](images/demo.gif)
*(Add a screen recording of your dashboard here — see "How to Add a Demo" below)*

---

## Problem Statement

E-commerce platforms lose billions annually to fraudulent transactions, and traditional fraud detection often runs in daily or hourly batches — meaning fraud is caught *after* the damage is done. This project builds a real-time anomaly detection system using an **unsupervised autoencoder** that scores each transaction the instant it occurs, flags suspicious ones, and translates model output into estimated dollars saved — giving business stakeholders an intuitive, live view of fraud prevention instead of a black-box model.

**Why unsupervised?** In the real world, labeled fraud data is scarce and fraud patterns constantly evolve — labels lag behind reality. An autoencoder sidesteps this by learning what "normal" looks like, then flagging anything it can't reconstruct well, without ever needing fraud labels during training.

---

## How It Works

1. **Train on normal behavior only** — the autoencoder is trained exclusively on legitimate transactions, learning to compress and reconstruct them accurately.
2. **Reconstruction error = anomaly score** — when a fraudulent transaction (something the model has never really seen) comes through, the model reconstructs it poorly. That error becomes the anomaly signal.
3. **Threshold-based flagging** — transactions with reconstruction error above the 95th percentile are flagged as suspicious.
4. **Real-time simulation** — transactions are streamed one at a time (simulating a live feed) and scored instantly.
5. **Business-facing dashboard** — a Streamlit app shows live metrics: transactions checked, fraud flagged, and estimated dollars saved — translating model math into numbers a business stakeholder actually cares about.

---

## Results

Evaluated against the ground-truth fraud labels (not used during training):

| Metric | Score |
|---|---|
| ROC-AUC | 0.938 |
| Recall (fraud class) | 0.84 |
| Precision (fraud class) | 0.14 |
| Accuracy | 0.96 |

**What this means in business terms:** the model catches **84% of actual fraud** in real time without ever being trained on a single labeled fraud example. Precision is intentionally traded off — catching more fraud means tolerating more false alarms (14% precision), a tunable threshold decision business teams can adjust based on the cost of a false positive (annoyed customer) vs. a false negative (fraud loss).

---

## Tech Stack

- **Model:** Autoencoder (TensorFlow/Keras)
- **Data:** [Kaggle Credit Card Fraud Detection dataset](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud) (284,807 transactions, 492 fraud)
- **Real-time simulation:** Python generator streaming transactions with delay
- **Dashboard:** Streamlit
- **Tunneling (for Colab demo):** ngrok

---

## Project Structure

```
fraud-detection-realtime/
├── README.md
├── notebook.ipynb        # Full training pipeline: data prep, model, evaluation, streaming sim
├── app.py                 # Streamlit real-time dashboard
├── requirements.txt
└── images/                 # Dashboard screenshots/demo gif
```

---

## How to Run

1. Clone this repo:
   ```bash
   git clone https://github.com/YOUR_USERNAME/fraud-detection-realtime.git
   cd fraud-detection-realtime
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Download the dataset from [Kaggle](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud) and place `creditcard.csv` in the project folder.

4. Run `notebook.ipynb` to train the model and generate `autoencoder_model.keras`, `X_test.pkl`, `threshold.pkl`, and `original_amounts.pkl`.

5. Launch the dashboard:
   ```bash
   streamlit run app.py
   ```

---

## Future Work (Production Roadmap)

This project is scoped for a fast, focused build — here's what a production version would add:

- **True streaming ingestion** via Kafka or AWS Kinesis instead of a simulated generator
- **Model retraining pipeline** to adapt to evolving fraud patterns over time
- **Adjustable threshold slider** in the dashboard so business users can tune the precision/recall tradeoff live
- **Alerting integration** (Slack/email) for flagged high-value transactions
- **A/B testing framework** to compare autoencoder performance against supervised models as labeled fraud data accumulates

---

## How to Add a Demo GIF

Record your Streamlit dashboard running (e.g. with [ScreenToGif](https://www.screentogif.com/) on Windows or [Kap](https://getkap.co/) on Mac), save it as `images/demo.gif`, and it'll show up at the top of this README automatically.

---

## Author

Built as an end-to-end deep learning project — from raw data to a real-time, business-facing dashboard.
