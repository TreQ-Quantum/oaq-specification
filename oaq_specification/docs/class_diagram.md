# Class Diagram

The OAQ specification is formally defined as a set of UML class diagrams. These diagrams are the authoritative definition of the specification — the reference documentation and serialization formats are derived from them.

## Job ASG

The diagram below shows the full class hierarchy for a job submission. Use it to understand inheritance relationships, field types, and how the different parts of the specification connect.

![OAQ Job class diagram](http://raw.githubusercontent.com/TreQ-Quantum/oaq-specification/refs/heads/main/oaq_specification/uml/Job/job_uml.svg)

> **Note:** If the diagram is too small to read, open the raw SVG file at [`oaq_specification/uml/Job/job_uml.svg`](http://raw.githubusercontent.com/TreQ-Quantum/oaq-specification/refs/heads/main/oaq_specification/uml/Job/job_uml.svg) for a scalable view.

## Results

The results structure returned by a backend after a successful job run.

![OAQ Results class diagram](http://raw.githubusercontent.com/TreQ-Quantum/oaq-specification/refs/heads/main/oaq_specification/uml/Results/results_uml.svg)

## Reading the Diagrams

- **Abstract classes** are shown in italics or with an `«abstract»` stereotype
- **Inheritance** is shown with a hollow-headed arrow pointing from subtype to supertype
- **Composition** is shown with a filled diamond — the composed object cannot exist independently of its owner
- **Multiplicity** annotations (e.g. `0..*`, `1`) appear on association lines

For a textual summary of the inheritance hierarchy, see [Base Types](./base_types.md).
