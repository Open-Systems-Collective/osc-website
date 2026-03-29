# Open Knowledge Blocks (OKBs)

## What is an OKB?

An Open Knowledge Block is a self-contained, machine-interpretable module that encodes a single ethical or regulatory obligation as a formal ontological schema paired with an executable constraint set. Blocks are composed into jurisdiction-specific compliance profiles. AI services emit structured evidence graphs. Validators apply profiles to evidence and produce deterministic compliance reports. The entire process is reproducible, continuous, and open.

## The Four-Layer Compliance Pipeline

OKBs translate governance obligations into machine-checkable infrastructure through four layers:

**Layer 1: Obligation Representation**

Obligations are captured as structured Regulatory Intermediate Representation (IR) records. Each IR record identifies the target concept, the required relation or attribute, the constraint type, and the severity level. IR records are authored and approved by human compliance engineers.

**Layer 2: Deterministic Compilation**

A compiler maps each IR record to a SHACL node shape within a Knowledge Block module. The formal model is:

KB = (O, C, V, E, P)

Where O is the obligation set, C is the RDF/OWL concept schema, V is the SHACL shape set, E specifies required evidence artifacts, and P encodes PROV-O provenance links.

**Layer 3: Evidence Graph Construction**

Each AI service decision is accompanied by an evidence graph: an RDF document that links the decision to its provenance artifacts, usage logs, explanation references, and any quantitative measurements required by the active profile. The evidence graph is emitted by the AI service itself, not reconstructed after the fact.

**Layer 4: Profile Composition and Validation**

A governance profile is a named selection of KB modules. Composition is defined by set union over all tuple components. The SHACL validation engine evaluates all shapes over the evidence graph.

## Block Structure

Every OKB follows a canonical directory layout:

    kb_block_name/
      schema.ttl       # RDF/OWL vocabulary (classes and properties)
      shapes.ttl       # SHACL constraint shapes (the "must" rules)
      metadata.ttl     # DCAT-AP dataset metadata
      README.md        # Documentation
      tests/
        conform/       # Evidence graphs that pass all shapes
        violate/       # Evidence graphs that fail specific shapes

## The Six Core Ethical Block Families

The EK-BLOCKS framework defines six canonical ethical dimensions:

| Block | Dimension | Core Obligation |
|-------|----------|----------------|
| kb_transparency | Transparency | AI decisions must carry human-interpretable explanations with resolvable evidence URIs |
| kb_accountability | Accountability | Decision processes must be traceable to responsible actors, activities, and artefacts via PROV-O chains |
| kb_trust | Trust | AI services must demonstrate reliability metrics and uncertainty estimates within declared thresholds |
| kb_control | Control | Human oversight mechanisms must be evidenced; override capability must be asserted |
| kb_fairness | Fairness | Allocation disparities across protected groups must not exceed declared thresholds; FAIR data principles apply to training data |
| kb_governance | Governance | Risk management procedures, audit logs, and regulatory registration must be evidenced |

## Profile Composition

Profiles compose blocks by set union. For example:

- **EU Baseline**: kb_transparency + kb_accountability + kb_governance
- **EU High-Risk**: EU Baseline + kb_control + kb_fairness
- **US Baseline**: kb_accountability + kb_governance
- **Healthcare**: kb_clinical_safety + kb_control + kb_trust

A system conformant under EU High-Risk is necessarily conformant under EU Baseline and US Baseline.

## Technology Stack

OKBs are built on mature W3C standards:

| Standard | Role |
|----------|------|
| RDF/OWL | Concept schema and vocabulary |
| SHACL | Constraint shapes and validation |
| SPARQL | Quantitative and relational constraints |
| PROV-O | Provenance and traceability |
| DCAT-AP | Block metadata and discovery |

## Source Code and Prototype

- **Block Library**: [github.com/Open-Systems-Collective/ek-blocks](https://github.com/Open-Systems-Collective/ek-blocks)
- **Prototype Validator**: [github.com/AasishKumarSharma/open-knowledge-blocks](https://github.com/AasishKumarSharma/open-knowledge-blocks)
