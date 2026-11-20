# Dynamic Facts & Idempotency Enforcement

## 1. Dynamic Fact Initialization vs. Global Vars

In large or multi-stage playbooks, global `vars:` blocks can collide or produce race conditions when interacting with dynamic inventories. Use `ansible.builtin.set_fact` to initialize variables dynamically within the execution flow:

```yaml
# ✅ Safe dynamic variable scoping:
- name: Initialize deployment facts
  ansible.builtin.set_fact:
    target_environment: "{{ env_override | default('staging') }}"
    cache_ttl_seconds: 3600
    deploy_timestamp: "{{ ansible_date_time.iso8601 }}"
```

## 2. Idempotency Verification Rules

Every task must report `ok` (not `changed`) when run against a node that is already in compliance.

### Guiding Principles:
1. **Never use `ansible.builtin.command` or `shell` without state guards:**
   ```yaml
   # ❌ Non-idempotent: Runs on every execution
   - name: Run database migration
     ansible.builtin.command: /opt/app/bin/migrate.sh

   # ✅ Idempotent with creates guard:
   - name: Run database migration
     ansible.builtin.command: /opt/app/bin/migrate.sh
     args:
       creates: /var/log/app/.migration_complete
   ```
2. **Explicit `changed_when` and `failed_when`:**
   ```yaml
   - name: Query service status
     ansible.builtin.command: systemctl is-active custom-daemon
     register: daemon_status
     changed_when: false
     failed_when: daemon_status.rc not in [0, 3]
   ```

## 3. Empirical Dry-Run Testing

Always verify idempotency using dry-run mode:
```bash
# Run 1: Apply changes
ansible-playbook -i inventory/staging playbooks/site.yml

# Run 2: Verify zero changes
ansible-playbook -i inventory/staging playbooks/site.yml --check --diff
# Success criteria: changed=0 failed=0
```
