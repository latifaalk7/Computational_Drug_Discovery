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

<img width="493" height="298" alt="Screenshot 2026-09-05 at 8 41 35 PM" src="https://github.com/user-attachments/assets/3a56853a-dae5-4f44-af88-e1defaa5bd9d" />



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

# 🔗 Molecular Similarity

I used Tanimoto similarity to measure how similar different molecules are.

```text
0 → Low similarity
1 → High similarity
```
<img width="778" height="120" alt="Screenshot 2026-09-05 at 8 47 46 PM" src="https://github.com/user-attachments/assets/8f007cf1-9733-47d9-8081-be298ad8cb9e" />

Erlotinib and Imatinib are both kinase inhibitors, and their MACCS similarity (0.485) was higher than their similarity with aspirin.

However, their Morgan similarity was only 0.167.

This shows that:

Molecules with related biological effects do not always have very similar structures.
The measured similarity depends on the fingerprint method being used.

---

# ⚗️ 3. Physicochemical Properties

Fingerprints tell us about **structural features**.

The next question is:

> **What chemical and physical properties result from those structures?**

The analysis calculated:

* **MW** — Molecular Weight
* **LogP** — Lipophilicity
* **HBD** — Hydrogen Bond Donors
* **HBA** — Hydrogen Bond Acceptors
* **RotB** — Rotatable Bonds
* **TPSA** — Topological Polar Surface Area
* **Rings** — Total Ring Count
* **ArRings** — Aromatic Ring Count

## 📊 Molecular Descriptors

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

### 📈 What stands out?

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

The molecules therefore occupy very different regions of **physicochemical space**.

This is important because molecular structure doesn't just determine how a molecule looks — it influences properties such as size, polarity, flexibility and lipophilicity.

![Physicochemical Properties](images/physicochemical_descriptors.png)

---

# 💊 4. Lipinski's Rule of Five

The next question is:

> **Despite their structural differences, do these molecules have broadly drug-like physicochemical properties?**

To investigate this, the analysis applies **Lipinski's Rule of Five**.

| Property         | Threshold |
| ---------------- | --------: |
| Molecular Weight |     ≤ 500 |
| LogP             |       ≤ 5 |
| HBD              |       ≤ 5 |
| HBA              |      ≤ 10 |

## Results

| Drug          | Violations | Overall Result |
| ------------- | ---------: | -------------- |
| Aspirin       |          0 | ✓ Pass         |
| Caffeine      |          0 | ✓ Pass         |
| Ibuprofen     |          0 | ✓ Pass         |
| Paracetamol   |          0 | ✓ Pass         |
| Ciprofloxacin |          0 | ✓ Pass         |
| Sildenafil    |          1 | ✓ Pass         |
| Erlotinib     |          0 | ✓ Pass         |
| Imatinib      |          0 | ✓ Pass         |

### 🔎 The interesting result

Seven of the eight compounds satisfy **all four individual thresholds**.

Sildenafil is the exception because:

```text
Molecular Weight = 539.70
Lipinski threshold = 500
```

Therefore, sildenafil has **one violation**.

However, using the criterion applied in this analysis — passing with fewer than two violations — it still passes the overall Rule-of-Five classification.

### 💡 What does this tell us?

The result challenges a simple assumption:

> **"A more complex molecule must be less drug-like."**

The data shows that this isn't necessarily true.

Imatinib, for example, is substantially more complex than paracetamol, but it still satisfies the selected Lipinski criteria.

Therefore:

> **Drug-likeness is not the same as structural simplicity.**

---

# 🧪 5. EGFR Biological Activity

So far, the analysis has focused on the molecules themselves.

But there is a bigger question:

> **Do these chemical properties actually translate into biological activity?**

To investigate this, the project examines **EGFR inhibitor data from ChEMBL** using IC50 measurements.

## What is IC50?

IC50 represents the concentration of a compound required to produce **50% inhibition** of a biological target under a particular assay.

In simple terms:

```text
Lower IC50
      ↓
Stronger inhibition

Higher IC50
      ↓
Weaker inhibition
```

---

## 📊 IC50 Distribution

The EGFR dataset contains IC50 measurements spanning a very large range.

Examples include:

```text
40 nM
41 nM
170 nM
300 nM
440 nM
7,820 nM
9,300 nM
500,000 nM
```

The median IC50 is approximately:

> **508 nM**

This demonstrates that EGFR inhibitor potency can vary by several orders of magnitude.

![EGFR IC50 Distribution](images/egfr_ic50_distribution.png)

---

## 📉 Why Use a Logarithmic Scale?

The IC50 distribution is highly skewed.

A small number of extremely large values — including an observation around **500,000 nM** — stretch the linear x-axis.

As a result, most of the observations become compressed near the lower end.

A logarithmic transformation makes the distribution much easier to interpret.

![EGFR IC50 Log Distribution](images/egfr_ic50_log_distribution.png)

### Key lesson

> **Biological activity data often spans multiple orders of magnitude, making log-transformation useful for visualising and analysing potency.**

---

## 🔬 The Same Molecule Can Have Different IC50 Measurements

One particularly interesting observation is that **CHEMBL68920** appears with multiple reported IC50 values:

```text
41 nM
300 nM
7,820 nM
```

The molecular descriptors remain the same across these records.

This highlights an important limitation:

> **Molecular structure alone cannot explain every biological activity measurement.**

Differences in experimental conditions, assay systems, biological context and measurement variability can contribute to different observed IC50 values.

This is particularly important when developing machine-learning models for drug discovery.

---

# 🧠 6. Key Findings

## 🔹 1. Chemical structures can be converted into numerical data

Molecular fingerprints transform chemical structures into machine-readable features.

This makes it possible to perform quantitative comparisons between molecules.

---

## 🔹 2. Molecular complexity varies substantially

The eight drugs activate very different numbers of fingerprint features.

For example:

```text
Paracetamol → 19 active Morgan bits
Sildenafil   → 65 active Morgan bits
Imatinib     → 65 active Morgan bits
```

This reflects substantial structural diversity.

---

## 🔹 3. Similarity depends on how it is measured

Erlotinib and Imatinib have:

```text
Morgan → 0.167
MACCS  → 0.485
```

The difference demonstrates that molecular similarity is dependent on the representation used.

---

## 🔹 4. Structurally complex molecules can still be drug-like

Despite substantial differences in structure, all eight compounds pass the Rule-of-Five criterion used in this analysis.

Sildenafil has one violation because of its molecular weight.

---

## 🔹 5. Drug-likeness and biological potency are different concepts

A compound satisfying Lipinski's criteria does not automatically mean it will strongly inhibit a particular biological target.

Lipinski evaluates broad physicochemical characteristics.

IC50 measures biological activity against a specific target under an experimental setup.

They answer **different questions**.

---

## 🔹 6. Biological activity varies dramatically

EGFR inhibitor IC50 values span from tens of nanomolar to hundreds of thousands of nanomolar.

This means compounds targeting the same biological target can have dramatically different levels of potency.

---

## 🔹 7. Structure provides information — but not the whole story

The overall analysis leads to an important conclusion:

```text
Molecular Structure
       ↓
Structural Features
       ↓
Physicochemical Properties
       ↓
Drug-Likeness
       ↓
          ?
       ↓
Biological Activity
```

The final relationship is not perfectly predictable.

**Biological activity is influenced by more than molecular structure alone.**

---


# 🧬 Final Takeaway

This project tells a story that moves from **chemistry to data science to biology**.

We begin with molecular structures.

We convert those structures into numerical fingerprints.

We use those fingerprints to measure similarity.

We calculate physicochemical properties to understand how the molecules differ.

We test whether those properties satisfy common drug-likeness criteria.

Finally, we examine biological activity and discover that potency can vary enormously — meaning that **structure alone cannot completely explain biological behaviour**.

The overall journey is:

```text
STRUCTURE
    ↓
REPRESENTATION
    ↓
SIMILARITY
    ↓
PHYSICOCHEMICAL PROPERTIES
    ↓
DRUG-LIKENESS
    ↓
BIOLOGICAL ACTIVITY
    ↓
MACHINE LEARNING
```

> **The key lesson is that computational drug discovery is not about finding one property that makes a good drug. It is about combining multiple layers of molecular and biological information to understand — and eventually predict — how compounds behave.**

---

## 🛠️ Technologies Used

* **Python**
* **RDKit** — cheminformatics and molecular structure analysis
* **Pandas** — data manipulation
* **NumPy** — numerical computation
* **Matplotlib** — visualisation
* **Seaborn** — statistical visualisation
* **ChEMBL** — biological activity data
* **Jupyter Notebook** — analysis environment

---

## 📁 Repository Structure

```text
computational-drug-discovery/
│
├── data/
│   ├── drugs.csv
│   └── egfr_inhibitors.csv
│
├── images/
│   ├── morgan_fingerprints.png
│   ├── morgan_radius_comparison.png
│   ├── maccs_fingerprints.png
│   ├── physicochemical_descriptors.png
│   ├── egfr_ic50_distribution.png
│   └── egfr_ic50_log_distribution.png
│
├── notebooks/
│   └── computational_drug_discovery.ipynb
│
├── requirements.txt
└── README.md
```

---

## ▶️ Running the Project

### Clone the repository

```bash
git clone https://github.com/YOUR-USERNAME/computational-drug-discovery.git
cd computational-drug-discovery
```

### Install dependencies

```bash
pip install -r requirements.txt
```

### Launch Jupyter

```bash
jupyter notebook
```

Open the analysis notebook:

```text
notebooks/computational_drug_discovery.ipynb
```

---

**Latifa Al Khalifa**

Data Science & Business Analytics
