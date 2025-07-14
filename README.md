# Q64_SAM: Deterministic Execution Time Prediction for Spark Applications

This Jupyter notebook implements the **graph-based deterministic analytical model** described in the paper:

**"A Deterministic Model to Predict Execution Time of Spark Applications"**  
*Hina Tariq and Olivia Das, EPEW 2022, Springer LNCS.*

## 📘 Overview

The notebook predicts the execution time of **Query-64 (TPC-DS benchmark)** on a Spark cluster using a deterministic simulation approach. The model simulates a flat DAG of stages, assigns tasks based on Spark's round-robin scheduling, and accounts for execution time per task and parallelism due to multiple executor cores.

### Key Features:
- Represents the DAG of Query-64 stages with dependency and task execution parameters.
- Simulates task execution on executor cores (8 cores used).
- Produces a predicted total execution time.
- Validated with real Spark history logs (Google Cloud).

## 🔬 Methodology

The model:
1. Flattens the Spark DAG to remove job-level hierarchy.
2. Uses input parameters from two reference input sizes (20GB, 100GB).
3. Estimates stage-level parameters for a target input size (e.g., 200GB).
4. Schedules tasks using round-robin policy over executor cores.
5. Predicts execution time including warm-up overhead.

## 🧪 Validation

The model achieves **2.85% error** in execution time prediction compared to measured runtime on Google Cloud for 200GB input.

| Measured Time (s) | Predicted Time (s) | Error (%) |
|-------------------|---------------------|-----------|
| 491.71            | 477.71              | 2.85      |

## 📂 Files

- `Q64_SAM.ipynb` – Main notebook implementing the model and predicting execution time.
- `paper/` – Contains the original publication in PDF format.

## 🚀 How to Run

1. Open the notebook in **JupyterLab**, **Jupyter Notebook**, or **Google Colab**.
2. Run each cell sequentially.
3. Inspect the DAG, input parameters, and final predicted execution time.

## 📝 Citation

If you use this repository, please cite:

```bibtex
@inproceedings{tariq2022deterministic,
  title={A Deterministic Model to Predict Execution Time of Spark Applications},
  author={Tariq, Hina and Das, Olivia},
  booktitle={European Performance Engineering Workshop (EPEW)},
  pages={167--181},
  year={2023},
  publisher={Springer}
}
```

## 🔧 Future Work

- Support for dynamic executor allocation.
- Integration with Spark UI history logs directly.
- Comparison with ML-based predictive models.
