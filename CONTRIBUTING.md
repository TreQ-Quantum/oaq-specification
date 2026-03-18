# Contributing to OAQ

We're excited that you want to contribute to the Open Architecture Quantum (OAQ) specification. Contributions of all kinds are welcome — from fixing typos to proposing concepts and features.

## Ways to Contribute

### Reporting Issues

If you find an ambiguity, inconsistency, or gap in the specification, please [open a GitHub issue](../../issues). Good issues include:

- A clear description of the problem or ambiguity
- The relevant section(s) of the specification
- A proposed resolution if you have one

### Proposing Spec Changes

Changes to the normative specification (new objects, modified fields, serialization changes) require discussion before a pull request is opened. Please open an issue describing the motivation and proposed change and allow time for community discussion.

### Adding or Improving Examples

New examples that demonstrate real quantum workflows are very welcome. Examples should:

- Be placed in `oaq_specification/examples/` and follow the existing naming convention
- Include a JSON job definition, a brief explanation, and ideally a Mermaid timing diagram
- Be added to the index in `oaq_specification/examples/README.md` and `oaq_specification/docs/README.md`

### Improving Documentation

Prose improvements, clearer explanations, and better cross-references are always appreciated. No issue is required for purely editorial changes — a pull request is sufficient.

## Pull Request Process

1. Fork the repository and create a branch from `main`
2. Make your changes, following the documentation style guidelines below
3. Open a pull request with a clear description of what was changed and why
4. All spec changes must reference an open issue in the PR description

## Documentation Style

- Use [GitHub-flavored Markdown](https://github.github.com/gfm/)
- Field reference tables should follow the format used in existing docs:

  ```markdown
  | name | type | description |
  |------|------|-------------|
  | FieldName | Type | Description of the field. |
  ```

- Use `!!! note` callouts for normative constraints:

  ```markdown
  !!! note
    A valid implementation must satisfy this constraint.
  ```

- Keep descriptions concise; avoid restating what is already evident from the type

## Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/). By participating, you agree to uphold it. Please report unacceptable behavior to the project maintainers.
