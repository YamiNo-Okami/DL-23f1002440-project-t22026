# 💡SMART MCQ SOLVER CHALLENGE 

---

## Overview
This project focuses on building an intelligent answer-ranking pipeline capable of:
- Understanding question semantics
- Evaluating answer choices
- Ranking possible answers
- Generating Top-3 predictions
- Supporting deployment as an API or web application **(Optional)**


The project follows a structured workflow :
```
EDA
 ↓
Traditional ML Models
 ↓
Deep Learning Models
 ↓
Hybrid Ensemble
 ↓
Deployment
``` 


---

## Goal 

The goal of this competition is to **build intelligent models that can accurately predict the top three most probable answers for challenging multiple choice questions**. Participants are encouraged to develop efficient AI and machine learning solutions capable of strong reasoning and answer ranking.

---

## Project Structure 

```
mcq-ranking-project/
│
├── data/
│   ├── raw/
│   ├── processed/
│   └── submissions/
│
├── notebooks/
│
├── src/
│   ├── preprocessing/
│   ├── features/
│   ├── models/
│   ├── training/
│   ├── inference/
│   └── utils/
│
├── app/
├── configs/
├── outputs/
├── tests/
│
├── requirements.txt
├── README.md
└── main.py
```

---
## Dataset Description

The dataset contains multiple-choice reasoning questions.
Each sample includes : 
| Column | Description                       |
| ------ | --------------------------------- |
| id     | Unique question identifier        |
| prompt | Question/problem statement        |
| A      | Option A                          |
| B      | Option B                          |
| C      | Option C                          |
| D      | Option D                          |
| E      | Option E                          |
| answer | Correct answer label (train only) |

The test dataset does not contain the ```answer``` column.

The goal is to **predict the Top-3 most likely answers**.

---
## Problem Type 

This project combines.

### Multi-Class Classification
Predict the correct answer among :
``` A, B, C, D, E  ```

and 

### Ranking
Generate the Top-3 ranked predictions

---

## Evaluation Metrics 

The system will be evaluated using:
| Metric         | Purpose                |
| -------------- | ---------------------- |
| Accuracy       | Correct classification |
| Top-3 Accuracy | Ranking performance    |
| MAP@3          | Competition metric     |

### Mean Average Precision at $K$ (```mAP@K```)

```mAP@K``` is an error metric that can be used when the **sequence** or **ranking** of your recommended items **plays an important role or is the objective of your task**.  
By using it, we can get answers to the following questions:
- Are my generated or predicted recommendations **relevant**?
- Are the **most relevant** recommendations on the **first ranks**?

### Steps to find ```mAP@K``` in our case ```mAP@3``` :
#### 1. Find Precision : 
**Precision** : Precision can be defined as the fraction of **relevant items** in **all recommended items (relevant + irrelevant items)**.
    $$
    \text{Precision} = \frac{\sum \space \text{Relevent Items}}{\sum \space \text{All Items}}
    $$

#### 2. Find Precision at $K$ (```P@K```) :
The precision metric  itself does not consider the rank or order in which the relevant items appear. Time to include the ranks to our precision formula. Precision<b>@K</b> can be defined as **the fraction of relevant items in the top K recommended items**.
$$
\text{P@K} = \frac{ \sum \space \text{Relevent items in top K recommendations }}{ \sum \space \text{Items in top K recommendations }}
$$

#### 3. Find Average Precision@$K$ (```AP@K```)  :
The Average Precision@$K$ or ```AP@K``` is the sum of precision@$K$ where the item at the k<sub>th</sub> rank is **relevant** (```rel(k)```) divided by the **total number of relevant items** (r) in the top K recommendations
$$
\text{AP@K} = \frac{1}{r} \sum_{k=1}^{K}{\text{Precision@}k \cdot  rel(k)}
$$

$$
rel(k)=
\begin{cases}
1, & \text{if item at } k^{\text{th}} \text{ rank is relevant} \\
0, & \text{otherwise}
\end{cases}
$$

#### 4. Find Mean Average Precision@K (```MAP@K```) : 
It averages the AP@K for recommendations for N multiple-choice questions.
$$
\text{MAP@K} = \frac{1}{N} \sum_{j=1}^{N} \frac{1}{r}\sum_{k=1}^{K}\text{Precision@}k\cdot rel(k)
$$