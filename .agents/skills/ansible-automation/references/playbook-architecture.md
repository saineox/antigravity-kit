# Playbook Architecture & Directory Layout

## Recommended Directory Tree

```plaintext
ansible-project/
├── ansible.cfg
├── inventory/
│   ├── production/
│   │   ├── hosts.yml
│   │   └── group_vars/
│   │       ├── all.yml
│   │       └── webservers.yml
│   └── staging/
├── playbooks/
│   ├── site.yml
│   ├── web.yml
│   └── db.yml
├── roles/
│   └── common/
│       ├── defaults/main.yml
│       ├── handlers/main.yml
│       ├── meta/main.yml
│       ├── tasks/main.yml
│       └── templates/
└── collections/
    └── requirements.yml
```

## Role Modularity Rules

1. **Single Responsibility:** Each role handles one logical service (e.g., `nginx`, `postgresql`, `chrony`).
2. **Default vs Vars:**
   - Put overridable parameters in `roles/<role>/defaults/main.yml`.
   - Put fixed, internal variables in `roles/<role>/vars/main.yml`.
3. **Handlers for State Changes:**
   - Never restart services unless configuration changes occur:
   ```yaml
   - name: Deploy Nginx configuration
     ansible.builtin.template:
       src: nginx.conf.j2
       dest: /etc/nginx/nginx.conf
       owner: root
       group: root
       mode: '0644'
     notify: Reload Nginx
   ```
4. **Collection Requirements:** Declare dependencies strictly in `collections/requirements.yml`:
   ```yaml
   collections:
     - name: ansible.posix
       version: ">=1.5.0"
     - name: community.general
       version: ">=8.0.0"
   ```
