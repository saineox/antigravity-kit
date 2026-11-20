# Syntax & Linting Safety Guidelines

## 1. Quoting Jinja2 Pipes in Conditionals

The vertical bar `|` is a reserved YAML block scalar indicator. If an unquoted conditional contains an inline Jinja pipe filter, YAML parsers will fail:

```yaml
# ❌ INCORRECT (Triggers YAML parse error or scalar crash):
when: datacenter | upper == 'DDC'
failed_when: install.rc != 0 and 'already installed' not in install.stdout | lower

# ✅ CORRECT (Fully quoted string expression):
when: "(datacenter | upper) == 'DDC'"
failed_when: "install.rc != 0 and 'already installed' not in (install.stdout | lower)"
```

## 2. Multi-Distribution Package Modules

Avoid using the generic `ansible.builtin.package` module when specific package flags (like GPG check bypass or specific repository enablement) are needed:

```yaml
# ✅ Safe Red Hat OS family execution:
- name: Install security sensor on RHEL
  ansible.builtin.dnf:
    name: "sec-sensor"
    state: present
    disable_gpg_check: false
  when: ansible_facts['os_family'] == 'RedHat'

# ✅ Safe Debian OS family execution:
- name: Install security sensor on Debian/Ubuntu
  ansible.builtin.apt:
    name: "sec-sensor"
    state: present
    update_cache: yes
  when: ansible_facts['os_family'] == 'Debian'
```

## 3. Automated Static Linting

Run `ansible-lint` against all playbooks and roles before deployment:
```bash
# Lint playbook syntax and best practices
ansible-lint playbooks/site.yml

# Check syntax only
ansible-playbook -i inventory/staging playbooks/site.yml --syntax-check
```
