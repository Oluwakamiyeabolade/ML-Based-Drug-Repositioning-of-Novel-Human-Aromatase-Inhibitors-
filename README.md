
# Machine Learning-Based Drug Repositioning of Novel Human Aromatase Inhibitors

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue)](https://www.python.org/)
[![RDKit](https://img.shields.io/badge/RDKit-2023-green)](https://www.rdkit.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.x-orange)](https://scikit-learn.org/)

This repository contains the machine learning pipeline used in the study:

> **"Machine Learning-Based Drug Repositioning of Novel Human Aromatase Inhibitors Utilizing Molecular Docking and Molecular Dynamic Simulation"**  
> Published on [Journal of Computational Biophysics and Chemistry](https://www.worldscientific.com/doi/10.1142/S2737416524410035)

---

## Overview

Aromatase (CYP19A1) is a key enzyme in estrogen biosynthesis and a validated target in estrogen receptor-positive (ER+) breast cancer. This project applies a **drug repositioning** strategy to identify novel aromatase inhibitors from the ChEMBL database using:

1. **Molecular descriptor calculation** from SMILES strings via RDKit
2. **Random Forest regression** to predict pIC50 activity
3. **Virtual screening** of ~1.5 million ChEMBL compounds
4. **Hit filtering** based on predicted pIC50 thresholds

Top predicted candidates were subsequently validated through **molecular docking** and **molecular dynamics (MD) simulation** (described in the paper).

---

## Repository Structure

```
aromatase-ml-repo/
├── notebooks/
│   └── Aromatase.ipynb         # Main ML pipeline notebook
├── data/
│   ├── aromatase_inhibitors.csv          # Known aromatase inhibitor reference library (pIC50 from ChEMBL)
│   └── chembl_22_clean_1576904_sorted_std_final.smi  # ChEMBL small molecule library (~1.5M compounds)
├── results/
│   └── filtered.csv            # Top predicted aromatase inhibitor candidates (pIC50 > 8.8)
├── requirements.txt
├── LICENSE
└── README.md
```

> **Note:** The large ChEMBL `.smi` file (~1.5M compounds) is not included in this repository due to size. It can be downloaded from the [ChEMBL database](https://www.ebi.ac.uk/chembl/).

---

## Methodology

### 1. Feature Engineering
Lipinski-based and physicochemical descriptors were computed for each molecule using RDKit:

| Feature | Description |
|---|---|
| MW | Molecular Weight |
| LogP | Lipophilicity (Crippen) |
| HBA | H-Bond Acceptors (Lipinski) |
| HBD | H-Bond Donors (Lipinski) |
| CSP3 | Fraction of sp³ carbons |
| NumRotBond | Number of Rotatable Bonds |
| NumRings | Total Ring Count |
| TPSA | Topological Polar Surface Area |
| NumAromaticRings | Aromatic Ring Count |

### 2. Model Training
A **Random Forest Regressor** was trained on known aromatase inhibitors (from ChEMBL) to predict **pIC50** values.

- 5-fold cross-validation for robust evaluation
- 80/20 train-test split for final performance reporting
- Metrics: R², MAE, MSE, RMSE

### 3. Virtual Screening
The trained model was applied to screen all ~1.5 million ChEMBL compounds. Compounds with predicted **pIC50 > 8.8** were shortlisted as candidate inhibitors.

### 4. Downstream Validation (see paper)
Top hits were assessed via:
- Molecular docking against the crystal structure of human aromatase
- Molecular dynamics (MD) simulations to evaluate binding stability

---

## Results

The model achieved strong predictive performance on the test set. Feature importance analysis identified **LogP**, **TPSA**, and **MW** as the most influential descriptors for aromatase inhibition prediction.

Top-ranked candidates with pIC50 > 8.8 were exported to `results/filtered.csv` for docking studies.

---

---

## Citation

If you use this code in your research, please cite:

```
[Full citation available at the publication link above]
```

---

## License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.
