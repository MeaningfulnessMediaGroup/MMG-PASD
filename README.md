<div align="center">

# Portable AI Support Documentation (PASD)
### A Standardized, Self-Contained Documentation Framework for Software Support

[![Specification](https://img.shields.io/badge/Specification-MMG--PASD--1.0-blue.svg)](MMG-PASD-1.0.pdf)
[![Status](https://img.shields.io/badge/Status-Public_Draft-orange.svg)](#architecture-the-3-layers-of-pasd)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![DOI](https://img.shields.io/badge/DOI-10.5281%2Fzenodo.19163276-blue.svg)](https://doi.org/10.5281/zenodo.19163276)
[![Reference Implementation](https://img.shields.io/badge/Reference_App-MyFileExplorer-success.svg)](https://github.com/dobrado76/MyFileExplorer)

<p align="center">
  <b>The developer provides the verified knowledge. The user provides the AI assistant.</b><br>
  Zero hosted chatbots. Zero API costs. Zero hallucinated settings.
</p>

</div>

---

## ⚡ The Problem: The Support Bottleneck in the AI Era

When users encounter an error or configuration question in your software, they rarely search documentation wikis anymore. They open ChatGPT, Claude, or a local model and ask:

> *"How do I enable cloud sync in App X?"*  
> *"Why did my export fail with error code 12?"*

Because standard documentation only describes what software **can** do, the LLM encounters an informational void. Due to statistical priors on similar applications and instruction-tuning sycophancy, the assistant **hallucinates plausible answers**: it invents nonexistent menu paths, fake configuration flags, and dangerous reset commands.

Meanwhile, hosting a commercial RAG support bot on your docs site costs $50–$300/month, requires infrastructure maintenance, and leaks user logs to external cloud APIs.

---

## 💡 The Solution: Bring Your Own AI (BYOAI)

**PASD** formalizes a lightweight alternative: shipping a single, version-bounded, self-contained Markdown file—canonically named **`AI-SUPPORT.md`**—directly alongside your software (in your repository root or inside your release installer).

```text
               TRADITIONAL SUPPORT                     PASD (BYOAI) SUPPORT
               
     [User encounters error / question]        [User encounters error / question]
                     │                                         │
                     ▼                                         ▼
         Manual doc / forum search             Clicks in-app "Copy AI Help Link"
                     │                          (or downloads local AI-SUPPORT.md)
                     ▼                                         │
        Confused by technical terms                             ▼
                     │                         Pastes into ChatGPT / Claude / Ollama
                     ▼                                         │
       Funnels to Maintainer Issue Tracker                      ▼
           (Recurring support burden)          AI reads verified state + negative space
                                               ──► Delivers grounded diagnostic or
                                                   calibrated abstention
```

---

## 🛡️ The Core Moat: The Three-State Epistemic Model

Traditional manuals only document verified features. But preventing AI hallucination requires explicitly mapping the **Negative Space** of your application:

| Epistemic State | Condition in `AI-SUPPORT.md` | Model Behavior Required by PASD |
| :--- | :--- | :--- |
| **1. Supported** | Explicitly documented as implemented in version $V$. | Explains exact verified UI controls, flags, and diagnostic steps. |
| **2. Explicitly Unsupported** | Declared in the **Negative Space** section (anti-features, roadmap-only, or third-party responsibilities). | Clearly states the feature **does not exist**; blocks sycophantic hallucinations. |
| **3. Unknown / Undocumented** | Neither documented nor declared unsupported; or diagnostic evidence $E$ is missing. | **Calibrated Abstention**: States the document does not establish the answer and requests clarifying logs instead of guessing. |

---

## 📁 Repository Artifacts

This repository houses the formal specification and the open-source generation toolchain:

| Artifact | Description | Location |
| :--- | :--- | :--- |
| **White Paper / Formal Specification** | Complete 26-page academic specification (`MMG-PASD-1.0`). | [`MMG-PASD-1.0.pdf`](MMG-PASD-1.0-Portable_AI_Support_Documentation.pdf) |
| **Knowledge Extraction Specification** | 14-section prompt instructing coding agents on how to audit code and extract a support manual. | [`AI-SUPPORT-documentation-generation-prompt.md`](AI-SUPPORT-documentation-generation-prompt.md) |
| **Sequential Agent Runbook** | 5-step prompt script for Cursor, Claude Code, or Aider to prevent single-turn context truncation. | [`AI-SUPPORT Generation and Publishing Workflow.txt`](AI-SUPPORT%20Generation%20and%20Publishing%20Workflow.txt) |
| **Live Reference Implementation** | Production deployment in a real-world Windows desktop file workbench. | [*MyFileExplorer* Repository](https://github.com/dobrado76/MyFileExplorer) <br> [`AI-SUPPORT.md` (Raw Payload)](https://raw.githubusercontent.com/dobrado76/MyFileExplorer/refs/heads/main/AI-SUPPORT.md) |

---

## 🚀 Quickstart: Add `AI-SUPPORT.md` to Your Project in 5 Minutes

You can generate and maintain a fully conformant support system using your existing AI coding assistant (Cursor Composer, Claude Code, Aider, or Copilot):

### Step 1: Stage the Extraction Specification
Download [`AI-SUPPORT-documentation-generation-prompt.md`](AI-SUPPORT-documentation-generation-prompt.md) and place it into your project's `docs/` directory.

### Step 2: Execute the 5-Step Runbook
Open [`AI-SUPPORT Generation and Publishing Workflow.txt`](AI-SUPPORT%20Generation%20and%20Publishing%20Workflow.txt) and paste the prompts sequentially into your coding assistant:

1. **Prompt 1 (Inspect & Generate):** The agent reads source code, entry points, schemas, and tests to draft modular docs in `docs/` and compiles the root `AI-SUPPORT.md`.
2. **Prompt 2 (Consistency Review):** The agent cross-checks every documented UI label, CLI flag, and default against the actual codebase and verifies negative-space boundaries.
3. **Prompt 3 (Cleanup):** The agent deletes the temporary prompt from `docs/` and verifies that nothing links to it.
4. **Prompt 4 (Publish):** Commit the updated documentation alongside your code.

### Step 3: Make It Discoverable in Your `README.md`
Add a visible section near the top of your repository's root `README.md`:

```markdown
## Need Help? Ask an AI Assistant

You can get instant, grounded technical support by providing our verified support manual to your preferred AI assistant:

1. **Copy our Raw Support Link:**  
   `https://raw.githubusercontent.com/<user>/<repo>/refs/heads/main/AI-SUPPORT.md`
2. **Paste it into ChatGPT, Claude, or Gemini** alongside your question or error log:  
   > *"Read this documentation and help me fix this issue: [describe problem]"*
3. **Using Local / Offline AI (LM Studio, Ollama)?**  
   Drag and drop the local `AI-SUPPORT.md` file (included in the root directory) directly into your chat session.
```

---

## 🏗️ Architecture: The 3 Layers of PASD

```text
┌────────────────────────────────────────────────────────────────────────┐
│               LAYER A: Normative Document Specification                │
│  • 10-Point Conformance Matrix (PASD-001 to PASD-010)                  │
│  • Three-State Epistemic Model (Supported / Unsupported / Unknown)     │
│  • Two-Tiered Topology (Tier-1 Triage Header / Tier-2 Deep Reference)  │
│  • Symptom-Oriented Diagnostic Trees & Data-Safety Recovery Warnings   │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │ implemented via
┌───────────────────────────────────▼────────────────────────────────────┐
│         LAYER B: Recommended Generation & Maintenance Lifecycle        │
│  • 5-Stage Staged Generation & Consistency Review Runbook              │
│  • Deterministic Bundler & CI Freshness Checks                         │
│  • Raw Payload Tag-Pinning & Distribution Protocol                     │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │ verified by
┌───────────────────────────────────▼────────────────────────────────────┐
│            LAYER C: Empirical Support Evaluation Protocol              │
│  • Independent Test Harness (docs/support-evaluation.md; N ≥ 40 cases) │
│  • 4-Condition Within-Subjects Benchmark (A: Raw, B: Docs, C: Content, │
│    D: Full PASD)                                                       │
│  • Metrics: SFA (Accuracy), NBP (Negative Precision), CAR (Abstention),│
│    URR (Safety), and DRR (Downstream Resolution)                       │
└────────────────────────────────────────────────────────────────────────┘
```

---

## ⚖️ How PASD Compares to Existing Standards

| Capability | Conventional Docs | `llms.txt` | `AGENTS.md` | AIRD (v1.2) | Hosted RAG Bots | **PASD (`AI-SUPPORT.md`)** |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Primary Audience** | Humans | Web Crawlers | Coding Agents | Agents/Devs | End Users | **End Users via BYOAI** |
| **Zero Infrastructure Cost** | ✅ | ✅ | ✅ | ✅ | ❌ ($50–$300/mo) | ✅ **100% Free / Serverless** |
| **Offline / Air-Gapped Ready** | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ **Drag & Drop Local** |
| **Mandatory Negative Space** | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ **Prevents Hallucinations** |
| **Third-Party Boundary Tables** | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ **Isolates Dependency Faults** |
| **Negative-Case Falsification** | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ **Layer C $N \ge 40$ Suite** |

---

## 🔒 Security & Privacy

* **Indirect Prompt Injection Defense:** PASD prohibits support files from claiming system-level authority, attempting to override model safety policies, or directing autonomous tool actions outside answering support questions.
* **Documentation Poisoning Guardrails:** Prescribes automated CI linting for high-risk shell patterns (`curl | bash`), protected release compilation pipelines, and `CODEOWNERS` policies for troubleshooting files.
* **Data Privacy:** Users control their own data telemetry by choosing their model provider or running local, private models (LM Studio / Ollama). Redaction rules enforce stripping credentials, API keys, and personal paths before escalation.

---

## 📖 Citation

If you use or build upon the PASD specification or generation prompts in your research or software projects, please cite:

```bibtex
@techreport{bee2026pasd,
  author      = {Bee, Djeff},
  title       = {Portable {AI}-Assisted Software Support: A Proposal for a Standardized, Self-Contained Documentation Framework},
  institution = {Meaningfulness Media Group},
  number      = {MMG-PASD-1.0},
  year        = {2026},
  month       = {October},
  doi         = {10.5281/zenodo.19163276},
  url         = {https://doi.org/10.5281/zenodo.19163276}
}
```

---

## 📄 License

The PASD specification document, extraction prompts, and workflow runbooks are licensed under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) license.

Copyright © 2026 Meaningfulness Media Group.
