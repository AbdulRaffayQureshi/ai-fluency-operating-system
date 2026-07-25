# Framed Cases

**Voice:** Direct, rigorous, plain-spoken, no hype, evidence-first.

---

## Case Study 1: Quantum-Classical Pipeline for BACE1 Inhibitors

**Problem**
A collaborator in medicinal chemistry was screening BACE1 inhibitors using AutoDock Vina alone. The classical scores ranked a known false positive in the top 10 every time. They needed a second opinion that captured electronic correlation effects — something classical force fields miss — but they didn't have 200 hours to learn quantum chemistry.

**What I Did (and Decided)**
I built a hybrid pipeline. Classical docking handled the initial pose generation (fast, proven). Then I fed the top 20 poses into a variational quantum eigensolver (VQE) running on Qiskit's Aer simulator to estimate ground-state binding energy. I made two specific decisions: (1) I pinned every dependency in a Docker image with a locked `requirements.txt` so the quantum circuit parameters reproduced byte-for-byte on rerun, and (2) I wrote a data contract in Pandera that killed the job if the input SDF file was missing explicit hydrogen flags — the exact reason their last pipeline silently returned garbage for three days.

**What Came of It**
The VQE pipeline demoted the known false positive from rank 3 to rank 18. Two compounds that VQE pushed into the top 5 had been buried at rank 34 and 41 by classical scoring alone. The pipeline runs end-to-end in 3.5 hours on a laptop. Docker image and locked dependencies are in the repo; you can rerun it and get the same ranking.

---

## Case Study 2: Flyrank 8-Week ML Internship — Churn Prediction API

**Problem**
The client had a CSV of customer behavior and a Jupyter notebook that "predicted churn" but crashed when the production database added a new column. They couldn't ship it. Their engineer was hand-editing the CSV every Monday morning to keep the notebook alive.

**What I Did (and Decided)**
I replaced the notebook with a FastAPI service. The critical decision was the data contract: I used Pandera to enforce a strict schema at the API boundary. If the upstream database sends a column rename, a type change, or a null in a non-nullable field, the API returns a 422 error immediately with the exact mismatch — no silent failure, no Monday morning debugging. I also containerized it with Docker and added a GitHub Action that runs the test suite + schema validation on every push.

**What Came of It**
The hand-editing stopped. The API has been running in production for 6 weeks. The client added two new features to the database last month; the contract caught both schema changes within seconds, and the engineer knew exactly what to fix before any bad data hit the model.

---

## Bio Copy

I build reproducible ML pipelines that turn raw biological data into production-ready predictions. My work sits at the intersection of quantum chemistry and classical ML — but the part I care about most is that the pipeline doesn't break at 2 AM when the input schema changes.

## Contact / CTA Copy

Book a 20-minute call. I'll tell you if your dataset is pipeline-ready or if your current setup is one column rename away from silent failure.