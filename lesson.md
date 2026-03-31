# 🎓 Lesson 1.6: Introduction to NumPy — Instructor Guide

## Session Overview

| Item | Detail |
|------|--------|
| **Duration** | 3 hours |
| **Format** | Flipped Classroom + Guided Coding in Jupyter |
| **Prerequisites** | Python basics (lists, loops, functions); Jupyter/Colab setup verified in pre-class |
| **Tools** | Google Colab (recommended) or VS Code + `pds` conda environment |
| **Notebook** | `notebooks/numpy_lesson.ipynb` |

### Agenda

| Time | Section | Focus |
|------|---------|-------|
| 0:00 – 0:10 | Welcome & Pre-Class Review | Setup check; quick concept quiz |
| 0:10 – 1:00 | Part 1: Arrays & Performance | ndarray creation, attributes, and benchmarking |
| 1:00 – 1:05 | Break | — |
| 1:05 – 1:55 | Part 2: Indexing & Broadcasting | Slicing, Boolean masking, element-wise ops |
| 1:55 – 2:00 | Break | — |
| 2:00 – 2:55 | Part 3: ufuncs & Linear Algebra | Statistical methods, matrix operations |
| 2:55 – 3:00 | Wrap-Up | Key Takeaways & Post-Class Assignment Briefing |

> **Instructor Note:** Have all students run the first notebook cell (import numpy and print version) before Part 1. This catches environment issues early.

---

## 🏃 Part 1: Arrays & Performance (50 min)

### 🎯 Learning Objective
Create NumPy arrays from Python sequences and inspect their key attributes (`shape`, `dtype`, `ndim`), and understand why NumPy is faster than Python lists.

### 📖 Theory Recap (10 min)

**Analogy:** A Python list is like a wallet with random cards of different sizes — flexible, but hard to process uniformly. A NumPy array is like a standardised 35mm film strip — every frame is the same size, so the projector (CPU) can blaze through it at full speed.

NumPy's speed advantage comes from:
- **Contiguous memory:** All values sit next to each other in RAM (no pointer chasing).
- **Fixed type:** Every element is the same dtype — the CPU uses optimised vectorised instructions (SIMD).
- **C-compiled:** NumPy operations run in compiled C under the hood, not interpreted Python.

Key array attributes:
| Attribute | What it tells you |
|-----------|------------------|
| `.shape` | Dimensions as a tuple, e.g. `(3, 4)` |
| `.dtype` | Data type of elements, e.g. `float64` |
| `.ndim` | Number of dimensions |
| `.size` | Total number of elements |

### 🛠️ Hands-On Activity: "The Performance Benchmark" (30 min)

**In the notebook:**

1. Benchmark Python list vs. NumPy array:
```python
import numpy as np

# Python list
python_list = list(range(1_000_000))
%timeit [x * 2 for x in python_list]

# NumPy array
numpy_array = np.arange(1_000_000)
%timeit numpy_array * 2
```

2. Create arrays and inspect attributes:
```python
arr_1d = np.array([1, 2, 3, 4, 5])
arr_2d = np.array([[1, 2, 3], [4, 5, 6]])

print(f"1D shape: {arr_1d.shape}, dtype: {arr_1d.dtype}")
print(f"2D shape: {arr_2d.shape}, ndim: {arr_2d.ndim}")
```

3. Array creation shortcuts:
```python
np.zeros((3, 4))         # All zeros
np.ones((2, 5))          # All ones
np.arange(0, 10, 2)      # Like range(), but returns array
np.linspace(0, 1, 5)     # 5 evenly spaced values from 0 to 1
```

**Discussion Questions:**
- "How much faster is NumPy than pure Python in your benchmark? What does this mean for data science at scale?"
- "What happens when you create `np.array([1, 2.0, 3])` — what dtype does Python infer?"

### 💬 Q&A & Reflection (10 min)

- **Common Misconception:** "NumPy is only useful for maths." → NumPy is the foundation of the entire scientific Python ecosystem — Pandas DataFrames are built on NumPy arrays; scikit-learn models use NumPy arrays for all inputs and outputs.
- **Business Case:** Tesla's self-driving team processes thousands of camera frames per second through NumPy-backed numerical pipelines before feeding them to neural networks. Without vectorised computation, real-time inference would be impossible.

---

## 🏃 Part 2: Indexing & Broadcasting (50 min)

### 🎯 Learning Objective
Apply indexing, slicing, and Boolean masking to extract and filter data, and understand how broadcasting handles arithmetic between arrays of different shapes.

### 📖 Theory Recap (10 min)

**Slicing:** Works like Python lists but extends to multiple dimensions.
```python
arr[row_start:row_end, col_start:col_end]
```

**Important:** NumPy slices return **views**, not copies. Modifying a slice modifies the original. Use `.copy()` to avoid this.

**Broadcasting Rule:** When operating on arrays of different shapes, NumPy stretches the smaller one to match — but only if the dimensions are compatible (trailing dimensions must be equal or one of them must be 1).

```
Shape (3, 4) + Shape (4,)  → Broadcasts to (3, 4)  ✅
Shape (3, 4) + Shape (3,)  → Error                  ❌
Shape (3, 4) + Shape (1, 4) → Broadcasts to (3, 4) ✅
```

### 🛠️ Hands-On Activity: "The Campaign Analyser" (30 min)

**In the notebook:**

```python
# Monthly ROI data: 4 campaigns × 6 months
roi_data = np.array([
    [0.12, 0.08, 0.15, 0.11, 0.09, 0.14],  # Campaign A
    [0.05, 0.22, 0.18, 0.07, 0.25, 0.19],  # Campaign B
    [0.30, 0.28, 0.31, 0.27, 0.29, 0.33],  # Campaign C
    [0.03, 0.04, 0.02, 0.05, 0.01, 0.06],  # Campaign D
])
```

1. Slice to get Campaign B's last 3 months: `roi_data[1, 3:]`
2. Boolean mask — campaigns with ROI > 0.20 in any month: `roi_data > 0.20`
3. Use the mask to extract high-performing values: `roi_data[roi_data > 0.20]`
4. Broadcasting — add a 0.02 bonus to all January ROIs (column 0):
```python
bonus = np.array([0.02, 0.02, 0.02, 0.02])  # shape (4,)
roi_data[:, 0] += bonus
```

**Discussion Questions:**
- "If you slice `row = roi_data[0]` and change `row[0] = 99`, what happens to `roi_data`?"
- "How would you select only the months where Campaign C *and* Campaign A both exceeded 0.10?"

### 💬 Q&A & Reflection (10 min)

- **Common Misconception:** "Slices give me a safe copy of the data." → They give you a *view*. This is a common source of bugs. Always use `.copy()` when you want independence.
- **Business Case:** Marketing analytics platforms use Boolean masking on large NumPy arrays to segment campaign performance in milliseconds — filtering millions of ad impressions by conversion threshold.

---

## 🏃 Part 3: ufuncs & Linear Algebra (50 min)

### 🎯 Learning Objective
Use NumPy universal functions (`ufuncs`) for element-wise operations and perform basic linear algebra including matrix multiplication.

### 📖 Theory Recap (10 min)

**ufuncs (Universal Functions)** are vectorised wrappers around C-level operations — they apply element-by-element across an entire array without a Python loop.

Common ufuncs: `np.sqrt()`, `np.exp()`, `np.log()`, `np.abs()`, `np.round()`

**Statistical methods:** `arr.mean()`, `arr.sum()`, `arr.std()`, `arr.min()`, `arr.max()`

**Axis parameter:**
- `axis=0` → operate down columns (across rows)
- `axis=1` → operate across rows (within each row)

**Matrix multiplication:**
- `arr1 * arr2` → element-wise (shapes must match)
- `arr1 @ arr2` or `np.dot(arr1, arr2)` → matrix multiplication (inner dimensions must match)

### 🛠️ Hands-On Activity: "The Scoring Model" (30 min)

**In the notebook:**

```python
# Student scores: 5 students × 3 subjects
scores = np.array([
    [85, 72, 90],
    [60, 88, 75],
    [92, 95, 88],
    [45, 60, 55],
    [78, 82, 80]
])

# Subject weights: [30%, 40%, 30%]
weights = np.array([0.30, 0.40, 0.30])
```

1. Compute each student's weighted final grade using matrix multiplication:
```python
final_grades = scores @ weights
print(final_grades)
```

2. Compute per-subject statistics:
```python
print("Class mean per subject:", scores.mean(axis=0))
print("Student total per student:", scores.sum(axis=1))
print("Std dev per subject:", scores.std(axis=0))
```

3. Normalise scores: subtract mean and divide by std (z-score):
```python
normalised = (scores - scores.mean(axis=0)) / scores.std(axis=0)
```

4. Reshape: convert 1D array of 12 values to 3×4 matrix and back:
```python
arr = np.arange(12)
matrix = arr.reshape(3, 4)
flat = matrix.flatten()
```

**Discussion Questions:**
- "What does `scores.mean(axis=0)` give you vs. `scores.mean(axis=1)`?"
- "Why use matrix multiplication instead of `scores * weights` for the weighted grade?"

### 💬 Q&A & Reflection (10 min)

- **Common Misconception:** "`arr1 * arr2` is the same as `arr1 @ arr2`." → Completely different. `*` is element-wise (same shape required). `@` is matrix multiplication (inner dimensions must match — `(m,n) @ (n,p) → (m,p)`).
- **Business Case:** Recommendation systems like Netflix's use matrix multiplication to compute the dot product of user preference vectors against item feature vectors — producing a relevance score for every movie simultaneously.

---

## 🎯 Wrap-Up (5 min)

### Key Takeaways
1. **NumPy arrays are the foundation** — Pandas, scikit-learn, and TensorFlow all build on NumPy's ndarray. Understanding arrays makes every higher-level tool easier.
2. **Slices are views, not copies** — use `.copy()` when you need independence from the source array.
3. **`@` for matrix multiplication, `*` for element-wise** — getting these confused causes silent wrong results, not error messages.

### Next Steps
- **Post-Class:** Complete the [post-class.md](./post-class.md) — 7 NumPy practice exercises covering arrays, masking, broadcasting, and linear algebra (45–60 min).
- **Next Lesson:** Lesson 1.7 introduces Pandas — a higher-level library built on NumPy that adds labelled indexing, mixed data types, and table-like operations.
