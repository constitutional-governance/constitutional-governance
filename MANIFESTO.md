# The Constitutional Governance Manifesto

## The problem

Technical organizations govern themselves by accident.

Most engineering teams have conventions. Few have made them explicit. Fewer still have made them executable. Rules live in wikis that nobody updates, in onboarding documents nobody reads after the first week, and in the heads of senior engineers who eventually leave. When a new team member — human or artificial — joins and generates something that violates an unwritten rule, the violation is caught by a human reviewer who happens to remember, or it is not caught at all.

This is governance by culture, not by design. It does not scale with team size. It does not survive organizational change. And it is fundamentally incompatible with AI-assisted engineering, where agents generate code at a rate that outpaces any human review process and carry no organizational memory between sessions.

The software industry has already solved analogous problems in adjacent domains. Infrastructure is no longer configured by hand — it is declared in code, reviewed through pull requests, and applied through automated pipelines. Security is no longer enforced at the perimeter — it is embedded in the delivery pipeline. Observability is no longer an afterthought — it is infrastructure that runs continuously and is queried on demand.

Governance has not made this transition. It remains the last domain managed primarily through documentation and human memory. Constitutional Governance is the methodology for changing that.

---

## The declaration

We hold that the rules by which a technical organization governs itself are as important as the code it produces, and must be treated with the same rigor: written down, version-controlled, reviewed, tested, and enforced automatically.

We hold that AI agents are first-class members of engineering organizations and must have access to governance knowledge in the same structured, queryable form as any other tool.

We hold that governance should be delegated — defined once, enforced everywhere — not distributed in copies that drift and decay.

---

## We value

- **Codified rules** over documented conventions
- **Delegated authority** over distributed copies
- **Automatic enforcement** over manual review
- **Structured rationale** over implicit knowledge
- **Machine-readable governance** over human-only interpretation

As with the Agile Manifesto: the items on the right have value. We value the items on the left more.

---

## The principles

**1. Governance is code.**
Rules that cannot be version-controlled, reviewed, and tested are not governance — they are suggestions. Every convention, standard, and architectural decision must exist as a file in a repository, subject to the same change process as production code.

**2. One source of truth.**
No team should own a copy of the rules. Every team delegates rule authority to a single governance repository. When a rule changes, it changes once and propagates everywhere. Distributed copies are technical debt.

**3. Rationale is not optional.**
A rule without explanation is a rule that will be broken the moment the person who wrote it is unavailable. Every decision must document what was decided, why it was decided, what alternatives were rejected, and what the consequences of the decision are. Architecture Decision Records are the legislative history of the organization.

**4. AI agents are first-class governance consumers.**
Governance systems that assume a human reader will fail as AI agents become primary contributors to codebases. Rules must be structured, typed, and queryable by machines. An agent that can ask "is this name valid?" and receive a structured answer is a governed agent. An agent that must interpret prose documentation is ungoverned.

**5. Enforcement is layered, not singular.**
No single gate catches every violation at the right moment. Governance must be enforced at guidance time (before generation), validation time (during generation), commit time (before sharing), and integration time (before merging). Each layer has a different cost of failure; the earlier a violation is caught, the cheaper it is to fix.

**6. Constitutions constrain laws, laws constrain implementations.**
Not all rules have equal authority. Principles — why the organization values consistency, why it manages access the way it does — constrain and explain the specific rules derived from them. Specific rules constrain and explain the implementations that must follow them. This hierarchy must be explicit.

**7. Governance must be discoverable.**
A rule that cannot be found is a rule that does not exist. Any agent, tool, or team member must be able to enumerate what domains are governed, what rules apply to each domain, and what the rationale behind each rule is — without prior knowledge of the governance repository's structure.

**8. Compliance must be verifiable.**
Rules without tests are aspirations. Every governance rule must have at least one executable check that verifies correct implementation and at least one that verifies incorrect implementation is rejected. These checks run in CI on every proposed change. A rule whose check fails blocks the change.

**9. Governance evolves through amendment, not erasure.**
Rules change. When they do, the history of what was decided and why must be preserved. A rule is amended — the old decision is superseded, the reason for supersession is recorded — never silently replaced. An organization that cannot explain why its current rules differ from its former rules has lost institutional memory.

**10. The overhead of governance must be lower than the cost of its absence.**
A governance system that slows teams down will be bypassed. Constitutional Governance succeeds only if querying the rules is faster than guessing, validating a name is faster than having it rejected in review, and finding the rationale for a decision is faster than reconstructing it from git history. The system must earn its adoption continuously.

---

## The call

We are not the first to recognize that technical organizations need better governance. We are proposing a specific, implementable methodology — one that treats governance as infrastructure, delegation as the organizational primitive, and AI agents as primary consumers of machine-readable rules.

**Nomos** is the open-source reference implementation: a governance server that operationalizes Constitutional Governance for engineering platforms, exposing rules as MCP tools queryable by AI agents, as a CLI for pre-commit hooks, and as executable Gherkin checks for CI pipelines.

The methodology is the standard. The implementation is one path to it.

If your organization governs itself by memory, we invite you to govern it by design.

---

*Constitutional Governance is an open methodology. Contributions, critiques, and implementations in other technology stacks are welcome.*

---

**Authors:** [Your name / org]
**Version:** 1.0
**License:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
