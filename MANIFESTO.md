# The Constitutional Governance Manifesto

## The problem

Technical organizations govern themselves by accident.

Most engineering teams have conventions. Few have written them down in any durable way. Rules live in wikis that fall out of date, in onboarding docs that get read once, in the memory of whoever's been around the longest. When someone new generates something that violates an unwritten rule, it gets caught by whoever happens to remember it. Or it doesn't.

This is governance by culture. It doesn't survive team growth. It doesn't hold through organizational change. It breaks entirely when agents are generating half your codebase and every session starts fresh with no memory of what came before.

Infrastructure solved a version of this problem. Configuration used to live in runbooks and tribal knowledge. Then it became code, version-controlled and enforced through the same pipelines as everything else. Governance is still where infrastructure was ten years ago.

Constitutional Governance is the methodology for making that same transition.

---

## What we believe

Rules that govern a technical organization matter as much as the code it produces. They should live in a repository, not a wiki. They should be reviewable and testable, and when they're wrong, they should fail a check rather than slip through unnoticed.

AI agents need governance knowledge in a structured, queryable form. Prose documentation doesn't work for them. Every session starts cold, with no memory of last week's decisions. A rule they can query is a rule they can follow.

Governance defined in one place and delegated everywhere stays coherent. Governance copied and localized by every team is drift waiting to happen.

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
If it can't be tested and enforced, it's a suggestion. Every rule needs to exist as a file in a repository and go through the same change process as production code.

**2. One source of truth.**
No team should own a copy of the rules. Every team delegates rule authority to a single governance repository. When a rule changes, it changes once. Distributed copies are technical debt and they compound.

**3. Rationale is not optional.**
A rule without an explanation is a rule that gets broken the moment the person who wrote it isn't around. Every decision needs to document not just what was decided but why, and what alternatives were on the table. That's the part that survives contact with an edge case.

**4. AI agents are first-class governance consumers.**
Governance systems built for human readers will fail as AI agents become primary contributors. Rules need to be structured and queryable by machines. An agent that can ask "is this name valid?" and get a structured answer is a governed agent. One that has to interpret prose documentation isn't.

**5. Enforcement is layered.**
No single gate catches every violation at the right time. Governance needs to run before the agent generates, after it generates, at commit time, and before the merge. Each point in that chain has a different cost of failure. Earlier is cheaper.

**6. Constitutions constrain laws, laws constrain implementations.**
Not all rules carry equal authority. Principles explain the reasoning behind specific rules. Specific rules constrain the implementations that follow them. None of this works if the hierarchy stays implicit.

**7. Governance must be discoverable.**
A rule that can't be found doesn't exist. Anyone should be able to discover what's governed and why, without already knowing where to look.

**8. Compliance must be verifiable.**
Rules without tests are aspirations. Every governance rule needs an executable check that runs in CI on every proposed change. A rule whose check fails blocks the change.

**9. Governance evolves through amendment, not erasure.**
Rules change. When they do, the old decision gets superseded and the reason for the change gets recorded alongside it. Silent replacement destroys institutional memory.

**10. The overhead of governance must be lower than the cost of its absence.**
A governance system that slows teams down will be bypassed. Constitutional Governance needs to make following the rules the faster path. Not the correct path. The fast one. If it isn't, teams will find a way around it.

---

## What this is

**Nomos** is the open-source reference implementation. It exposes governance rules as MCP tools so AI agents can query them at generation time. The same server handles REST validation for CI pipelines and CLI calls for pre-commit hooks.

The methodology is the standard. Nomos is one implementation of it.

If your organization governs itself by memory, this is the alternative.

---

*Constitutional Governance is an open methodology. Contributions, critiques, and implementations in other stacks are welcome.*

---

**Author:** Jesus Rodriguez
**Version:** 1.0
**License:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
