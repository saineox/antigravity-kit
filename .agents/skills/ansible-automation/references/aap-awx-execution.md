# AWX & Red Hat AAP Execution Patterns

## 1. Controller Architecture

Red Hat Ansible Automation Platform (AAP) and AWX execute playbooks inside isolated Execution Environments (containerized runtimes with pinned python packages, ansible-core, and collections).

### Key Components:
- **Project:** Git repository containing playbooks and roles.
- **Inventory:** Dynamic cloud inventories, Satellite inventories, or static YAML sources.
- **Job Template:** The runnable unit linking a Project, Playbook, Inventory, and Credentials.
- **Execution Environment (EE):** Container image built with `ansible-builder`.

## 2. Best Practices for Controller Automation

1. **Parameterize via Surveys:** Expose runtime variables (e.g., target version, dry-run toggle) through AWX surveys with strict input regex validation.
2. **Handle Sensitive Credentials via Machine Credential Objects:**
   - Never embed SSH private keys, vault passwords, or cloud tokens in git repositories.
   - Use AAP/AWX managed credentials, injected securely into environment variables or temp files at run time.
3. **Webhook Integration:** Trigger AAP/AWX Job Templates automatically via Git push events (GitLab / GitHub / Bitbucket webhooks) for automated drift correction.
4. **Error Handling & Notification:**
   - Attach AWX Slack/Teams/Email notification templates on failure.
   - Keep stdout clean and parseable by configuring standard log formatters in `ansible.cfg`.
