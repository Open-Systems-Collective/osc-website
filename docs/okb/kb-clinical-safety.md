# Block: kb_clinical_safety

**Version:** 1.0.0
**Regulatory Sources:** EU MDR 2017/745 (Annex XV, Art. 14(3)), FDA Clinical Decision Support Guidance, WHO Ethics and Governance of AI for Health (2021)
**Status:** Working prototype with full test coverage

---

## Overview

The `kb_clinical_safety` block encodes clinical safety obligations for AI-assisted medical diagnostics. It draws requirements from three regulatory frameworks and applies them to a concrete clinical scenario: AI-assisted lung cancer diagnosis in oncology. This block ensures that diagnostic recommendations are backed by clinical evidence, uncertainty quantification, patient cohort documentation, and qualified clinical review.

---

## Clinical Scenario

An AI system analyzes chest CT scans and produces a diagnostic recommendation (e.g., "suspected lung adenocarcinoma, SNOMED CT 254637007"). The `kb_clinical_safety` block validates that every such recommendation carries the evidence, uncertainty data, cohort context, and clinical review required by the applicable regulations.

---

## Obligations Encoded

| # | Obligation | Regulation | What It Checks |
|---|-----------|-----------|---------------|
| 1 | Clinical evidence required | EU MDR Annex XV | Every DiagnosticRecommendation must link to at least one ClinicalEvidence resource |
| 2 | Clinical evidence completeness | EU MDR Annex XV | Each ClinicalEvidence must include an evidence source URI and an evidence level |
| 3 | Uncertainty report required | FDA CDS Guidance | Every DiagnosticRecommendation must link to at least one UncertaintyReport |
| 4 | Uncertainty report completeness | FDA CDS Guidance | Each UncertaintyReport must include a confidence score and an uncertainty method |
| 5 | Patient cohort documentation required | WHO 2021 | Every DiagnosticRecommendation must link to at least one PatientCohortDescription |
| 6 | Patient cohort completeness | WHO 2021 | Each PatientCohortDescription must include a cohort size and description |
| 7 | Clinical review required | EU MDR Art. 14(3) | Every DiagnosticRecommendation must link to at least one ClinicalReviewRecord |
| 8 | Clinical review completeness | EU MDR Art. 14(3) | Each ClinicalReviewRecord must include reviewer qualification, timestamp, and decision |

---

## Schema Reference

### Classes

| Class | Description |
|-------|------------|
| `ex:DiagnosticRecommendation` | An AI-generated diagnostic recommendation subject to clinical safety validation |
| `ex:ClinicalEvidence` | Clinical evidence supporting a diagnostic recommendation |
| `ex:UncertaintyReport` | A report quantifying the uncertainty of an AI diagnostic output |
| `ex:PatientCohortDescription` | A description of the patient cohort on which the AI model was trained or validated |
| `ex:ClinicalReviewRecord` | A record of clinical review performed by a qualified healthcare professional |

### Object Properties

| Property | Domain | Range | Description |
|----------|--------|-------|------------|
| `ex:hasClinicalEvidence` | DiagnosticRecommendation | ClinicalEvidence | Links a recommendation to its supporting clinical evidence |
| `ex:hasUncertaintyReport` | DiagnosticRecommendation | UncertaintyReport | Links a recommendation to its uncertainty quantification |
| `ex:hasPatientCohort` | DiagnosticRecommendation | PatientCohortDescription | Links a recommendation to the relevant patient cohort data |
| `ex:hasClinicalReview` | DiagnosticRecommendation | ClinicalReviewRecord | Links a recommendation to its clinical review record |

### Datatype Properties

| Property | Domain | Datatype | Description |
|----------|--------|----------|------------|
| `ex:evidenceSource` | ClinicalEvidence | xsd:anyURI | URI pointing to the source of clinical evidence |
| `ex:evidenceLevel` | ClinicalEvidence | xsd:string | Level of evidence (e.g., "Level I", "Level II") |
| `ex:confidenceScore` | UncertaintyReport | xsd:decimal | Model confidence score for the recommendation |
| `ex:uncertaintyMethod` | UncertaintyReport | xsd:string | Method used to quantify uncertainty (e.g., "MC Dropout") |
| `ex:cohortSize` | PatientCohortDescription | xsd:integer | Number of patients in the training or validation cohort |
| `ex:cohortDescription` | PatientCohortDescription | xsd:string | Description of the patient cohort demographics and scope |
| `ex:reviewerQualification` | ClinicalReviewRecord | xsd:string | Professional qualification of the clinical reviewer |
| `ex:reviewTimestamp` | ClinicalReviewRecord | xsd:dateTime | When the clinical review was performed |
| `ex:reviewDecision` | ClinicalReviewRecord | xsd:string | Outcome of the clinical review (e.g., "confirmed", "overridden") |
| `ex:icdOCode` | DiagnosticRecommendation | xsd:string | ICD-O morphology code for the diagnosis |
| `ex:recommendedAction` | DiagnosticRecommendation | xsd:string | Recommended clinical action |

---

## Shapes Reference

| Shape | Target | Constraints | SHACL Message |
|-------|--------|------------|---------------|
| `ClinicalEvidenceRequiredShape` | `ex:DiagnosticRecommendation` | minCount 1 on `ex:hasClinicalEvidence` | "Diagnostic recommendation must have clinical evidence (MDR Annex XV)" |
| `ClinicalEvidenceShape` | `ex:ClinicalEvidence` | minCount 1 on `ex:evidenceSource`, `ex:evidenceLevel` | "Clinical evidence must include source URI and evidence level" |
| `UncertaintyRequiredShape` | `ex:DiagnosticRecommendation` | minCount 1 on `ex:hasUncertaintyReport` | "Diagnostic recommendation must have an uncertainty report (FDA CDS)" |
| `UncertaintyReportShape` | `ex:UncertaintyReport` | minCount 1 on `ex:confidenceScore`, `ex:uncertaintyMethod` | "Uncertainty report must include confidence score and method" |
| `CohortDocumentationRequiredShape` | `ex:DiagnosticRecommendation` | minCount 1 on `ex:hasPatientCohort` | "Diagnostic recommendation must have patient cohort documentation (WHO 2021)" |
| `PatientCohortShape` | `ex:PatientCohortDescription` | minCount 1 on `ex:cohortSize`, `ex:cohortDescription` | "Patient cohort must include size and description" |
| `ClinicalReviewRequiredShape` | `ex:DiagnosticRecommendation` | minCount 1 on `ex:hasClinicalReview` | "Diagnostic recommendation must have a clinical review record (MDR Art.14(3))" |
| `ClinicalReviewRecordShape` | `ex:ClinicalReviewRecord` | minCount 1 on `ex:reviewerQualification`, `ex:reviewTimestamp`, `ex:reviewDecision` | "Clinical review must include reviewer qualification, timestamp, and decision" |

---

## Full Schema (schema.ttl)

??? note "Click to expand schema.ttl"

    ```turtle
    @prefix ex:   <http://example.org/okb#> .
    @prefix owl:  <http://www.w3.org/2002/07/owl#> .
    @prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
    @prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .

    # Classes
    ex:DiagnosticRecommendation a owl:Class ;
        rdfs:label "Diagnostic Recommendation" ;
        rdfs:comment "An AI-generated diagnostic recommendation subject to clinical safety validation." .

    ex:ClinicalEvidence a owl:Class ;
        rdfs:label "Clinical Evidence" ;
        rdfs:comment "Clinical evidence supporting a diagnostic recommendation." .

    ex:UncertaintyReport a owl:Class ;
        rdfs:label "Uncertainty Report" ;
        rdfs:comment "A report quantifying the uncertainty of an AI diagnostic output." .

    ex:PatientCohortDescription a owl:Class ;
        rdfs:label "Patient Cohort Description" ;
        rdfs:comment "A description of the patient cohort used for training or validation." .

    ex:ClinicalReviewRecord a owl:Class ;
        rdfs:label "Clinical Review Record" ;
        rdfs:comment "A record of clinical review by a qualified healthcare professional." .

    # Object Properties
    ex:hasClinicalEvidence a owl:ObjectProperty ;
        rdfs:domain ex:DiagnosticRecommendation ;
        rdfs:range ex:ClinicalEvidence ;
        rdfs:label "has clinical evidence" .

    ex:hasUncertaintyReport a owl:ObjectProperty ;
        rdfs:domain ex:DiagnosticRecommendation ;
        rdfs:range ex:UncertaintyReport ;
        rdfs:label "has uncertainty report" .

    ex:hasPatientCohort a owl:ObjectProperty ;
        rdfs:domain ex:DiagnosticRecommendation ;
        rdfs:range ex:PatientCohortDescription ;
        rdfs:label "has patient cohort" .

    ex:hasClinicalReview a owl:ObjectProperty ;
        rdfs:domain ex:DiagnosticRecommendation ;
        rdfs:range ex:ClinicalReviewRecord ;
        rdfs:label "has clinical review" .

    # Datatype Properties
    ex:evidenceSource a owl:DatatypeProperty ;
        rdfs:domain ex:ClinicalEvidence ;
        rdfs:range xsd:anyURI ;
        rdfs:label "evidence source" .

    ex:evidenceLevel a owl:DatatypeProperty ;
        rdfs:domain ex:ClinicalEvidence ;
        rdfs:range xsd:string ;
        rdfs:label "evidence level" .

    ex:confidenceScore a owl:DatatypeProperty ;
        rdfs:domain ex:UncertaintyReport ;
        rdfs:range xsd:decimal ;
        rdfs:label "confidence score" .

    ex:uncertaintyMethod a owl:DatatypeProperty ;
        rdfs:domain ex:UncertaintyReport ;
        rdfs:range xsd:string ;
        rdfs:label "uncertainty method" .

    ex:cohortSize a owl:DatatypeProperty ;
        rdfs:domain ex:PatientCohortDescription ;
        rdfs:range xsd:integer ;
        rdfs:label "cohort size" .

    ex:cohortDescription a owl:DatatypeProperty ;
        rdfs:domain ex:PatientCohortDescription ;
        rdfs:range xsd:string ;
        rdfs:label "cohort description" .

    ex:reviewerQualification a owl:DatatypeProperty ;
        rdfs:domain ex:ClinicalReviewRecord ;
        rdfs:range xsd:string ;
        rdfs:label "reviewer qualification" .

    ex:reviewTimestamp a owl:DatatypeProperty ;
        rdfs:domain ex:ClinicalReviewRecord ;
        rdfs:range xsd:dateTime ;
        rdfs:label "review timestamp" .

    ex:reviewDecision a owl:DatatypeProperty ;
        rdfs:domain ex:ClinicalReviewRecord ;
        rdfs:range xsd:string ;
        rdfs:label "review decision" .

    ex:icdOCode a owl:DatatypeProperty ;
        rdfs:domain ex:DiagnosticRecommendation ;
        rdfs:range xsd:string ;
        rdfs:label "ICD-O code" .

    ex:recommendedAction a owl:DatatypeProperty ;
        rdfs:domain ex:DiagnosticRecommendation ;
        rdfs:range xsd:string ;
        rdfs:label "recommended action" .
    ```

## Full Shapes (shapes.ttl)

??? note "Click to expand shapes.ttl"

    ```turtle
    @prefix ex:   <http://example.org/okb#> .
    @prefix sh:   <http://www.w3.org/ns/shacl#> .
    @prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .

    ex:ClinicalEvidenceRequiredShape a sh:NodeShape ;
        sh:targetClass ex:DiagnosticRecommendation ;
        sh:property [
            sh:path ex:hasClinicalEvidence ;
            sh:minCount 1 ;
            sh:message "Diagnostic recommendation must have clinical evidence (MDR Annex XV)" ;
        ] .

    ex:ClinicalEvidenceShape a sh:NodeShape ;
        sh:targetClass ex:ClinicalEvidence ;
        sh:property [
            sh:path ex:evidenceSource ;
            sh:minCount 1 ;
            sh:datatype xsd:anyURI ;
            sh:message "Clinical evidence must include a source URI" ;
        ] ;
        sh:property [
            sh:path ex:evidenceLevel ;
            sh:minCount 1 ;
            sh:datatype xsd:string ;
            sh:message "Clinical evidence must include an evidence level" ;
        ] .

    ex:UncertaintyRequiredShape a sh:NodeShape ;
        sh:targetClass ex:DiagnosticRecommendation ;
        sh:property [
            sh:path ex:hasUncertaintyReport ;
            sh:minCount 1 ;
            sh:message "Diagnostic recommendation must have an uncertainty report (FDA CDS)" ;
        ] .

    ex:UncertaintyReportShape a sh:NodeShape ;
        sh:targetClass ex:UncertaintyReport ;
        sh:property [
            sh:path ex:confidenceScore ;
            sh:minCount 1 ;
            sh:datatype xsd:decimal ;
            sh:message "Uncertainty report must include a confidence score" ;
        ] ;
        sh:property [
            sh:path ex:uncertaintyMethod ;
            sh:minCount 1 ;
            sh:datatype xsd:string ;
            sh:message "Uncertainty report must include the uncertainty method" ;
        ] .

    ex:CohortDocumentationRequiredShape a sh:NodeShape ;
        sh:targetClass ex:DiagnosticRecommendation ;
        sh:property [
            sh:path ex:hasPatientCohort ;
            sh:minCount 1 ;
            sh:message "Diagnostic recommendation must have patient cohort documentation (WHO 2021)" ;
        ] .

    ex:PatientCohortShape a sh:NodeShape ;
        sh:targetClass ex:PatientCohortDescription ;
        sh:property [
            sh:path ex:cohortSize ;
            sh:minCount 1 ;
            sh:datatype xsd:integer ;
            sh:message "Patient cohort must include a cohort size" ;
        ] ;
        sh:property [
            sh:path ex:cohortDescription ;
            sh:minCount 1 ;
            sh:datatype xsd:string ;
            sh:message "Patient cohort must include a description" ;
        ] .

    ex:ClinicalReviewRequiredShape a sh:NodeShape ;
        sh:targetClass ex:DiagnosticRecommendation ;
        sh:property [
            sh:path ex:hasClinicalReview ;
            sh:minCount 1 ;
            sh:message "Diagnostic recommendation must have a clinical review record (MDR Art.14(3))" ;
        ] .

    ex:ClinicalReviewRecordShape a sh:NodeShape ;
        sh:targetClass ex:ClinicalReviewRecord ;
        sh:property [
            sh:path ex:reviewerQualification ;
            sh:minCount 1 ;
            sh:datatype xsd:string ;
            sh:message "Clinical review must include reviewer qualification" ;
        ] ;
        sh:property [
            sh:path ex:reviewTimestamp ;
            sh:minCount 1 ;
            sh:datatype xsd:dateTime ;
            sh:message "Clinical review must include a timestamp" ;
        ] ;
        sh:property [
            sh:path ex:reviewDecision ;
            sh:minCount 1 ;
            sh:datatype xsd:string ;
            sh:message "Clinical review must include a decision" ;
        ] .
    ```

---

## Test Cases

### Conformant: Lung Adenocarcinoma Diagnosis

This evidence graph documents a complete AI-assisted lung adenocarcinoma diagnosis with all required evidence, uncertainty data, cohort documentation, and clinical review.

```turtle
@prefix ex:  <http://example.org/okb#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

ex:rec1 a ex:DiagnosticRecommendation ;
    ex:icdOCode "8140/3" ;
    ex:recommendedAction "Refer to multidisciplinary tumour board" ;
    ex:hasClinicalEvidence ex:ev1 ;
    ex:hasUncertaintyReport ex:unc1 ;
    ex:hasPatientCohort ex:cohort1 ;
    ex:hasClinicalReview ex:review1 .

ex:ev1 a ex:ClinicalEvidence ;
    ex:evidenceSource "https://doi.org/10.1016/j.jtho.2020.01.005"^^xsd:anyURI ;
    ex:evidenceLevel "Level II" .

ex:unc1 a ex:UncertaintyReport ;
    ex:confidenceScore "0.87"^^xsd:decimal ;
    ex:uncertaintyMethod "Monte Carlo Dropout, 50 forward passes" .

ex:cohort1 a ex:PatientCohortDescription ;
    ex:cohortSize "12400"^^xsd:integer ;
    ex:cohortDescription "Adults aged 45-80, mixed ethnicity, NLST and LIDC-IDRI datasets" .

ex:review1 a ex:ClinicalReviewRecord ;
    ex:reviewerQualification "Board-certified thoracic radiologist, 12 years experience" ;
    ex:reviewTimestamp "2026-03-10T11:45:00Z"^^xsd:dateTime ;
    ex:reviewDecision "confirmed" .
```

### Non-conformant: Missing Uncertainty Report

This evidence graph omits the uncertainty report. The `UncertaintyRequiredShape` will fire a violation.

```turtle
@prefix ex:  <http://example.org/okb#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

ex:rec2 a ex:DiagnosticRecommendation ;
    ex:icdOCode "8140/3" ;
    ex:recommendedAction "Refer to oncology" ;
    ex:hasClinicalEvidence ex:ev2 ;
    ex:hasPatientCohort ex:cohort2 ;
    ex:hasClinicalReview ex:review2 .

ex:ev2 a ex:ClinicalEvidence ;
    ex:evidenceSource "https://doi.org/10.1016/j.jtho.2020.01.005"^^xsd:anyURI ;
    ex:evidenceLevel "Level II" .

ex:cohort2 a ex:PatientCohortDescription ;
    ex:cohortSize "8000"^^xsd:integer ;
    ex:cohortDescription "Adults aged 50-75, European cohort" .

ex:review2 a ex:ClinicalReviewRecord ;
    ex:reviewerQualification "Consultant oncologist" ;
    ex:reviewTimestamp "2026-03-12T09:00:00Z"^^xsd:dateTime ;
    ex:reviewDecision "confirmed" .
```

Expected violation message: "Diagnostic recommendation must have an uncertainty report (FDA CDS)"

---

## Minimal Conformant Evidence

The smallest evidence graph that passes all eight shapes:

```turtle
@prefix ex:  <http://example.org/okb#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

ex:r a ex:DiagnosticRecommendation ;
    ex:hasClinicalEvidence ex:e ;
    ex:hasUncertaintyReport ex:u ;
    ex:hasPatientCohort ex:p ;
    ex:hasClinicalReview ex:c .

ex:e a ex:ClinicalEvidence ;
    ex:evidenceSource "https://example.org/study"^^xsd:anyURI ;
    ex:evidenceLevel "Level I" .

ex:u a ex:UncertaintyReport ;
    ex:confidenceScore "0.92"^^xsd:decimal ;
    ex:uncertaintyMethod "MC Dropout" .

ex:p a ex:PatientCohortDescription ;
    ex:cohortSize "1000"^^xsd:integer ;
    ex:cohortDescription "General adult population" .

ex:c a ex:ClinicalReviewRecord ;
    ex:reviewerQualification "Radiologist" ;
    ex:reviewTimestamp "2026-01-01T00:00:00Z"^^xsd:dateTime ;
    ex:reviewDecision "confirmed" .
```

---

## How to Validate

Validate the conformant test case:

    pyshacl -s kb_clinical_safety/shapes.ttl \
            -e kb_clinical_safety/schema.ttl \
            -df turtle \
            kb_clinical_safety/tests/conform/lung_adenocarcinoma.ttl

Validate the non-conformant test case (expects failure):

    pyshacl -s kb_clinical_safety/shapes.ttl \
            -e kb_clinical_safety/schema.ttl \
            -df turtle \
            kb_clinical_safety/tests/violate/missing_uncertainty.ttl

For more information on running validations, see the [Getting Started](getting-started.md) guide.

---

## Source

- [kb_clinical_safety in ek-blocks](https://github.com/Open-Systems-Collective/ek-blocks/tree/main/kb_clinical_safety)
- [OKB Prototype Validator](https://github.com/AasishKumarSharma/open-knowledge-blocks)
