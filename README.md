# Burmese-KYC Sentinel MCP

**AI-Powered Entity Resolution & Multi-Document Verification Agent for Myanmar User Onboarding**

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![MCP Protocol](https://img.shields.io/badge/MCP-FastMCP%20%7C%20SDK-green.svg)](https://gofastmcp.com/)
[![Evaluation](https://img.shields.io/badge/Evals-Pytest%20%7C%20Adversarial-orange.svg)](#evaluation--latency-benchmarks)
[![Privacy](https://img.shields.io/badge/Privacy-100%25%20Synthetic%20Data-purple.svg)](#privacy--data-governance)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

`burmese-kyc-sentinel-mcp` is an applied data science and agentic AI system engineered to reduce **high false-rejection rates in Myanmar KYC onboarding** while maintaining strict Enhanced Due Diligence (EDD) compliance.

Inspired by real-world onboarding bottlenecks discussed at **Innovate Chiang Mai 2026**, this repository pairs a **Model Context Protocol (MCP)** verification server with **Burmese phonetic & semantic entity resolution**, **Self-Consistency ($N=3$) test-time scaling**, **adversarial prompt-injection guardrails**, and **vLLM serving benchmarks**.

---

## 🎯 Problem Statement

Global digital-asset exchanges face two compounding challenges when verifying genuine customers from Myanmar:

1. **Non-Western Naming Conventions & Romanization Drift:**
   * Myanmar names have **no fixed surname** and routinely change honorific prefixes across official documents (e.g., `Mg Aung Aung` on an NRC card, `U Aung Aung` on a bank statement, `AUNG AUNG` on a passport, or ethnic prefixes such as `Saw`, `Naw`, `Sai`, `Nang`, `Khun`, `Min`, `Mi`).
   * Inconsistent Burmese-to-English transliteration (e.g., *Nyan Linn Htun* vs. *Nyan Lin Tun*, *Kyaw* vs. *Chaw*, *Thiha* vs. *Thi Ha*) causes legacy exact-match KYC pipelines to falsely reject legitimate users.
2. **Multi-Document Enhanced Due Diligence (EDD) Friction:**
   * AML/EDD compliance requires cross-checking multiple documents (National Registration Card, Passport, Household List, Proof of Address). Manual review creates severe onboarding delays, while rigid rule engines fail on minor formatting discrepancies.

**Our Approach:** Decompose KYC verification into deterministic **MCP tools** (honorific normalization, phonetic/embedding matching, NRC township code validation, cross-document anchor matching) orchestrated by a **Self-Consistency ($N=3$) reasoning agent** hardened against OCR prompt injection.

---

## 🏗️ System Architecture

```mermaid
flowchart TD
    Bundle["Submitted KYC Document Bundle\n(Synthetic NRC, Passport, Bank Statement)"] --> Agent["KYC Reasoning Agent\n(Chain-of-Thought + Self-Consistency N=3)"]

    subgraph Guardrails["Input & Output Guardrails"]
        Agent <--> Guard["XML Data Isolation & Ground-Truth Verifier"]
    end

    Agent <-->|Model Context Protocol| Server["FastMCP Verification Server"]

    subgraph Tools["Deterministic & ML Verification Tools"]
        Server --> NameTool["resolve_burmese_entity_names\n(Honorific Strip + Phonetic + Embeddings)"]
        Server --> NRCTool["validate_myanmar_nrc\n(State/Township Registry + Checksum)"]
        Server --> AnchorTool["cross_verify_document_anchors\n(DOB, Father's Name, Address Consistency)"]
        Server --> GuideTool["generate_burmese_remediation_note\n(Localized Actionable User Guidance)"]
    end

    Agent --> Output["Final Disposition + Confidence Score +\nBurmese User Feedback + Latency Telemetry"]
