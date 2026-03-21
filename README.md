# 🎮 League of Legends Player Toxicity Analysis

> Identifying and predicting toxic player behaviour in high-ELO ranked matches using in-game performance data, deviation-based proxy labelling, and machine learning.

*Completed as part of ST445 (Managing and Visualising Data) at the London School of Economics. Collaborative project with [lybytes](https://github.com/yh-brain) and [zoey-yhk](https://github.com/zoey-yhk).*

---

## Motivation

Toxic players — those who intentionally feed, abandon objectives, or systematically underperform — are among the most persistent threats to player retention in online multiplayer games. Yet toxicity is notoriously hard to study: players never self-identify as toxic, chat logs are unavailable via public APIs, and there is no ground-truth labelling in any released dataset.

This project sidesteps that problem by asking a different question: **can we reconstruct toxicity as a signal from behavioural traces alone?** The answer, it turns out, is yes — but only with the right methodology.

---

## Research Questions

1. What behavioural patterns distinguish toxic players from normal players?
2. Can in-game decision-making metrics predict toxic behaviour with high accuracy?
3. Do toxic players form natural, separable groupings in the feature space — or is toxicity better understood as an anomalous deviation?

---

## Data

All data was retrieved programmatically from official and publicly available sources, strictly for academic purposes.

| Source | Usage |
|---|---|
| **HuggingFace Datasets** | Challenger-tier match ID corpus (originally sourced from Riot Games) |
| **Riot Games API** | Per-player match statistics, timeline events, champion metadata |
| **Data Dragon & OP.GG** | Champion class metadata and role-specific performance benchmarks |

Focusing on **Challenger-tier** matches was a deliberate choice: high-ELO play provides a well-defined performance baseline, making deviations from expected behaviour more meaningful and less attributable to skill variance.

---

## Methodology

### Proxy Toxicity Labelling

The absence of ground-truth labels is the central challenge of this problem. We addressed it by constructing a **multi-criteria proxy labelling scheme** grounded in the data distributions themselves. A player is flagged as potentially toxic if they trigger **3 or more** of the following behavioural criteria:

| Flag | Description |
|---|---|
| **Underperformance** | Extreme negative deviation from role-adjusted KDA expectations |
| **Resource inefficiency** | CS, gold, and objective participation below peer benchmarks |
| **Irrational itemisation** | Item builds that deviate significantly from optimal or meta builds |
| **Vision abandonment** | Ward score substantially below the role norm |
| **Team disengagement** | Low assist rate, surrender votes, and minimal team coordination |

Critically, **labelling is role-aware**: supports naturally deal less damage; junglers are evaluated on camp efficiency rather than lane CS. This prevents false positives from penalising players for role-appropriate behaviour.

### Modelling

We treated this as both an unsupervised discovery problem and a supervised classification task.

**K-Means Clustering** was used to test whether toxic players form cohesive natural groupings in the feature space. They do not. While Cluster 1 captured a disproportionate share of toxic players, it also contained a large number of non-toxic players — indicating that toxicity is not a geometrically compact phenomenon. This is a meaningful negative result: it tells us that toxic behaviour is diffuse and anomalous, not archetypal.

**Support Vector Machine (SVM)** with class-weight balancing was then trained to learn the decision boundary between toxic and non-toxic players directly. The model achieved:

- **97.9% balanced accuracy**
- **0.998 AUC** on the held-out test set
- **96% recall** on toxic players
- **66% precision** on toxic players

The precision-recall tradeoff here is by design: in a detection context, missing a toxic player (false negative) is more costly than a false positive, so the model is calibrated to maximise recall. The 66× improvement in precision over the clustering baseline (4%) confirms that supervised learning is essential for this problem.

---

## Key Findings

- **Toxicity is multidimensional.** No single metric reliably identifies a toxic player. It is the co-occurrence of extreme deviations across multiple behavioural dimensions — deaths, KDA, vision, team coordination — that constitutes a toxicity signal.

- **Deaths and KDA are the strongest individual predictors**, but their predictive power increases substantially when combined with resource and vision metrics.

- **Toxicity is largely role-invariant** (with the exception of supports), suggesting it reflects a player-level disposition rather than anything inherent to game mechanics or champion kit.

- **Toxic players do not cluster.** This is arguably the most theoretically interesting finding: toxicity is better characterised as a learnable anomaly boundary than a distinct behavioural archetype. Unsupervised methods are insufficient; supervised learning is necessary.

- **The proxy label scheme is robust.** The SVM's strong generalisation performance provides indirect validation that the labelling criteria capture a real and learnable signal, despite the absence of ground-truth annotations.

---

## Limitations & Future Work

- **No chat data.** The Riot Games API does not expose in-game communications. Incorporating NLP over chat logs would allow a richer, multi-modal toxicity signal — distinguishing behavioural toxicity (inting, AFK) from communicative toxicity (flaming, harassment).
- **Proxy labels are imperfect.** A player on a losing streak may trigger multiple flags without being genuinely toxic. Temporal context (e.g. multi-game losing streaks) could improve label quality.
- **Challenger-tier only.** Behavioural norms shift significantly across ELO brackets; the model may not generalise to lower ranks without recalibration.

---

## Tools & Libraries

`Python` · `NumPy` · `Pandas` · `Scikit-learn` · `Matplotlib` · `Seaborn` · `Plotly` · `Requests` · `BeautifulSoup` · `HuggingFace Datasets` · `Riot Games API`

---

## Setup

```bash
git clone https://github.com/YOUR_USERNAME/lol-toxicity-analysis.git
cd lol-toxicity-analysis

conda env create -f environment.yml
conda activate <env-name>

jupyter notebook ST445_Project_Main.ipynb
```

You will need:
- A [Riot Games API key](https://developer.riotgames.com/) → save to `ST445_Project_Riot API Key.txt`
- A [HuggingFace access token](https://huggingface.co/settings/tokens) → save to `ST445_Project_hf token.txt`

---

*All data was collected in accordance with the Riot Games API Terms of Service. No personal player data is stored or redistributed.*
