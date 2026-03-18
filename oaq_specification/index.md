# Open Architecture Quantum (OAQ) Specification for Low-level Jobs

[![Version](https://img.shields.io/badge/version-0.1.0-blue)](../oaq_specification/docs/job_serialization.md)
[![License](https://img.shields.io/badge/license-Apache%202.0-green)](../LICENSE)


The OAQ specification is as a system-level interface (as opposed to a user-level "language") for low-level jobs, composed of pulse schedules and additional logic.

This open specification

* Expresses low-level quantum jobs as an **abstract syntax graph** (ASG) together with rules for static analysis.
* Defines a contract between users and quantum backend providers about:
  * how pulse schedules and control logic are expressed,
  * what inputs are accepted,
  * what outputs are returned, and
  * how errors are handled.
* Enables portability of software solutions through a vendor-neutral interface that compiler writers can target and backend providers can implement.

## Getting Started

The recommended reading order for new users:

1. **[Examples](./examples/)** — worked examples from simple single-pulse jobs through to multiplexed readout and active reset
2. **[UML class diagram](./uml/Job/)** — the formal definition of the specification as a class hierarchy. Start here to understand the shape of the ASG
3. **[Reference documentation](./docs/)** — descriptions of each object, field, and serialization format

## Documentation

| Section | Description |
|---------|-------------|
| [UML / Formal Definition](./uml/) | Class diagrams defining the full ASG structure for Job and Results |
| [Reference Docs](./docs/) | Reference and descriptions for all elements of the specification |
| [Examples](./examples/) | Annotated example jobs covering simple cases and common quantum workflows |

### Scope

The specification can be thought of as a contract between a user and a backend provider about how a low-level job (pulse schedule and additional logic) is expressed, including allowed inputs, expected outputs, and error handling. Objectives include:

- Capturing higher-level abstractions that allow users to be more expressive and retain contextual information for the backend
- Covering workflows for multiple modalities, starting with superconducting and trapped ions
- Maintaining stack vendor agnosticism
- Supporting on-control hardware logic and advanced features where reasonable
- Being verifiable against a target backend
- Having specified error handling
- Allowing for reproducible job definitions
- Being serializable to and from at least a binary format and JSON
- Aiming for memory compactness

### Not in Scope

- Classical instructions for hybrid programs
- Data fitting and analysis
- Support for backends below a minimum level of functionality

### Backend Providers

A backend provider is expected to publish documentation describing how users should tailor a job for their system, including the list of valid ports and their physical mappings, channel sample rates, allowed instructions, and any further hardware constraints.

Backend providers may choose not to expose all native capabilities — for example, "set once and forget" attenuations or operations that are unsafe to expose to end users. Users may assume that the backend provider has established appropriate configurations for such operations.

## Origins

OAQ grew out of an UK Quantum Missions Pilot Project funded by [Innovate UK (IUK)](https://www.ukri.org/councils/innovate-uk/) to develop the open architecture quantum testbed for evaluating component and interface solutions.
The specification was developed collaboratively with a consortium consisting of:
- Q-CTRL,
- Qruise,
- Rigetti, and
- Oxford Ionics.

It was built with the goal of becoming an open standard to benefit the broader community.

## License

Apache 2.0
