# Meltdown and Spectre — Experiment

COSC 6385 Computer Architecture — Assignment 2 (Experiment).
Reproduces the published proof-of-concept (PoC) code for the 2018
transient-execution vulnerabilities **Meltdown** (CVE-2017-5754) and **Spectre**
(CVE-2017-5753 / CVE-2017-5715) on Linux, and documents setup, results and
mitigations.

> These PoCs demonstrate *published, patched* CPU vulnerabilities for a course
> experiment. Disabling CPU mitigations (`mitigations=off`) is only for an
> isolated lab VM — never on a machine that handles real data.

## What is here
- `Experiment-Meltdown-Spectre-report.pdf` / `.docx` — the report: how both
  attacks work, the experiment environment, results, and software + hardware
  mitigations.
- `results/` — screenshots of the vulnerability-check script and the PoC runs.
- (add the two PoC repositories from the assignment as git submodules, and the
  `spectre-meltdown-checker` script — see below.)

## Reproducing
1. On a **pre-2018 Intel machine** (newer CPUs such as Cascade Lake have the
   Meltdown fix in silicon and will report *Not affected*), set up a Linux VM.
2. Boot an older kernel with mitigations disabled, e.g. Ubuntu 16.04 / 20.04 with
   `mitigations=off` (older kernels also need `nopti nospectre_v2`).
3. Run `spectre-meltdown-checker.sh` to record which CVEs each system reports.
4. Build and run each PoC with `make`; capture the secret-recovery output.
5. Save the screenshots to `results/`.

## Submission
Per the assignment, the report is submitted to **GitHub** (this repository).
