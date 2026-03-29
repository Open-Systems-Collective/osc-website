# Community

The Open Systems Collective welcomes contributions from anyone interested in open and responsible computational research.

---

## How to Contribute

There are several ways to get involved:

- **Fork a repository and submit pull requests:** Pick an issue or feature that interests you, fork the relevant repo, and open a PR when your changes are ready.
- **Open discussions or issues:** Share new ideas, report bugs, or propose improvements through GitHub Issues and Discussions.
- **Join as a collaborator:** If you want to take on a larger role in an ongoing project, reach out through our Community Board.

---

## Contributing a New Block

Open Knowledge Blocks (OKBs) are the building blocks of our compliance infrastructure. Contributing a new block is one of the most impactful ways to help the project. Here is the process:

1. **Identify a regulatory obligation.** Choose a specific article, section, or requirement from a regulation (e.g., GDPR Article 22, NIST AI RMF). Each block should encode one coherent obligation set.

2. **Create the block directory.** Follow the canonical layout:

        kb_your_block/
          schema.ttl       # RDF/OWL vocabulary
          shapes.ttl       # SHACL constraint shapes
          metadata.ttl     # DCAT-AP dataset metadata
          README.md        # Documentation
          tests/
            conform/       # Evidence graphs that pass
            violate/       # Evidence graphs that fail

3. **Define the vocabulary** in `schema.ttl`. Declare the classes and properties that describe the evidence an AI service must produce.

4. **Define the constraints** in `shapes.ttl`. Each SHACL shape encodes a "must" rule derived from the regulation.

5. **Write test cases.** Include at least one conformant evidence graph (passes all shapes) and one non-conformant graph (triggers a specific violation).

6. **Validate locally** using pyshacl to confirm the expected behavior.

7. **Submit a pull request** to the [ek-blocks](https://github.com/Open-Systems-Collective/ek-blocks) repository. Include a clear description of the regulatory source and the obligations encoded.

For a detailed authoring guide, see the [OKB Getting Started](okb/getting-started.md) page.

---

## Contributor Guide

For detailed guidelines on contributing code, documentation, and research artifacts, please see our [Contributor Guide](https://github.com/Open-Systems-Collective/osc-core/blob/main/CONTRIBUTING.md).

---

## Community Discussions

Join the conversation on our [Community Board](https://github.com/Open-Systems-Collective/osc-core/discussions). This is the place to ask questions, propose ideas, and connect with other members.

---

## Contact

For questions or collaboration inquiries, reach out through our [GitHub Organization](https://github.com/Open-Systems-Collective).
