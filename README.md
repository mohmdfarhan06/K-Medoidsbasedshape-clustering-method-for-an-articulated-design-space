# K-Medoids Based Shape Clustering for Articulated Design Spaces

[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![CI](https://github.com/user/kmedoids-shape-clustering/actions/workflows/ci.yml/badge.svg)](.github/workflows/ci.yml)

## Overview

This repository provides a modular, test-driven machine learning framework for clustering 2D geometric shape representations using **K-Medoids**. 

In generative design workflows, algorithmically synthesized geometry often contains significant structural redundancy. Standard clustering methods like K-Means calculate artificial centroids that do not correspond to actual physical or valid geometries. **K-Medoids** resolves this limitation by selecting actual observations (medoids) from the dataset to serve as exact, physical representatives for each cluster.

---

## Project Structure

```text
kmedoids-shape-clustering/
├── README.md
├── requirements.txt
├── .gitignore
├── pytest.ini
├── LICENSE
├── Dockerfile
│
├── .github/
│   └── workflows/
│       └── ci.yml
│
├── data/
│   ├── README.md
│   ├── raw/
│   ├── processed/
│   └── sample/
│       └── sample_shapes.csv
│
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   ├── 02_shape_preprocessing.ipynb
│   ├── 03_kmedoids_clustering.ipynb
│   └── 04_cluster_evaluation.ipynb
│
├── src/
│   ├── __init__.py
│   ├── config.py
│   ├── data/
│   │   ├── __init__.py
│   │   ├── loader.py
│   │   └── preprocessing.py
│   ├── features/
│   │   ├── __init__.py
│   │   └── shape_features.py
│   ├── clustering/
│   │   ├── __init__.py
│   │   ├── kmedoids.py
│   │   └── cluster_selection.py
│   ├── evaluation/
│   │   ├── __init__.py
│   │   └── metrics.py
│   └── visualization/
│       ├── __init__.py
│       └── plots.py
│
├── outputs/
│   ├── clusters.csv
│   ├── medoids.csv
│   ├── cluster_plot.png
│   ├── silhouette_plot.png
│   └── elbow_plot.png
│
└── tests/
    ├── __init__.py
    ├── test_loader.py
    ├── test_preprocessing.py
    ├── test_features.py
    ├── test_clustering.py
    └── test_evaluation.py
```

---

## Architecture & Data Pipeline

```text
                 2D SHAPE DATA
                       │
                       ↓
              ┌─────────────────┐
              │ Data Loading    │
              └────────┬────────┘
                       ↓
              ┌─────────────────┐
              │ Preprocessing   │
              │ Standardize     │
              └────────┬────────┘
                       ↓
              ┌─────────────────┐
              │ Feature         │
              │ Extraction      │
              └────────┬────────┘
                       ↓
              ┌─────────────────┐
              │ K-Medoids       │
              │ Clustering      │
              └────────┬────────┘
                       ↓
          ┌────────────┴────────────┐
          ↓                         ↓
   Shape Clusters              Medoid Shapes
          │                         │
          └────────────┬────────────┘
                       ↓
              ┌─────────────────┐
              │ Evaluation      │
              │ Silhouette Score│
              └────────┬────────┘
                       ↓
              ┌─────────────────┐
              │ Visualization   │
              └─────────────────┘
```

---

## Objectives

- **Identify Geometric Clusters**: Group similar candidate designs based on extracted geometric descriptors (area, perimeter, aspect ratio, compactness).
- **Reduce Design Redundancy**: Filter out redundant shape variations across large generative design spaces.
- **Select Valid Representative Shapes**: Extract exact dataset observations as medoids for downstream inspection and CAD export.
- **Unsupervised Evaluation**: Validate clustering quality using the **Silhouette Score** and **Davies-Bouldin Index**.
- **Automated QA & CI/CD**: Enforce rigorous data validation, bounds testing, and model assertion tests via `pytest` integrated with GitHub Actions.

---

## Key Tech Stack

| Technology | Purpose |
| :--- | :--- |
| **Python 3.11** | Core Development Language |
| **NumPy & Pandas** | Vectorized Numerical Operations & Tabular Manipulation |
| **Scikit-learn** | Standard Scaling, Metric Calculations |
| **scikit-learn-extra** | K-Medoids Clustering Implementation |
| **Matplotlib** | Cluster & Silhouette Visualization |
| **pytest** | Automated Unit & Integration Testing |
| **GitHub Actions** | Continuous Integration Pipeline |
| **Docker** | Containerized Execution Environment |

---

## Quickstart Guide

### 1. Clone & Setup Environment

```bash
git clone https://github.com/user/kmedoids-shape-clustering.git
cd kmedoids-shape-clustering

python -m venv venv
# On Windows:
venv\Scripts\activate
# On Linux/macOS:
source venv/bin/activate

pip install -r requirements.txt
```

### 2. Run Main Pipeline

```bash
python -m src.main
```

### 3. Run Automated Tests

```bash
pytest
```

---

## Evaluation Metrics

Because shape clustering in generative spaces is unsupervised (lacking ground-truth class labels), clustering performance is validated using:

1. **Silhouette Score**: Measures how similar an object is to its own cluster compared to other clusters. Scores range from -1 to 1 (higher is better).
2. **Davies-Bouldin Index**: Assesses average similarity measure of each cluster with its most similar cluster. Lower values indicate better separation and compactness.

---

## Testing Strategy & QA

Automated test suites in `tests/` cover:
- **Data Loader Validation**: Testing missing/malformed files and empty datasets.
- **Preprocessing Guardrails**: Verifying schema compliance and handling of missing columns.
- **Feature Math Invariants**: Testing mathematical bounds (e.g., perimeter $\le 0$, height $\le 0$).
- **Model Invariants**: Ensuring the number of assigned medoid indices strictly matches the target number of clusters $K$.

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
