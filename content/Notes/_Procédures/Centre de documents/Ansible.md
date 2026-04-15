---
base: "[[_Centre de documents.base]]"
Catégorie:
  - Déploiement
---

<!-- Column 1 -->
## 🔎 1. C’est quoi Ansible ?

**Ansible** est un outil d’automatisation :

- Configuration management
- Déploiement applicatif
- Orchestration
- Provisioning

✔ Agentless (SSH)

✔ Basé sur YAML

✔ Idempotent (rejouable sans casser)

# 📁 3. Structure de Projet Recommandée

```plain text
ansible-project/
│
├── inventory/
│   ├── hosts.yml
│
├── group_vars/
│   ├── all.yml
│   ├── web.yml
│
├── host_vars/
│   ├── server1.yml
│
├── roles/
│   ├── nginx/
│   │   ├── defaults/
│   │   │   └── main.yml
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   ├── handlers/
│   │   │   └── main.yml
│   │   ├── templates/
│   │   │   └── nginx.conf.j2
│   │   ├── files/
│   │   └── vars/
│
├── playbook.yml
└── ansible.cfg
```

<!-- Column 2 -->
# 🏗 2. Architecture Type

## 📌 Composants

### 🖥 Control Node

- Machine où Ansible est installé
- Contient :
    - Playbooks
    - Inventory
    - Roles
- Se connecte en SSH

### 🖥 Managed Nodes

- Serveurs cibles
- Pas d’agent requis
- Python installé

### 📂 Inventory

Liste des hôtes gérés.

# 📜 4. Inventory

### YAML (recommandé)

```plain text
all:
  children:
    web:
      hosts:
        web1:
          ansible_host: 192.168.1.10
        web2:
          ansible_host: 192.168.1.11

    db:
      hosts:
        db1:
          ansible_host: 192.168.1.20
```

<!-- Column 1 -->
# ▶ 5. Playbook Minimal

```plain text
- name: Installer nginx
  hosts: web
  become: yes

  tasks:
    - name: Installer nginx
      apt:
        name: nginx
        state: present
```

Lancer :

```plain text
ansible-playbook-i inventory/hosts.yml playbook.yml
```

# 🔁 7. Conditions (when)

Oui, Ansible supporte les conditions.

```plain text
- name: Installer Apache sur Debian
  apt:
    name: apache2
    state: present
  when: ansible_os_family == "Debian"
```

Avec variable :

```plain text
when: install_webserver is defined and install_webserver == true
```

# 🧠 9. Variables

## Définition

Dans `group_vars/web.yml` :

```plain text
http_port: 80
```

Utilisation :

```plain text
- debug:
    msg:"Le port est {{ http_port }}"
```

<!-- Column 2 -->
# 🎭 6. Les Roles

Un rôle = module structuré réutilisable.

Dans `roles/nginx/tasks/main.yml` :

```plain text
- name: Installer nginx
  apt:
    name: nginx
    state: present
```

Dans `playbook.yml` :

```plain text
- hosts: web
  roles:
    - nginx
```

# 🔄 8. Boucles (loop)

### Boucle simple

```plain text
- name: Installer plusieurs paquets
  apt:
    name:"{{ item }}"
    state: present
  with_items:
    - git
    - curl
    - vim
```

### Boucle avec dictionnaire

```plain text
- name: Créer utilisateurs
  user:
    name:"{{ item.name }}"
    shell:"{{ item.shell }}"
  loop:
    - { name:"dev1", shell:"/bin/bash" }
    - { name:"dev2", shell:"/bin/zsh" }
```


<!-- Column 1 -->
# 🧩 10. Templates Jinja2

Ansible utilise **Jinja**.

## 📄 Template (`nginx.conf.j2`)

```plain text
server {
    listen {{ http_port }};
    server_name {{ domain_name }};

    location / {
        proxy_pass http://{{ backend_ip }};
    }
}
```

## 📜 Task

```plain text
- name: Déployer config nginx
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/sites-available/default
  notify: Restart nginx
```


<!-- Column 2 -->
# 🔔 11. Handlers

`roles/nginx/handlers/main.yml`

```plain text
- name: Restart nginx
  service:
    name: nginx
    state: restarted
```

Appelé uniquement si changement 👍

---

# 📦 12. Modules Essentiels

| Module | Usage |
| --- | --- |
| apt / yum | Gestion paquets |
| service | Services |
| copy | Copier fichier |
| template | Template Jinja |
| file | Permissions |
| user | Gestion utilisateurs |
| command | Commande simple |
| shell | Commande shell |


---

---

# 🛠 13. Commandes Utiles

### Test ping

```plain text
ansible all-mping-i inventory/hosts.yml
```

### Dry-run

```plain text
ansible-playbook playbook.yml--check
```

### Diff

```plain text
ansible-playbook playbook.yml--diff
```

---

# 🎯 14. Bonnes Pratiques

✔ Utiliser des roles

✔ Séparer variables / tasks

✔ Utiliser `--check`

✔ Versionner avec Git

✔ Utiliser Ansible Vault pour secrets

---

# 🔐 15. Ansible Vault

```plain text
ansible-vault create secrets.yml
ansible-vault edit secrets.yml
ansible-vault encrypt secrets.yml
```

Dans playbook :

```plain text
ansible-playbook playbook.yml--ask-vault-pass
```

---

# 🚀 16. Workflow Type

1. Écrire inventory
2. Créer role
3. Ajouter variables
4. Tester en `-check`
5. Déployer

---

# 🎓 Résumé Ultra Rapide

| Concept | Mot clé |
| --- | --- |
| Cible | hosts |
| Action | tasks |
| Logique | when |
| Boucle | loop |
| Réutilisable | roles |
| Config dynamique | template |
| Redémarrage | handlers |