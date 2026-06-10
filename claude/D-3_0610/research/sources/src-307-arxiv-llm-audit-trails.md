# src-307 — Audit Trails for Accountability in Large Language Models

- **URL:** https://arxiv.org/html/2601.20727v1
- **Title:** Audit Trails for Accountability in Large Language Models
- **AccessDate:** 2026-06-10
- **Related section:** Why (governance/audit argument), When (regulated/high-stakes decisions)

---

## Key Excerpts

### Abstract — Core Problem Statement

> "Large language models (LLMs) are increasingly embedded in consequential decisions across healthcare, finance, employment, and public services. Yet **accountability remains fragile because process transparency is rarely recorded in a durable and reviewable form**. We propose LLM audit trails as a sociotechnical mechanism for continuous accountability. An audit trail is a chronological, tamper-evident, context-rich ledger of lifecycle events and decisions that links technical provenance (models, data, training and evaluation runs, deployments, monitoring) with governance records (approvals, waivers, and attestations), so organizations can reconstruct what changed, when, and who authorized it."

### The Accountability Gap

> "When an error or harm occurs, organizations often **cannot reconstruct which version was live, which data influenced it, who approved its release, or why particular changes were made.** External audits and investigative reporting continue to surface harms, yet the internal records needed to trace decisions are often fragmented across tools, stored in ad hoc ways, or never captured in the first place. The result is a persistent accountability gap."

### Financial Chatbot Scenario (Motivating Case)

A retail bank deploys BankGPT for mortgage guidance. Months later a customer complaint triggers review.

> "To investigate, the bank needs records that show which fine-tuned model version was serving in the relevant channel at the time of the interaction, which prompt templates and decoding settings were active, and how rollout controls were configured. In many current LLM deployments, such information is **scattered across experiment trackers, CI logs, configuration files, and email threads**, making it difficult to reconstruct a coherent timeline for internal review or external scrutiny."

### Clinical Documentation Scenario (Motivating Case)

An LLM drafting assistant (NoteAssist) is deployed in a hospital. A cluster of follow-up gaps is discovered.

> "The health system needs evidence about which version of NoteAssist was active during the affected encounters, **which prompt templates and configuration settings were used** for generating follow-up plans, and how those settings changed over time."

### Traceability as Accountability Mechanism

> "Legal and policy work has argued that traceability — the ability to reconstruct which artifacts, configurations, and decisions produced a system's behavior at a given time by linking outcomes back to design choices, data use, and responsible parties — is a key mechanism for operationalizing accountability."

### Alignment with EU AI Act

> "Audit trails... align with regulatory record-keeping (EU AI Act, Article 12) and risk management guidance (NIST AI RMF 1.0)."

---

*Authors: Brown University, Dept. of Computer Science. Published Jan 2026. arXiv:2601.20727v1*
