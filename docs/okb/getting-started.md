# Getting Started

This guide walks through validating your first OKB in under five minutes.

## Prerequisites

Install the Python dependencies:

    pip install rdflib pyshacl

Clone the block library:

    git clone https://github.com/Open-Systems-Collective/ek-blocks.git
    cd ek-blocks

## Run the Validation Script

The repository includes a ready-to-run validation script:

    python validate_example.py

Expected output: two blocks validated, conformant cases pass, violation cases fail with the expected SHACL messages.

## Manual Validation

You can also validate individual evidence graphs using pyshacl directly.

### Validate a conformant case (should pass)

    pyshacl -s kb_human_oversight/shapes.ttl \
            -e kb_human_oversight/schema.ttl \
            -df turtle \
            kb_human_oversight/tests/conform/full_oversight.ttl

### Validate a non-conformant case (should fail)

    pyshacl -s kb_human_oversight/shapes.ttl \
            -e kb_human_oversight/schema.ttl \
            -df turtle \
            kb_human_oversight/tests/violate/no_override.ttl

The output will identify exactly which shape fired, which node violated it, and what the violation message says.

## Write Your Own Evidence Graph

Here is a minimal evidence graph that satisfies the `kb_human_oversight` block:

    @prefix ex:  <http://example.org/okb#> .
    @prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

    ex:myDecision a ex:Decision ;
        ex:hasOversightRecord ex:rec1 ;
        ex:hasOverrideCapability ex:cap1 ;
        ex:hasSystemDescription ex:desc1 .

    ex:rec1 a ex:HumanOversightRecord ;
        ex:oversightTimestamp "2026-01-01T00:00:00Z"^^xsd:dateTime ;
        ex:reviewerID "reviewer-001" ;
        ex:oversightOutcome "approved" .

    ex:cap1 a ex:OverrideCapability ;
        ex:overrideMechanism "Manual override button in operator console" ;
        ex:overrideAvailable true .

    ex:desc1 a ex:AISystemDescription ;
        ex:capabilityDescription "Image classification for chest CT scans" ;
        ex:limitationDescription "Not validated for pediatric patients" .

Save this as `my_evidence.ttl` and validate:

    pyshacl -s kb_human_oversight/shapes.ttl \
            -e kb_human_oversight/schema.ttl \
            -df turtle my_evidence.ttl

## Build Your Own Block

To create a new OKB, follow this template:

1. Identify the regulatory obligation (e.g., a specific article or section)
2. Create the directory structure: `kb_your_block/schema.ttl`, `shapes.ttl`, `tests/`
3. Define the RDF vocabulary in `schema.ttl` (what classes and properties describe the evidence)
4. Define the SHACL shapes in `shapes.ttl` (what "must" be true)
5. Write at least one conformant and one non-conformant test case
6. Validate with pyshacl to confirm expected behavior
7. Submit a pull request to [ek-blocks](https://github.com/Open-Systems-Collective/ek-blocks)

For a detailed authoring guide, see the [OKB Community Guide](https://github.com/Open-Systems-Collective/osc-core).

## Next Steps

- Explore the [kb_human_oversight](kb-human-oversight.md) block (EU AI Act Article 14)
- Explore the [kb_clinical_safety](kb-clinical-safety.md) block (Medical AI, Oncology)
- Read the [OKB Overview](overview.md) for the full framework architecture
- Join the [Community Discussions](https://github.com/Open-Systems-Collective/osc-core/discussions)
