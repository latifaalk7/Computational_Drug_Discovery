# Computational Drug Discovery Analysis

### How much can we learn about a drug simply from its molecular structure?
---

## Project Overview

<img width="792" height="274" alt="Screenshot 2026-09-05 at 4 42 37 PM" src="https://github.com/user-attachments/assets/47591408-fa90-4185-bc26-c4d7db028fa5" />


This project explores how much we can learn about a drug from its molecular structure. I analysed 8 drug molecules, converted their structures into numerical fingerprints, measured their similarity, calculated key chemical properties, and tested their drug-likeness using Lipinski's Rule of Five.

I then analysed EGFR inhibitor data from ChEMBL to explore whether molecular information can help explain biological activity.

### 📌 Key Finding

The results show that molecular structure provides useful information, but it cannot fully explain how strongly a drug acts on a biological target.


---

### Project Workflow

 The project follows a simple progression:

```text 
Molecular Structures
        ↓
Structure Standardisation
        ↓
Molecular Fingerprints
        ↓
Structural Similarity
        ↓
Chemical Properties
        ↓
Lipinski Rule of Five
        ↓
EGFR Activity Data
        ↓
      IC50
        ↓
Structure → Activity
```

The analysis therefore moves from **what a molecule looks like** to **what properties it has** and finally to **how it behaves biologically**.

---

## Molecular Fingerprints 

To compare molecules using a computer, I converted each chemical structure into a molecular fingerprint.

A fingerprint is a list of numbers showing which structural features are present:

```text
1 = structural feature is present
0 = structural feature is absent
```

### 📊 Morgan Fingerprints
The analysis used **Morgan fingerprints with 1,024 bits**.

| Drug          | Active bits |
| ------------- | ----------: | 
| Aspirin       |          24 | 
| Caffeine      |          24 | 
| Ibuprofen     |          25 | 
| Paracetamol   |          19 |
| Ciprofloxacin |          45 |
| Sildenafil    |          65 | 
| Erlotinib     |          47 |
| Imatinib      |          65 | 


The number of active bits varies considerably. This indicates that the molecules contain very different numbers of structural features captured by the fingerprint.
More active fingerprint bits does not mean a drug is more effective. It reflects differences in molecular structure and complexity.

---

## The Effect of Fingerprint Radius

Morgan fingerprints can look at different amounts of the structure around each atom.

For aspirin:

<img width="794" height="187" alt="Screenshot 2026-09-05 at 4 45 24 PM" src="https://github.com/user-attachments/assets/765a5e12-92b5-4c30-8951-00b4b61e8a2b" />

As the radius increases, the fingerprint considers a larger structural neighbourhood around each atom.

This means that increasing the radius allows the representation to capture **more structural context**.

```text
Radius 0
   ↓
Local atomic information

Radius 1
   ↓
Atoms + neighbouring structure

Radius 2
   ↓
Larger molecular environments
```

---

## MACCS Fingerprints

I also generated MACCS fingerprints, which use a fixed set of chemical features.

Using both Morgan and MACCS fingerprints allowed me to compare how different representations affect molecular similarity. While Morgan fingerprints identify structural patterns through molecular environments, MACCS fingerprints use a predefined set of chemical substructure keys.

<img width="792" height="242" alt="Screenshot 2026-09-05 at 4 48 51 PM" src="https://github.com/user-attachments/assets/fc23a81c-11d0-4e20-93c6-fcf75fd4381c" />

---

# Molecular Similarity

I used Tanimoto similarity to measure how similar different molecules are.

```text
0 → Low similarity
1 → High similarity
```
<img width="778" height="120" alt="Screenshot 2026-09-05 at 8 47 46 PM" src="https://github.com/user-attachments/assets/8f007cf1-9733-47d9-8081-be298ad8cb9e" />

Erlotinib and Imatinib are both kinase inhibitors, and their MACCS similarity (0.485) was higher than their similarity with aspirin.
However, their Morgan similarity was only 0.167.
This shows that: Molecules with related biological effects do not always have very similar structures.
The measured similarity depends on the fingerprint method being used.

---

## Physicochemical Properties

The analysis calculated:

* **MW** — Molecular Weight
* **LogP** — Lipophilicity
* **HBD** — Hydrogen Bond Donors
* **HBA** — Hydrogen Bond Acceptors
* **RotB** — Rotatable Bonds
* **TPSA** — Topological Polar Surface Area
* **Rings** — Total Ring Count
* **ArRings** — Aromatic Ring Count

## Molecular Descriptors

<img width="791" height="341" alt="Screenshot 2026-09-05 at 8 57 01 PM" src="https://github.com/user-attachments/assets/4fe10b81-a1a9-4371-9948-90cf0c51888b" />

| Drug          |     MW |  LogP | HBD | HBA | RotB |  TPSA | Rings | ArRings |
| ------------- | -----: | ----: | --: | --: | ---: | ----: | ----: | ------: |
| Aspirin       | 180.16 |  1.31 |   1 |   3 |    2 | 63.60 |     1 |       1 |
| Caffeine      | 194.19 | -1.03 |   0 |   3 |    0 | 61.82 |     2 |       2 |
| Ibuprofen     | 206.28 |  3.07 |   1 |   1 |    4 | 37.30 |     1 |       1 |
| Paracetamol   | 151.16 |  1.35 |   2 |   2 |    1 | 49.33 |     1 |       1 |
| Ciprofloxacin | 331.35 |  1.58 |   2 |   4 |    3 | 74.57 |     4 |       2 |
| Sildenafil    | 539.70 |  3.30 |   1 |   6 |   10 | 96.77 |     4 |       3 |
| Erlotinib     | 393.44 |  3.41 |   1 |   7 |   10 | 74.73 |     3 |       3 |
| Imatinib      | 493.62 |  4.59 |   2 |   7 |    7 | 86.28 |     5 |       4 |


The data shows a substantial difference between the simpler and more structurally complex drugs.

For example:

### Paracetamol

```text
MW       = 151.16
Rings    = 1
RotB     = 1
HBD      = 2
HBA      = 2
```

Compared with:

### Imatinib

```text
MW       = 493.62
Rings    = 5
RotB     = 7
HBD      = 2
HBA      = 7
LogP     = 4.59
```

This shows that the drugs occupy very different chemical spaces. This is important because molecular structure doesn't just determine how a molecule looks. It influences properties such as size, polarity, flexibility and lipophilicity.

---

## Lipinski's Rule of Five

I used Lipinski's Rule of Five to check whether the molecules have common drug-like properties.

| Property         | Threshold |
| ---------------- | --------: |
| Molecular Weight |     ≤ 500 |
| LogP             |       ≤ 5 |
| HBD              |       ≤ 5 |
| HBA              |      ≤ 10 |

### Results

<img width="790" height="442" alt="Screenshot 2026-09-05 at 9 00 33 PM" src="https://github.com/user-attachments/assets/11410f78-204f-44b6-b91e-f46768c1b51c" />



7 of 8 drugs met all four limits.

Sildenafil was the only drug with a violation because its molecular weight was 539.70 Da, above the 500 Da limit.

Using the criterion applied in this analysis, it still passes overall because it has fewer than two violations.

Key takeaway
A more complex molecule does not automatically mean a molecule is less drug-like.

---

## EGFR Biological Activity
<img width="791" height="298" alt="Screenshot 2026-09-05 at 9 02 23 PM" src="https://github.com/user-attachments/assets/1e40b5cb-a2e8-4660-a682-1feeae701ea2" />

To move from chemical properties to biological activity, I analysed EGFR inhibitor data from ChEMBL.

The main measurement was IC50 which is the concentration needed to reduce a biological activity by 50% under a particular experimental setup.

Lower IC50 → Stronger inhibition
Higher IC50 → Weaker inhibition

### 📊 IC50 Distribution

CHEMBL68920 had three reported IC50 measurements:

41 nM
300 nM
7,820 nM

The molecular descriptors were the same for these records.

This shows that molecular structure alone cannot explain every biological measurement.

Differences in experiments, assay conditions and biological context can affect the measured IC50.

---

## 🧬 Final Takeaway

This project follows the journey from molecular structure to biological activity.

```text
STRUCTURE
    ↓
FINGERPRINTS
    ↓
SIMILARITY
    ↓
CHEMICAL PROPERTIES
    ↓
DRUG-LIKENESS
    ↓
BIOLOGICAL ACTIVITY
```

> The main finding is that molecular structure contains useful information about a drug, but it cannot completely explain biological activity.
The project therefore provides a foundation for using data science and machine learning to study and predict drug behaviour.

## 🛠️ Technologies
Python,
RDKit,
Pandas,
NumPy,
Matplotlib,
Seaborn,
ChEMBL,
Jupyter Notebook

## 📁 Repository Structure
```text
computational-drug-discovery/
│
├── images/
│   ├── egfr_ic50_distribution.png
│   ├── maccs_fingerprints.png
│   ├── morgan_fingerprints.png
│   ├── morgan_radius_comparison.png
│   ├── physicochemical_descriptors.png
│
├── notebooks/
│   └── computational_drug_discovery.ipynb
│
├── requirements.txt
└── README.md
```

Latifa Al Khalifa

Data Science & Business Analytics
