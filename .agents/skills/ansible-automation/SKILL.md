---
name: ansible-automation
description: Enterprise Ansible automation, playbook design, role modularization, idempotency enforcement, and AWX/AAP execution.
when_to_use: "Use when authoring or debugging Ansible playbooks, designing reusable roles, managing inventory, ensuring syntax and Jinja2 quoting safety, validating idempotency (--check --diff), or executing automation via AWX / Red Hat Ansible Automation Platform."
allowed-tools: Read, Grep, Glob, Bash
version: 1.0.0
---

# Ansible Automation

> Enterprise-grade Infrastructure as Code (IaC) and system configuration automation.

---

## Overview

This skill guides the design, development, linting, and execution of production-ready Ansible automation across multi-distribution Linux environments (RHEL, CentOS, Debian, Ubuntu) and enterprise execution controllers (AWX, Red Hat Ansible Automation Platform).

---

## Core Engineering Principles

1. **Strict Idempotency:** Every task must converge to the desired state. Running a playbook multiple times must result in `changed=0` on subsequent runs.
2. **Explicit Fact Management:** Avoid top-level `vars:` collisions in complex playbooks. Use `ansible.builtin.set_fact` to dynamically initialize task facts.
3. **YAML & Jinja2 Safety:** Always quote Jinja2 expressions in `when`, `failed_when`, and `changed_when` clauses to prevent YAML parser misinterpretation of reserved characters (e.g., `|`).
4. **Declarative OS Modularity:** Use native package managers (`ansible.builtin.dnf`/`yum` vs `ansible.builtin.apt`) with conditional OS family dispatch rather than generic wrappers.
5. **Collection FQCN:** Always use Fully Qualified Collection Names (FQCN), such as `ansible.builtin.copy` or `ansible.posix.firewalld`.

---

## 5-Phase Playbook Development Workflow

```
[ Phase 1: Intake & Target Scoping ]
             │
             ▼
[ Phase 2: Role & Directory Architecture ]
             │
             ▼
[ Phase 3: Defensive Task Construction & Jinja Quoting ]
             │
             ▼
[ Phase 4: Static Linting & Dry-Run Verification (--check --diff) ]
             │
             ▼
[ Phase 5: Controller Execution (AWX / AAP Job Templates) ]
```

---

## Reference Guides

- **Playbook & Role Layout:** Read [references/playbook-architecture.md](references/playbook-architecture.md) for standard directory trees, collections, and role modularity.
- **Syntax, Jinja & Linting:** Read [references/syntax-and-linting.md](references/syntax-and-linting.md) for conditional quoting rules, block scalars, and `ansible-lint` compliance.
- **Dynamic Facts & Idempotency:** Read [references/dynamic-facts-and-idempotency.md](references/dynamic-facts-and-idempotency.md) for `set_fact` scoping and `--check --diff` patterns.
- **AWX & AAP Execution:** Read [references/aap-awx-execution.md](references/aap-awx-execution.md) for job templates, surveys, credential types, and execution environments.
