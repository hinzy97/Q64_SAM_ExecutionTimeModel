# Q64_SAM: Deterministic Execution Time Prediction for Spark Applications

This Jupyter Notebook implements the **Static Allocation Model (SAM)** – a graph-based deterministic model for predicting execution time of Spark applications. The model is based on the following peer-reviewed publication:

> **Tariq, H., Das, O. (2022).** A Deterministic Model to Predict Execution Time of Spark Applications.  
> In: *Computer Performance Engineering (EPEW 2022)*, LNCS, vol 13659. Springer.  
> https://doi.org/10.1007/978-3-031-25049-1_11


---

## 📘 Overview

The notebook simulates the execution of **TPC-DS Query 64** on a Spark cluster using a deterministic stage-wise simulation approach. It uses a flattened DAG of Spark stages and assigns tasks based on Spark’s round-robin task scheduling, simulating execution on a fixed number of cores.

- Models execution using **static resource allocation** (executor cores).
- Supports **parallel execution of independent stages**.
- Estimates stage-level execution using input-task duration relationships.
- Produces **accurate prediction** of total execution time for large input sizes.

---

## 🔍 Key Features

- **Graph-based simulation**: Represents the DAG of stages with explicit dependencies.
- **Task-level scheduling**: Simulates task execution using round-robin scheduling across cores.
- **Stage parallelism**: Enables parallel simulation of independent stages that do not share shuffle/output dependencies.
- **Real-world validation**: Predicted results closely match Spark execution on Google Cloud.
- **Input**: All stage/task parameters are defined within the notebook.

---

## 🔬 Methodology

- Flattens the Spark DAG to remove job-level hierarchy.
- Accepts stage metadata (number of tasks, task duration, dependencies) hardcoded for **20GB** and **100GB** inputs.
- Interpolates values for unseen input sizes (e.g., 200GB).
- Simulates task execution with 8 cores using a round-robin allocation policy.
- Aggregates per-stage completion time, considering parallel stages when dependencies allow.
- Accounts for warm-up overheads and shared-core contention during parallel execution.

---

## 🧪 Validation

The model has been validated using Spark history logs for Query 64 on a 200GB input size (Google Cloud Dataproc):

| Measured Time (s) | Predicted Time (s) | Error (%) |
|-------------------|--------------------|-----------|
| 491.71            | 477.71             | 2.85      |

---

## 📂 Files

- `Q64_SAM.ipynb` – Main Jupyter Notebook implementing the simulation and prediction model.

---

## 🚀 How to Run

1. Open `Q64_SAM.ipynb` using **Jupyter Notebook**, **JupyterLab**, or **Google Colab**.
2. Run each cell sequentially.
3. Inspect:
   - DAG structure and dependencies
   - Stage and task configuration
   - Predicted execution time

---

## 📝 Citation

If you use this repository or model in your work, please cite:

```bibtex
@InProceedings{10.1007/978-3-031-25049-1_11,
author="Tariq, Hina
and Das, Olivia",
editor="Gilly, Katja
and Thomas, Nigel",
title="A Deterministic Model to Predict Execution Time of Spark Applications",
booktitle="Computer  Performance Engineering",
year="2023",
publisher="Springer International Publishing",
address="Cham",
pages="167--181",
abstract="This work proposes a graph-based, deterministic analytical model that predicts the execution time of spark applications. It conceptualizes the structure of the spark application as a monolithic Directed Acyclic Graph (DAG) of stages capturing the precedence relationship among all the stages of the application. The model processes every stage of the DAG using a graph traversal algorithm, combined with a fixed scheduling policy of the spark platform in context (spark platform refers to the cloud that hosts the spark cluster). We validate our model against the measured execution time obtained by running a big data query (Query-64 of TPC-DS benchmark) that involves parallel execution of a large number of stages. The query is executed on the spark cluster of Google Cloud. Our model resulted in an execution time that is at 2.85{\%} error in comparison to the measured execution time.",
isbn="978-3-031-25049-1"
}
```

---

## 🔧 Future Work

- **Dynamic Executor Allocation**: Extend the model to support runtime changes in resource availability, reflecting Spark’s dynamic allocation mode.
- **Integration with Spark Logs**: Automate parameter extraction by parsing Spark UI logs (JSON/History Server).
- **Multiple Job Support**: Extend to handle multiple Spark jobs or job groups with independent DAGs.
- **ML-based Comparison**: Benchmark SAM’s predictions against data-driven models using regression, decision trees, or 
- **Visualization Enhancements**: Add DAG and Gantt chart-style visualizations for better understanding of parallelism and stage behavior.
