# Contributing to Constitutional Governance

Constitutional Governance is an open methodology. Contributions are welcome in several forms: critiques of the principles, new examples, implementations in different technology stacks, and improvements to the language of the manifesto and principles.

---

## What you can contribute

### Critiques and amendments

If you believe a principle is wrong, incomplete, or in tension with another principle, open an issue. Describe:

- Which principle you are questioning
- What the specific problem is
- What you would change or add

Constitutional Governance principles should be stable but not sacred. They evolve through amendment — the history of what was decided and why is preserved.

### New examples

Domain examples show how the methodology applies in a specific technical context. A good example includes:

- A governance repository structure
- A domain constitution
- At least one ADR
- At least one Gherkin feature file with `@enforced` and `@wip` scenarios
- A short narrative showing what the AI agent and CI experiences look like

Open a pull request with a new file in `examples/`. The file name should be the domain (e.g., `examples/graphql-api.md`, `examples/terraform-modules.md`).

### New implementations

An implementation is a tool that operationalizes Constitutional Governance. To add one:

1. Create a file in `implementations/` describing the tool
2. Include: what it provides, how it exposes governance rules, and where to find it
3. Open a pull request

Implementations do not need to be open-source to be listed. They need to implement the methodology — single source of truth, machine-readable rules, layered enforcement.

### Adopters

If your organization has adopted Constitutional Governance, add an entry to `ADOPTERS.md`. You do not need to open-source your governance repository.

---

## What this repository is not for

- Feature requests for Nomos (open issues on the Nomos repository)
- Organization-specific governance rules (those belong in your governance repository)
- Tooling comparisons or vendor evaluations

---

## Pull request guidelines

- Keep pull requests focused. One change per PR.
- For principle amendments: include a clear statement of what is changing and why. The PR description is the amendment record.
- For new examples: examples should be realistic but anonymized. Do not include real organization names, internal URLs, or proprietary naming conventions unless you have permission to publish them.
- For implementations: do not claim completeness unless the implementation covers all four enforcement layers. Note which layers are supported.

---

## Discussions

Use GitHub Discussions for:

- Questions about applying the methodology
- Case studies and experience reports
- Proposals for new principles before opening a PR
- Debate about the methodology itself

Discussions are the legislative process. Pull requests are the ratified amendments.

---

## License

By contributing to this repository, you agree that your contributions are licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). You retain authorship. You grant anyone the right to share, adapt, and build on your contribution with attribution.
