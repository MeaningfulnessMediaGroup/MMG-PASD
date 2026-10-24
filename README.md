# Portable AI Support Documentation (PASD)

**A vendor-neutral, self-contained documentation specification for AI-assisted software support.**

**Specification:** `MMG-PASD-1.0` · **Status:** Proposed Specification — Public Draft · **Publisher:** Meaningfulness Media Group (MMG)

> **The developer supplies verified knowledge. The user chooses the AI.**

Software users should be able to ask their preferred AI assistant a question about an application and receive an accurate, version-specific answer—without relying on a developer-operated chatbot or asking the maintainer the same questions repeatedly.

PASD proposes a straightforward way to make that possible: ship a comprehensive, structured `AI-SUPPORT.md` file alongside the software. A user can provide that file to a compatible assistant (for example, ChatGPT, Claude, Gemini, or a local LLM) and ask natural-language questions about installation, settings, workflows, errors, and limitations.

PASD is **a documentation specification and supporting authoring workflow**, not an AI model, hosted support service, chatbot, or mandatory runtime dependency. It does not guarantee that an assistant will answer correctly; the specification defines the knowledge boundaries and an optional protocol for evaluating support quality.

## Read the specification

**[Download the PASD 1.0 paper (PDF)](MMG-PASD-1.0-Portable_AI_Support_Documentation.pdf)** · **[LaTeX source](main.tex)**

The paper presents the rationale, normative requirements, implementation workflow, threat model, evaluation design, and a reference implementation. The specification is published as a **proposal**, not as an adopted industry standard; effectiveness testing remains future work.

## Why PASD?

Traditional documentation is primarily organised for human browsing. A user experiencing an unfamiliar symptom may need to search multiple pages, identify the relevant technical terminology, and reconstruct a troubleshooting procedure. Ungrounded language models can instead invent plausible but nonexistent settings, commands, and features.

PASD addresses this by packaging **verified, support-oriented knowledge** into a portable Markdown artifact. In particular, it requires a clear distinction between what software **supports**, what it **explicitly does not support**, and what the supplied documentation **does not establish**.


| Conventional support problem                            | PASD approach                                           |
| ------------------------------------------------------- | ------------------------------------------------------- |
| Scattered instructions and reference pages              | A self-contained support knowledge base                 |
| Users describe symptoms rather than internal components | Symptom-oriented troubleshooting paths                  |
| AI invents nonexistent controls or functionality        | Explicit *Negative Space* and calibrated uncertainty    |
| Confusion between the application and external tools    | Third-party responsibility and diagnostic boundaries    |
| Procedures may be unsafe or destructive                 | Backup warnings and non-destructive checks first        |
| Documentation gets out of date                          | Version bounding, staged review, and optional CI checks |
| Support quality is assumed rather than measured         | An optional, controlled evaluation protocol             |




## How it works

```text
Software project / maintainer
          │
          ├── verifies application behaviour
          ├── maintains support documentation
          └── distributes AI-SUPPORT.md
                         │
                         ▼
User downloads / attaches the document
(or provides the raw URL when retrieval is supported)
                         │
                         ▼
           User's preferred AI assistant
                         │
                         ▼
       Grounded, bounded support answer
```

The file is ordinary Markdown. It can be hosted in a public repository or bundled with an application; it does not require a PASD-specific server or proprietary inference API. Assistants differ in their ability to open URLs, accept attachments, and retrieve long documents, so **direct file attachment is the recommended baseline when supported by the client**.

## The three layers



### Layer A — Normative document specification

Defines what a conformant root-level `AI-SUPPORT.md` must contain, regardless of whether the document was written manually or with AI assistance. Core requirements include:

- Discoverability through the project's `README.md`.
- A consolidated, version-bounded reference for routine end-user support.
- Implementation-verified settings, UI labels, commands, defaults, and errors.
- A symptom index and safe troubleshooting sequences.
- Codebase-derived, actionable FAQs.
- **Negative Space:** explicit statements of unsupported capabilities and known limitations.
- Boundaries for third-party integrations and external dependencies.
- Instructions for handling uncertainty, escalation, and sensitive diagnostic data.



### Layer B — Recommended authoring and maintenance

A non-normative, staged approach to generating and reviewing the support artifact from the real codebase, keeping modular source documentation and the consolidated bundle consistent. Human-written PASD documents are equally valid: **using an AI coding assistant is not a conformance requirement**.

### Layer C — Optional empirical evaluation

A reproducible method for evaluating support answers across four conditions: no project documentation, conventional documentation, content-equivalent unstructured documentation, and full PASD documentation. Suggested measures include supported-feature accuracy, negative-boundary precision, calibrated abstention, unsafe recommendations, and downstream issue resolution.

**Layer A conformance does not require completing Layer C experiments.** This repository presents the evaluation protocol; it does not claim completed benchmark results demonstrating an improvement.

## The three-state epistemic model

This is a central PASD requirement:


| State                      | What the document establishes                                 | Expected response                                                      |
| -------------------------- | ------------------------------------------------------------- | ---------------------------------------------------------------------- |
| **Supported**              | The feature or workflow is verified for the specified version | Give the documented steps and prerequisites                            |
| **Explicitly unsupported** | The feature is positively identified as unavailable           | Say so directly; do not invent menus or workarounds                    |
| **Unknown / undocumented** | The provided evidence is insufficient                         | Acknowledge uncertainty, request details, or investigate when possible |


**Undocumented does not mean unsupported.** This avoids replacing hallucinated features with equally incorrect denials.

## Getting started: adopt PASD for your project

1. **Read the [PASD paper](MMG-PASD-1.0-Portable_AI_Support_Documentation.pdf)** to understand the normative requirements and the distinction between Layers A, B, and C.
2. **Review the [documentation generation specification](AI-SUPPORT-documentation-generation-prompt.md).** It describes how an AI coding assistant should inspect implementation, create modular reference files, document unsupported behaviour, and assemble `AI-SUPPORT.md`.
3. **Follow the [generation and publishing workflow](AI-SUPPORT%20Generation%20and%20Publishing%20Workflow.txt)** if you want a step-by-step AI-assisted approach. Its prompts are intended for a coding agent with repository access.
4. **Review the result against real implementation and tests.** Pay particular attention to exact UI labels, configuration defaults, destructive recovery procedures, and external tools. The agent's second pass is a consistency check, not independent proof of correctness.
5. **Publish the root** `AI-SUPPORT.md` **and link it prominently from your** `README.md`**.** Keep it version-aligned as the software evolves.
6. **Optionally create independent support test cases** and use the paper's Layer C methodology to assess effectiveness.

A PASD-style project may have a layout such as:

```text
project/
├── README.md
├── AI-SUPPORT.md
├── llms.txt                     # optional discovery pointer
└── docs/
    ├── getting-started.md
    ├── reference.md
    ├── configuration.md
    ├── troubleshooting.md
    ├── FAQ.md
    └── support-evaluation.md   # optional Layer C readiness
```

This is an illustrative layout, not a required list of filenames beyond the normative root support artifact and discoverability requirements. The publishing workflow may also generate additional project-specific support sections.

## Repository contents


| File                                                                                                               | Purpose                                                                         |
| ------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------- |
| **[PASD 1.0 paper](MMG-PASD-1.0-Portable_AI_Support_Documentation.pdf)**                                           | Full proposed specification, discussion, evaluation methodology, and appendices |
| **[main.tex](main.tex)**                                                                                           | Editable LaTeX source for the paper                                             |
| **[AI-SUPPORT-documentation-generation-prompt.md](AI-SUPPORT-documentation-generation-prompt.md)**                 | Detailed extraction and authoring instructions for a coding assistant           |
| **[AI-SUPPORT Generation and Publishing Workflow.txt](AI-SUPPORT%20Generation%20and%20Publishing%20Workflow.txt)** | Sequential prompts for generation, verification, cleanup, and publishing        |
| **[LICENSE](LICENSE)**                                                                                             | Licensing and attribution information                                           |




## Reference implementation

**[MyFileExplorer](https://github.com/dobrado76/MyFileExplorer)** is the reference application described in the paper: a desktop file manager whose repository distributes a consolidated **[AI-SUPPORT.md](https://github.com/dobrado76/MyFileExplorer/blob/main/AI-SUPPORT.md)** alongside conventional project documentation.

The example illustrates PASD's support-document structure and its treatment of real-world UI questions, third-party tools, and unsupported features. It is **not presented as completed empirical validation** of AI-answer accuracy.

## What PASD does not do

PASD does not require users to select any particular LLM provider; does not supply live application telemetry, execute repairs, or replace maintainer review; and does not guarantee that every assistant will ingest the entire document or retrieve every relevant passage. It complements rather than replaces conventional user documentation, issue trackers, and other support channels.

Large support artifacts can exceed smaller models' context windows. A structured index and clear headings can aid retrieval, but model capability and the client's document-processing workflow still matter.

## Contributing and discussion

Feedback, implementation experiences, proposed clarifications, and reproducible evaluation results are welcome through [GitHub Issues](https://github.com/MeaningfulnessMediaGroup/MMG-PASD/issues). Please distinguish normative specification changes from recommendations or experimental findings, and include relevant sections and reproducible examples where possible.

## Citation

If you reference this proposal in academic or technical work, please cite the published specification:

> Bee, D. (2026). *Portable AI-Assisted Software Support: A Proposal for a Standardized, Self-Contained Documentation Framework* (MMG-PASD-1.0). Meaningfulness Media Group. [GitHub repository](https://github.com/MeaningfulnessMediaGroup/MMG-PASD).



## License

© 2026 Meaningfulness Media Group. This repository's original documentation, paper, and authored instructional materials are made available under the **[Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)**, unless a particular file says otherwise. Attribution is required when redistributing or adapting licensed material. See [LICENSE](LICENSE).

*PASD is a proposed open documentation convention, not a certification or guarantee of software support quality.*