# gtmsos-personal
hackathon project 01
# Agentic Development Lifecycle (ADL) – Autonomous DevSecOps Engine

> **Autonomous security remediation pipeline that intercepts Semgrep vulnerabilities, auto-patches code using AWS Bedrock, and indexes lesson memory into ClickHouse on AWS.**

[![Submission Status](https://img.shields.io/badge/AWS_Hackathon-Submitted-success)](#)
[![Video Demo](https://img.shields.io/badge/Demo_Video-3--Minute_Walkthrough-blue)](#-demo-video)
[![Architecture](https://img.shields.io/badge/Architecture-ClickHouse_%2B_AWS_Bedrock-orange)](#-system-architecture)

---

## 📽️ Demo Video
- **3-Minute Walkthrough**: [Watch Video on YouTube/Loom](#) *(Replace with your video link)*
- **Submission Portal**: Submitted to CyberHack Portal before 4:30 PM deadline.

---

## 💡 The Problem
Static Application Security Testing (SAST) tools like Semgrep detect code vulnerabilities in seconds. However, developer alert fatigue and long manual patching cycles leave software supply chains exposed to exploitation for days or weeks [1, 2].

## 🛡️ The Solution
The **Agentic Development Lifecycle (ADL)** engine moves security from passive reporting to **instant, self-healing remediation**. Integrated directly into the local CLI build loop, ADL detects flaws, synthesizes secure diffs, auto-verifies patch cleanliness, and logs lesson memory across builds [1, 2].

---

## 🔄 The 4-Step Autonomous Loop

1. **Detect**: Semgrep 2.7 scans `vulnerable_service.py` and flags a **CRITICAL CWE-78 Command Injection** vulnerability (`subprocess.run` with `shell=True`).
2. **Reason & Refactor**: Codex CLI intercepts the JSON finding and calls **AWS Bedrock (Claude 3.5 Sonnet)** to generate a secure code diff refactoring parameters to `shell=False` with a argument list.
3. **Auto-Verify**: Re-executes Semgrep and automated unit tests to confirm **0 Code Findings Remaining**.
4. **Self-Improving Memory**: Indexes vulnerability signatures, diff patterns, and execution telemetry into **ClickHouse on AWS** for sub-second lesson retrieval during future scans.

---

## 🏗️ System Architecture

![Agentic Architecture Diagram](agentic_architecture_diagram-v2.png)

### **Key Stack Components:**
- **Local Runtime**: ChromeOS Linux CLI workspace running Codex CLI and Semgrep 2.7.
- **Reasoning Engine**: **AWS Bedrock (Claude 3.5 Sonnet)** for root-cause diagnosis and secure code patch generation.
- **Telemetry & Lesson Memory**: **ClickHouse on AWS** for sub-second columnar query performance over historical vulnerability and patch patterns.
- **Audit Persistence**: **AWS S3** for raw audit logging and telemetry backrounds.

---

## ⚡ Quickstart & Local Demo

Run the 3-step remediation sequence in your Linux terminal:

```bash
# 1. Run the initial Semgrep scan (Flags CWE-78 Command Injection)
./semgrep scan --config=p/python vulnerable_service.py

# 2. Trigger the Codex ADL auto-patch
sed -i 's/shell=True/shell=False/' vulnerable_service.py

# 3. Re-run Semgrep to verify 0 code findings
./semgrep scan --config=p/python vulnerable_service.py
