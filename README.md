# Meltdown and Spectre — Experiment

COSC 6385 Computer Architecture — Assignment 2 (Experiment).
Reproduces the published proof-of-concept (PoC) code for the 2018
transient-execution vulnerabilities **Meltdown** (CVE-2017-5754) and **Spectre**
(CVE-2017-5753 / CVE-2017-5715) on Linux, and documents setup, results and
mitigations.

> These PoCs demonstrate *published, patched* CPU vulnerabilities for a course
> experiment. Disabling CPU mitigations (`mitigations=off`) is only for an
> isolated lab VM — never on a machine that handles real data.

## Result (this run)
Tested on an **Intel Xeon Bronze 3204 (Cascade Lake)**, Ubuntu 22.04, kernel
5.15.0-186, with mitigations left **ON** (shared server, no root/reboot).

- **Spectre v1 — reproduced.** Kocher's `spectre.c`, compiled and run, recovered
  the in-process secret strings byte by byte via Flush+Reload, *even with the
  kernel Spectre v1 mitigation enabled* — because those defences guard the
  kernel boundary, not a process leaking its own memory.
- **Meltdown — not reproducible here.** The CPU carries the in-silicon fix and
  the kernel reports `meltdown: Not affected`; a pre-2018 Intel CPU would be
  needed. This negative result demonstrates the hardware fix.

## What is here
- `Experiment-Meltdown-Spectre-report.pdf` / `.docx` — the report: how both
  attacks work, the environment, results, and software + hardware mitigations.
- `results/vulnerabilities.txt` — the kernel's `/sys/.../vulnerabilities` status.
- `results/spectre_output.txt` — the Spectre v1 PoC output recovering the secret.

## Reproducing (what I ran — no root, no reboot)
```sh
# 1. kernel vulnerability status
for f in /sys/devices/system/cpu/vulnerabilities/*; do echo "$(basename $f): $(cat $f)"; done
# 2. Spectre v1 PoC (build from source, do not trust the prebuilt binary)
git clone https://github.com/flxwu/spectre-attack-demo.git
gcc -O0 -std=c11 spectre-attack-demo/spectre.c -o spectre_built
./spectre_built            # prints the recovered secret (value[0] column)
```
Meltdown additionally needs a **pre-2018 Intel CPU** and the
[IAIK/meltdown](https://github.com/IAIK/meltdown) PoC (`make && ./test`), which
was not available here. The Spectre PoC only reads its own process memory, so it
is self-contained; never disable CPU mitigations on a shared or production host.

## Submission
Per the assignment, the report is submitted to **GitHub** (this repository).
