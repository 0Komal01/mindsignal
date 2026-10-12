# 🧠 MindSignal

**Explainable depression screening for adolescents, from the words they write online.**

MindSignal is an AI screening aid that reads social-media style text, estimates a four-level severity (Not Depressed, Mild, Moderate, Severe), and highlights the exact words behind each result so a counselor can see *why* a post was flagged.

> ⚠️ **MindSignal is a screening aid, not a diagnostic tool.** Every result needs review by a trained person. If someone may be in danger, contact local emergency services or a crisis helpline (India: Tele-MANAS 14416, emergency 112).


## Why

- An estimated 280 million people worldwide live with depression (WHO), and most adolescents who are affected are never diagnosed or treated.
- Stigma and a shortage of specialists mean young people often say "I'm fine" in person, yet they often write about their distress online first.
- Schools and clinics cannot manually screen every student.
- Black-box AI gives a label but no reason, so counselors cannot trust or audit it.

## What it does

| Component | Role |
|---|---|
| **MentalBERT** | Understands the meaning of a post using a language model pre-trained on mental-health text (768-d). |
| **LIWC psycholinguistic features** | 20 clinically meaningful cues such as negative emotion, first-person focus, absolutist words and past-tense use. |
| **Attention gate** | Learns how much to trust the text meaning versus the language cues. |
| **BiLSTM classifier** | Reads context and decides depressed or not depressed. |
| **PHQ-9 style severity** | Maps the result to four levels to help counselors prioritise. |
| **LIME explanations** | Highlights the words that drove the prediction. |
| **Safety layer (demo)** | Death-related language triggers an urgent human-review notice with helpline guidance. |

```
Post text → MentalBERT + 20 LIWC features → Attention gate → BiLSTM → Severity level + LIME words
```

## Results

Evaluated on 3,687 held-out test posts (stratified 70 / 15 / 15 split of 24,574 Reddit and Twitter posts).

| Model | Accuracy | F1 | AUC-ROC |
|---|---|---|---|
| TF-IDF + Logistic Regression | 0.7795 | 0.7793 | 0.8676 |
| BERT-base | 0.8221 | 0.8220 | 0.9156 |
| MentalBERT | 0.8194 | 0.8190 | 0.9171 |
| MentalBERT + LIWC | 0.8245 | 0.8244 | 0.9181 |
| **Full model (gate + BiLSTM)** | **0.8218** | **0.8218** | **0.9137** |

- 5-fold cross-validation F1: **0.8181 ± 0.0057**
- Cross-platform F1: Reddit-only model on Twitter **0.5466**; mixed-platform training lifts Twitter to **0.7686** (Reddit **0.9437**).
- Test-set severity: Not Depressed 50.5%, Mild 2.3%, Moderate 31.7%, Severe 15.6%.

## Demo

The web demo (`MindSignal_Demo.html`) runs fully in your browser. In demo mode it computes the psycholinguistic-cue stage of the model with LIWC-style word lists and a distress score, highlights the cue words, and shows the safety notice. Full MentalBERT scoring and LIME explanations run in the notebooks. No text leaves your device.

Try the three built-in examples (severe cues, mild cues, no cues) or paste your own sample text.

## Repository structure

```
mindsignal/
├── MindSignal_Demo.html            # Web demo (GitHub Pages)
├── Notebook_1.ipynb                # Data preparation and exploration
├── Notebook_2.ipynb                # Model training and evaluation
├── Notebook_3.ipynb                # Results, ablation and explainability
├── MindSignal_Concept_Note.docx    # Concept note (Why, What, Whom, SDGs)
├── LICENSE
└── README.md
```

## Run it

**Web demo**

Open `MindSignal_Demo.html` in any modern browser, or enable GitHub Pages (Settings → Pages → Deploy from branch → `main` / root).

**Notebooks**

```bash
git clone https://github.com/<your-username>/mindsignal.git
cd mindsignal
pip install -r requirements.txt
jupyter notebook notebooks/
```

Run the notebooks in numeric order. A GPU is recommended for training; Google Colab or Kaggle works well.

## Sustainable Development Goals

| SDG | How MindSignal contributes |
|---|---|
| **SDG 3 – Good Health and Well-being** (primary) | Target 3.4: promotes mental health and well-being through early, scalable detection. |
| **SDG 4 – Quality Education** | Helps schools build safe, supportive learning environments. |
| **SDG 10 – Reduced Inequalities** | Low-cost screening can reach areas with few mental-health professionals. |
| **SDG 9 – Industry, Innovation and Infrastructure** | Applies responsible, explainable AI to a real public-health problem. |

## Who benefits

Students and adolescents · School and college counselors · Parents and teachers · Primary care clinics · NGOs and public-health programs · Researchers and developers


## Responsible use and limitations

- Screening aid only; a trained person must review every flag.
- Severity weights are heuristic and not yet clinically validated.
- User ages could not be confirmed for every post.
- Use only with consent, anonymisation and privacy safeguards (such as India's DPDP Act). Do not use the datasets to identify individuals.
- Datasets are not redistributed here; please obtain them from their original sources under their licenses.

## Roadmap

1. Clinical validation of severity levels with experts.
2. Hinglish and Gujarati support.
3. Counselor dashboard with triage queue, trends and early-warning alerts.

## License

MIT License (see `LICENSE`).

## Acknowledgements

MentalBERT, LIWC, LIME, Hugging Face Transformers and PyTorch, and the open Reddit and Twitter mental-health datasets that made this work possible.
