# 📚 Lesson 1.6: Introduction to NumPy

**Theme:** Speed and Scale — working with numerical data using arrays

---

## 📅 Lesson Overview

| Section | Duration | Topic / Activity |
|---------|----------|-----------------|
| **Part 1: Arrays & Performance** | 55 min | ndarray creation; shape/dtype; performance vs. Python lists |
| **Part 2: Indexing & Broadcasting** | 55 min | Slicing; Boolean masking; element-wise operations |
| **Part 3: ufuncs & Linear Algebra** | 55 min | Statistical methods; matrix multiplication; reshaping |

---

## 🎯 Learning Outcomes

By the end of this lesson, you will be able to:

1. **Create** NumPy arrays from Python sequences and inspect key attributes (`shape`, `dtype`, `ndim`).
2. **Apply** indexing, slicing, and Boolean masking to filter and extract data from multi-dimensional arrays.
3. **Use** broadcasting to perform efficient element-wise arithmetic between arrays of different shapes.
4. **Execute** statistical aggregations (`mean`, `sum`, `std`) and basic linear algebra operations using NumPy.

---

## 📂 Course Materials

| Material | Description | Est. Time |
|----------|-------------|-----------|
| [Pre-Class](./pre-class.md) | Jupyter/Colab setup; what NumPy is and why it matters | 30–45 min |
| [Lesson Plan](./lesson.md) | Instructor guide for the 3-hour coding session | 3 hours |
| [Post-Class](./post-class.md) | 7 NumPy practice exercises — arrays, filters, and matrix ops | 45–60 min |
| [Reference](./reference.md) | NumPy cheat sheet, common functions, and axis reference | As needed |

---

## 🛠️ Tools & Setup

- **[Google Colab](https://colab.research.google.com)** *(recommended)*: No installation, runs in-browser.
- **[VS Code](https://code.visualstudio.com)** + Python + Jupyter extensions *(alternative)*.
- **Notebook:** `notebooks/numpy_lesson.ipynb` — open in Colab or VS Code with the `pds` kernel.
- **Environment:** `conda env create -f environment.yml` then `conda activate pds` (VS Code users only).
