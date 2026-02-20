# Beyond the Black Box: Can Machine Learning Uncover Interpretable Rules for Quintic Polynomials?

**Rohan Thomas<sup>1</sup>, Majid Bani-Yaghoub<sup>2</sup>**

<sup>1</sup> The Barstow School, Kansas City, MO, USA  
<sup>2</sup> Division of Computing, Analytics & Mathematics, University of Missouri-Kansas City, Kansas City, MO, USA

---

## Overview

This repository contains the code and experiments for our paper investigating whether machine learning can autonomously discover interpretable mathematical rules for polynomial root classification. We focus on the quintic case, where the Abel–Ruffini theorem guarantees no closed-form algebraic solution exists.

We evaluate decision trees, logistic regression, SVMs, random forests, gradient boosting, XGBoost, symbolic regression (PySR), and neural networks across polynomial degrees 2–5. Our main findings:

- Neural networks achieve 84.2% ± 5.0% balanced accuracy on quintic classification from raw coefficients, but remain uninterpretable.
- Decision trees achieve only 60.7% ± 3.8% on the same task—a 23.5-point gap.
- When provided a human-engineered feature (Crit8: sign changes at critical points), decision trees jump to 86.7% ± 5.2%, nearly matching neural networks.
- Knowledge distillation with 98.9% fidelity reveals Crit8 accounts for 97.5% of the distilled tree's structure.
- No model autonomously discovered interpretable rules without explicit feature engineering.

## Repository Structure

```
MLPolyRoots/
├── 01_Multi-model_evaluation.ipynb    # Model screening across degrees 2–5
├── 02_NNs_and_Distillation.ipynb      # Neural networks, feature engineering, distillation
├── 03_Symbolic_Regression.ipynb       # PySR experiments across degrees 2–5
├── requirements.txt                   # Python dependencies
├── LICENSE                            # MIT License
└── README.md
```

### Notebook Descriptions

**`01_Multi-model_evaluation.ipynb`**  
Evaluates all model classes (CART, logistic regression, SVM, random forest, gradient boosting, XGBoost, neural networks) on polynomial root classification for degrees 2–5. Includes cross-validation with SMOTE balancing, SHAP analysis, PCA visualization, and multi-seed statistical comparison. This notebook produces the screening results reported in Table 2 of the paper.

**`02_NNs_and_Distillation.ipynb`**  
Three-part notebook:
1. *Degree 2–4 validation*: Confirms that decision trees can recover known algebraic discriminants (quadratic, cubic, quartic) when provided appropriate invariant features.
2. *Quintic feature engineering*: Implements all 63 features from six classical methods (Sturm sequences, Descartes' rule, Newton's sums, critical points, hybrid symbolic invariants, decomposition features) and evaluates each group individually and combined.
3. *Knowledge distillation*: Trains neural networks on the full feature set, distills into decision trees, and performs SHAP-based feature importance analysis identifying Crit8 as the dominant feature.

**`03_Symbolic_Regression.ipynb`**  
Runs PySR (genetic programming–based symbolic regression) on degrees 2–5 to test whether symbolic methods can discover algebraic classification rules directly from coefficients. Demonstrates the "complexity cliff"—near-perfect results on quadratics, rapid degradation for higher degrees.

## Getting Started

### Prerequisites

- Python 3.10+
- Julia 1.9+ (required by PySR for symbolic regression; see [PySR installation](https://astroautomata.com/PySR/installation/))

### Installation

```bash
git clone https://github.com/rohanthomas334-sketch/MLPolyRoots.git
cd MLPolyRoots
pip install -r requirements.txt
```

### Running the Experiments

The notebooks are intended to be run in order, though each is self-contained. All data is generated synthetically at runtime—no external datasets are needed.

```bash
jupyter notebook
```

Open each notebook and run all cells. Expected runtimes on a standard machine:

| Notebook | Approximate Runtime |
|----------|-------------------|
| `01_Multi-model_evaluation.ipynb` | 30–60 min |
| `02_NNs_and_Distillation.ipynb` | 20–40 min |
| `03_Symbolic_Regression.ipynb` | 10–30 min (varies with PySR timeout settings) |

Runtimes will vary depending on hardware. The symbolic regression notebook is particularly sensitive to CPU speed, as PySR runs evolutionary search in Julia.

### Reproducibility

All experiments use fixed random seeds (documented in each notebook) for the data generation and train/test splits. The 20-trial evaluations in Notebooks 01 and 02 iterate over seeds 0–19 to produce the mean ± std statistics reported in the paper. PySR results may show minor variation across runs due to the stochastic nature of genetic programming, but the qualitative findings (success on quadratics, failure on higher degrees) are consistent.

## Key Results Summary

| Model | Raw Coefficients | With Crit8 |
|-------|-----------------|------------|
| Neural Networks | 84.2% ± 5.0% | 88.7% ± 5.0% |
| Decision Trees | 60.7% ± 3.8% | 86.7% ± 5.2% |

Distillation metrics (63 engineered features → decision tree):

| Metric | Value |
|--------|-------|
| NN test accuracy | 85.6% ± 4.1% |
| Tree fidelity to NN | 98.9% ± 0.5% |
| Tree standalone accuracy | 84.9% ± 5.3% |
| Crit8 feature importance (SHAP) | 97.5% |

## Dependencies

Core dependencies are listed in `requirements.txt`. The main libraries used:

- **scikit-learn** — classifiers, evaluation metrics, preprocessing
- **NumPy / SciPy** — polynomial root computation, numerical methods
- **PySR** — symbolic regression via genetic programming (requires Julia backend)
- **XGBoost** — gradient boosting baseline
- **imbalanced-learn** — SMOTE oversampling for the screening phase
- **SHAP** — feature importance analysis for distillation
- **matplotlib / seaborn** — visualization

## Citation

If you use this code or build on this work, please cite:

```bibtex
@article{thomas2025beyondblackbox,
  title={Beyond the Black Box: Can Machine Learning Uncover Interpretable Rules for Quintic Polynomials?},
  author={Thomas, Rohan and Bani-Yaghoub, Majid},
  year={2025}
}
```

## License

This project is released under the [MIT License](LICENSE).
