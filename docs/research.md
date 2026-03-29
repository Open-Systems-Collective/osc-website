# Research

The Open Systems Collective conducts research across several interconnected domains that shape the future of intelligent, ethical, and adaptive systems.

---

## Research Focus Areas

- **Responsible and Transparent AI.** Designing AI systems that produce interpretable outputs, carry provenance metadata, and comply with regulatory frameworks such as the EU AI Act.
- **Semantic and Symbolic Reasoning.** Using ontologies, knowledge graphs, and formal logic to represent and reason over complex domains including healthcare, governance, and ethics.
- **High-Performance and Distributed Systems.** Scheduling and orchestrating computational workflows across heterogeneous infrastructure, from edge devices to cloud clusters.
- **Workflow Mapping and Scheduling.** Modeling task dependencies and resource constraints to optimize execution across the compute continuum.
- **Ethics and Normative Modeling.** Encoding ethical principles and regulatory obligations as machine-checkable constraints using W3C Semantic Web standards.
- **Compute Continuum and Cognitive Scheduling.** Exploring adaptive scheduling strategies that integrate workload characteristics, resource availability, and quality-of-service requirements.

---

## Publications

### Ontological Knowledge Blocks: Executable, Profile-Based Compliance for Trustworthy AI Services

This paper introduces Ontological Knowledge Blocks (OKBs), a programmable governance infrastructure that compiles regulatory obligations into machine-checkable SHACL constraints over RDF evidence graphs. The framework defines a four-layer pipeline: obligations are captured as structured intermediate representations, compiled deterministically into SHACL shapes within modular Knowledge Blocks, validated against evidence graphs emitted by AI services, and composed into jurisdiction-specific profiles by set union. The paper demonstrates two working blocks (kb_human_oversight for EU AI Act Article 14, kb_clinical_safety for medical AI regulations) and provides a formal model for profile composition and conformance.

This work provides the theoretical and technical foundation for the [EK-BLOCKS](https://github.com/Open-Systems-Collective/ek-blocks) prototype and the [OKB Prototype Validator](https://github.com/AasishKumarSharma/open-knowledge-blocks).

---

## Community Guide

For researchers and practitioners who want to understand the OKB framework in depth, the [OSC Core](https://github.com/Open-Systems-Collective/osc-core) repository contains the community guide, governance documentation, and contribution guidelines. The guide covers the design rationale, block authoring process, and profile composition rules.

---

## Citing Our Work

If you reference work from the Open Systems Collective in your publications, please cite the corresponding repository DOI (via Zenodo). For specific citation formats, refer to the `CITATION.cff` file in each project repository where available.
