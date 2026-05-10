# Nomos — Reference Implementation

**Nomos** is the open-source governance server that operationalizes Constitutional Governance for engineering platforms.

It provides three interfaces to the same governance rules:

| Interface | Use case |
|---|---|
| **MCP tools** | AI agents query rules before and during code generation |
| **CLI** | Pre-commit hooks validate local changes |
| **Gherkin suite** | CI pipeline verifies compliance on every pull request |

---

## What Nomos does

Nomos reads a governance repository — a directory containing a `governance.yml`, constitutions, ADRs, validators, and Gherkin feature files — and exposes its rules as queryable, executable tools.

**For AI agents:**

```
list_constitutions()        → ["global", "kafka", "camel", "springboot"]
get_constitution("kafka")   → {domain, content, version}
list_adrs()                 → [{id, title, status}, ...]
get_adr("001")              → {id, title, status, content}
validate_topic_name("...")  → {valid, reason, pattern}
validate_rbac_entry({...})  → {valid, violations}
get_kafka_conventions()     → {valid_prefixes, valid_roles, prefix_semantics, ...}
list_check_domains()        → ["kafka", "camel", "springboot"]
get_checks("kafka")         → [{title, status, path}, ...]
get_active_rules()          → full governance.yml as structured object
```

**For pre-commit hooks and team onboarding:**

```bash
# Install .mcp.json + pre-commit hook in a project repo (one command)
nomos install-hooks --server https://governance.yourcompany.com

# Validate a resource against the shared server
nomos-validate --server https://governance.yourcompany.com topic "payments.processed.v1"
nomos-validate --server https://governance.yourcompany.com sa "sa-payments-connector-source-jdbc-prod"
```

**For governance authoring:**

```bash
# Scaffold a new domain (constitution + ADR + Gherkin template)
nomos scaffold domain kafka

# Scaffold a team governance directory
nomos scaffold team team-pos

# Connect a team's agent to team-scoped rules
nomos install-hooks --server https://governance.acme.com --team team-pos

# Verify a @wip scenario is ready to promote to @enforced
nomos check-promotion features/kafka/topic-naming.feature --run
```

**For CI:**

```bash
behave features/ --tags=enforced
```

---

## Architecture

```mermaid
flowchart LR
    GY["governance.yml"]
    MD["constitutions\nand ADRs"]
    GK["Gherkin\nfeatures"]

    NS["Nomos Server"]

    MCP["MCP tools"]
    CLI["CLI validator"]
    BDD["Gherkin runner"]

    AG["AI Agents"]
    PC["Pre-commit hooks"]
    CICD["CI Pipeline"]

    GY --> NS
    MD --> NS
    GK --> NS

    NS --> MCP
    NS --> CLI
    NS --> BDD

    MCP --> AG
    CLI --> PC
    BDD --> CICD
```

```
governance repo/
├── governance.yml          ← single config driving all validators
├── constitution.md         ← global principles
├── constitutions/
│   ├── kafka.md
│   ├── camel.md
│   └── springboot.md
├── adrs/
│   ├── global/
│   │   ├── 001-topic-naming.md
│   │   └── 002-consumer-groups.md
│   └── kafka/
├── features/
│   ├── kafka/
│   │   ├── topic-naming.feature
│   │   └── rbac-rules.feature
│   └── springboot/
└── conventions/
    └── helm/
```

Nomos mounts this repo at startup and serves its rules. The governance repo is the authority — Nomos is the transport.

---

## The delegation model

Teams do not run their own governance server. They point their tools and agents at the shared governance server operated by the platform team:

```json
{
  "mcpServers": {
    "nomos": {
      "type": "http",
      "url": "https://governance.yourcompany.com/mcp"
    }
  }
}
```

When a rule changes in the governance repo, the GitHub webhook fires, the server invalidates its cache, and every agent that queries it gets the updated rule immediately. No redistribution. No stale copies.

```mermaid
flowchart LR
    GR["Governance Repository"]
    NS["Nomos Server\none shared instance"]
    A1["Team A — AI agent"]
    A2["Team B — AI agent"]
    CI["CI pipelines"]
    PC["Pre-commit hooks"]

    GR -->|"push → webhook"| NS
    NS --> A1
    NS --> A2
    NS --> CI
    NS --> PC
```

---

## Getting started

**Server** (install once, operated by the platform team):

```bash
pip install nomos
nomos --github https://github.com/your-org/your-governance-repo
```

**Governance template** (fork once per organization, owned by the platform team):

→ [nomos-template](https://github.com/your-org/nomos-template) — governance repository starter with constitutions, ADRs, Gherkin checks, and adoption guides

**For product teams** connecting to an existing Nomos instance, see:

→ [nomos-template/FOR-TEAMS.md](https://github.com/your-org/nomos-template/blob/main/FOR-TEAMS.md)

---

## Status

Nomos is in active development. The core MCP interface, CLI validators, REST endpoints, and Gherkin runner are functional.

→ [GitHub repository](https://github.com/your-org/nomos) *(update this link)*

---

## Other implementations

Constitutional Governance does not require Nomos. The methodology can be implemented with:

- **OPA (Open Policy Agent)** — for teams already invested in the OPA ecosystem
- **Custom scripts** — a shell script that validates names against a YAML config is a valid Constitutional Governance implementation
- **Backstage plugins** — for organizations using Backstage as the developer portal
- **Any tool that exposes governance rules as queryable, machine-readable APIs**

The standard is the methodology. Nomos is one path to it.

---

→ [Back to README](../README.md)
