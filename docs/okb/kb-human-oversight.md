# Block: kb_human_oversight

**Version:** 1.0.0
**Regulatory Source:** EU AI Act, Article 14 (Human Oversight of High-Risk AI Systems)
**Status:** Working prototype with full test coverage

---

## Overview

The `kb_human_oversight` block encodes the human oversight requirements of Article 14 of the EU AI Act. Article 14 mandates that high-risk AI systems be designed and developed so that they can be effectively overseen by natural persons during the period of use. This block translates those obligations into a machine-checkable SHACL constraint set over an RDF evidence graph.

---

## Obligations Encoded

This block encodes five core requirements derived from Article 14:

| # | Obligation | Article Reference | What It Checks |
|---|-----------|-------------------|---------------|
| 1 | Human oversight record required | Art. 14(1) | Every Decision must link to at least one HumanOversightRecord |
| 2 | Oversight record completeness | Art. 14(4)(a) | Each HumanOversightRecord must include a timestamp, reviewer ID, and outcome |
| 3 | Override capability required | Art. 14(4)(d) | Every Decision must link to at least one OverrideCapability |
| 4 | Override capability completeness | Art. 14(4)(d) | Each OverrideCapability must declare the mechanism and availability status |
| 5 | System description required | Art. 14(4)(b) | Every Decision must link to an AISystemDescription with capability and limitation statements |

---

## Schema Reference

### Classes

| Class | Description |
|-------|------------|
| `ex:Decision` | A decision made by an AI system that is subject to human oversight |
| `ex:HumanOversightRecord` | A record of a human oversight action taken on a decision |
| `ex:OverrideCapability` | Documentation of the ability for a human to override an AI decision |
| `ex:AISystemDescription` | A description of the AI system capabilities and limitations |

### Object Properties

| Property | Domain | Range | Description |
|----------|--------|-------|------------|
| `ex:hasOversightRecord` | Decision | HumanOversightRecord | Links a decision to its oversight record |
| `ex:hasOverrideCapability` | Decision | OverrideCapability | Links a decision to its override capability documentation |
| `ex:hasSystemDescription` | Decision | AISystemDescription | Links a decision to the system description |

### Datatype Properties

| Property | Domain | Datatype | Description |
|----------|--------|----------|------------|
| `ex:oversightTimestamp` | HumanOversightRecord | xsd:dateTime | When the oversight action occurred |
| `ex:reviewerID` | HumanOversightRecord | xsd:string | Identifier of the human reviewer |
| `ex:oversightOutcome` | HumanOversightRecord | xsd:string | Result of the oversight review (e.g., "approved", "rejected") |
| `ex:overrideMechanism` | OverrideCapability | xsd:string | Description of how a human can override the decision |
| `ex:overrideAvailable` | OverrideCapability | xsd:boolean | Whether the override mechanism is currently available |
| `ex:capabilityDescription` | AISystemDescription | xsd:string | What the AI system can do |
| `ex:limitationDescription` | AISystemDescription | xsd:string | Known limitations of the AI system |

---

## Shapes Reference

| Shape | Target | Constraints | SHACL Message |
|-------|--------|------------|---------------|
| `HumanOversightRequiredShape` | `ex:Decision` | minCount 1 on `ex:hasOversightRecord` | "Decision must have at least one human oversight record (Art.14)" |
| `OversightRecordShape` | `ex:HumanOversightRecord` | minCount 1 on `ex:oversightTimestamp`, `ex:reviewerID`, `ex:oversightOutcome` | "Oversight record must include timestamp, reviewer ID, and outcome" |
| `OverrideRequiredShape` | `ex:Decision` | minCount 1 on `ex:hasOverrideCapability` | "Decision must have at least one override capability (Art.14(4)(d))" |
| `OverrideCapabilityShape` | `ex:OverrideCapability` | minCount 1 on `ex:overrideMechanism`, `ex:overrideAvailable` | "Override capability must specify mechanism and availability" |
| `SystemDescriptionRequiredShape` | `ex:Decision` | minCount 1 on `ex:hasSystemDescription` | "Decision must have a system description (Art.14(4)(b))" |

---

## Full Schema (schema.ttl)

??? note "Click to expand schema.ttl"

    ```turtle
    @prefix ex:   <http://example.org/okb#> .
    @prefix owl:  <http://www.w3.org/2002/07/owl#> .
    @prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
    @prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .

    # Classes
    ex:Decision a owl:Class ;
        rdfs:label "Decision" ;
        rdfs:comment "A decision made by an AI system subject to human oversight." .

    ex:HumanOversightRecord a owl:Class ;
        rdfs:label "Human Oversight Record" ;
        rdfs:comment "A record of human oversight applied to a decision." .

    ex:OverrideCapability a owl:Class ;
        rdfs:label "Override Capability" ;
        rdfs:comment "Documentation of the ability for a human to override an AI decision." .

    ex:AISystemDescription a owl:Class ;
        rdfs:label "AI System Description" ;
        rdfs:comment "A description of the AI system capabilities and limitations." .

    # Object Properties
    ex:hasOversightRecord a owl:ObjectProperty ;
        rdfs:domain ex:Decision ;
        rdfs:range ex:HumanOversightRecord ;
        rdfs:label "has oversight record" .

    ex:hasOverrideCapability a owl:ObjectProperty ;
        rdfs:domain ex:Decision ;
        rdfs:range ex:OverrideCapability ;
        rdfs:label "has override capability" .

    ex:hasSystemDescription a owl:ObjectProperty ;
        rdfs:domain ex:Decision ;
        rdfs:range ex:AISystemDescription ;
        rdfs:label "has system description" .

    # Datatype Properties
    ex:oversightTimestamp a owl:DatatypeProperty ;
        rdfs:domain ex:HumanOversightRecord ;
        rdfs:range xsd:dateTime ;
        rdfs:label "oversight timestamp" .

    ex:reviewerID a owl:DatatypeProperty ;
        rdfs:domain ex:HumanOversightRecord ;
        rdfs:range xsd:string ;
        rdfs:label "reviewer ID" .

    ex:oversightOutcome a owl:DatatypeProperty ;
        rdfs:domain ex:HumanOversightRecord ;
        rdfs:range xsd:string ;
        rdfs:label "oversight outcome" .

    ex:overrideMechanism a owl:DatatypeProperty ;
        rdfs:domain ex:OverrideCapability ;
        rdfs:range xsd:string ;
        rdfs:label "override mechanism" .

    ex:overrideAvailable a owl:DatatypeProperty ;
        rdfs:domain ex:OverrideCapability ;
        rdfs:range xsd:boolean ;
        rdfs:label "override available" .

    ex:capabilityDescription a owl:DatatypeProperty ;
        rdfs:domain ex:AISystemDescription ;
        rdfs:range xsd:string ;
        rdfs:label "capability description" .

    ex:limitationDescription a owl:DatatypeProperty ;
        rdfs:domain ex:AISystemDescription ;
        rdfs:range xsd:string ;
        rdfs:label "limitation description" .
    ```

## Full Shapes (shapes.ttl)

??? note "Click to expand shapes.ttl"

    ```turtle
    @prefix ex:   <http://example.org/okb#> .
    @prefix sh:   <http://www.w3.org/ns/shacl#> .
    @prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .

    ex:HumanOversightRequiredShape a sh:NodeShape ;
        sh:targetClass ex:Decision ;
        sh:property [
            sh:path ex:hasOversightRecord ;
            sh:minCount 1 ;
            sh:message "Decision must have at least one human oversight record (Art.14)" ;
        ] .

    ex:OversightRecordShape a sh:NodeShape ;
        sh:targetClass ex:HumanOversightRecord ;
        sh:property [
            sh:path ex:oversightTimestamp ;
            sh:minCount 1 ;
            sh:datatype xsd:dateTime ;
            sh:message "Oversight record must include a timestamp" ;
        ] ;
        sh:property [
            sh:path ex:reviewerID ;
            sh:minCount 1 ;
            sh:datatype xsd:string ;
            sh:message "Oversight record must include a reviewer ID" ;
        ] ;
        sh:property [
            sh:path ex:oversightOutcome ;
            sh:minCount 1 ;
            sh:datatype xsd:string ;
            sh:message "Oversight record must include an outcome" ;
        ] .

    ex:OverrideRequiredShape a sh:NodeShape ;
        sh:targetClass ex:Decision ;
        sh:property [
            sh:path ex:hasOverrideCapability ;
            sh:minCount 1 ;
            sh:message "Decision must have at least one override capability (Art.14(4)(d))" ;
        ] .

    ex:OverrideCapabilityShape a sh:NodeShape ;
        sh:targetClass ex:OverrideCapability ;
        sh:property [
            sh:path ex:overrideMechanism ;
            sh:minCount 1 ;
            sh:datatype xsd:string ;
            sh:message "Override capability must specify a mechanism" ;
        ] ;
        sh:property [
            sh:path ex:overrideAvailable ;
            sh:minCount 1 ;
            sh:datatype xsd:boolean ;
            sh:message "Override capability must specify availability" ;
        ] .

    ex:SystemDescriptionRequiredShape a sh:NodeShape ;
        sh:targetClass ex:Decision ;
        sh:property [
            sh:path ex:hasSystemDescription ;
            sh:minCount 1 ;
            sh:message "Decision must have a system description (Art.14(4)(b))" ;
        ] .
    ```

---

## Test Cases

### Conformant: Full Oversight Evidence

This evidence graph satisfies all five shapes. It documents a chest CT classification decision with complete oversight, override capability, and system description.

```turtle
@prefix ex:  <http://example.org/okb#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

ex:decision1 a ex:Decision ;
    ex:hasOversightRecord ex:oversight1 ;
    ex:hasOverrideCapability ex:override1 ;
    ex:hasSystemDescription ex:sysDesc1 .

ex:oversight1 a ex:HumanOversightRecord ;
    ex:oversightTimestamp "2026-01-15T09:30:00Z"^^xsd:dateTime ;
    ex:reviewerID "reviewer-042" ;
    ex:oversightOutcome "approved" .

ex:override1 a ex:OverrideCapability ;
    ex:overrideMechanism "Manual override button in operator console" ;
    ex:overrideAvailable true .

ex:sysDesc1 a ex:AISystemDescription ;
    ex:capabilityDescription "Image classification for chest CT scans" ;
    ex:limitationDescription "Not validated for pediatric patients" .
```

### Non-conformant: Missing Override Capability

This evidence graph is missing the override capability link. The `OverrideRequiredShape` will fire a violation.

```turtle
@prefix ex:  <http://example.org/okb#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

ex:decision2 a ex:Decision ;
    ex:hasOversightRecord ex:oversight2 ;
    ex:hasSystemDescription ex:sysDesc2 .

ex:oversight2 a ex:HumanOversightRecord ;
    ex:oversightTimestamp "2026-02-01T14:00:00Z"^^xsd:dateTime ;
    ex:reviewerID "reviewer-007" ;
    ex:oversightOutcome "flagged" .

ex:sysDesc2 a ex:AISystemDescription ;
    ex:capabilityDescription "Radiology triage assistant" ;
    ex:limitationDescription "Limited to adult chest X-rays only" .
```

Expected violation message: "Decision must have at least one override capability (Art.14(4)(d))"

---

## Minimal Conformant Evidence

The smallest evidence graph that passes all five shapes:

```turtle
@prefix ex:  <http://example.org/okb#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

ex:d a ex:Decision ;
    ex:hasOversightRecord ex:r ;
    ex:hasOverrideCapability ex:c ;
    ex:hasSystemDescription ex:s .

ex:r a ex:HumanOversightRecord ;
    ex:oversightTimestamp "2026-01-01T00:00:00Z"^^xsd:dateTime ;
    ex:reviewerID "r1" ;
    ex:oversightOutcome "approved" .

ex:c a ex:OverrideCapability ;
    ex:overrideMechanism "console" ;
    ex:overrideAvailable true .

ex:s a ex:AISystemDescription ;
    ex:capabilityDescription "classification" ;
    ex:limitationDescription "none declared" .
```

---

## How to Validate

Validate the conformant test case:

    pyshacl -s kb_human_oversight/shapes.ttl \
            -e kb_human_oversight/schema.ttl \
            -df turtle \
            kb_human_oversight/tests/conform/full_oversight.ttl

Validate the non-conformant test case (expects failure):

    pyshacl -s kb_human_oversight/shapes.ttl \
            -e kb_human_oversight/schema.ttl \
            -df turtle \
            kb_human_oversight/tests/violate/no_override.ttl

For more information on running validations, see the [Getting Started](getting-started.md) guide.

---

## Source

- [kb_human_oversight in ek-blocks](https://github.com/Open-Systems-Collective/ek-blocks/tree/main/kb_human_oversight)
- [OKB Prototype Validator](https://github.com/AasishKumarSharma/open-knowledge-blocks)
