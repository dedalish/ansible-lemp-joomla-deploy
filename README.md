# Joomla 5.1.4 Automated Deployment with Ansible

Автоматизированное решение для развертывания **Joomla 5.1.4 с MySQL 8.0, Nginx и PHP 8.1-FPM** на Ubuntu/Debian серверах через Ansible.

---

## 📋 Требования

- Ubuntu 20.04+ или Debian 11+
- Ansible 2.10+
- SSH доступ к целевым серверам
- Python 3.8+

---

## Быстрый старт

```bash
# 1. Клонируйте репозиторий
git clone link_
cd dir_

# 2. Установите зависимости коллекций
ansible-galaxy collection install -r requirements.yml

# 3. Отредактируйте инвентарь
vim inventory.yml

# 4. Создайте и зашифруйте переменные c паролями
ansible-vault create vaults/vault.yml

# В ansible.cfg прописан inventory.yml и .vault_pass для сохранения паролей от vault
# Текущий пароль в .vault_pass: 123

# 5. Запустите playbook
ansible-playbook joomla-deploy.yml
```

---

## 📁 Структура проекта

```
final_project_ansible/
├── roles/
│   ├── initapps/
│   ├── joomla/
│   ├── mysql/
│   ├── nginx/
│   └── php/
├── vars/
│   └── vars.yml
├── vaults/
│   ├── .vault_pass
│   └── vault.yml
├── .gitignore
├── ansible.cfg
├── inventory.yml
├── joomla-deploy.yml
├── requirements.yml
└── README.md
```

---

## 🧪 Проверка и тестирование

- Линтер: ansible-lint joomla-deploy.yml

---


## 🔄 Переменные и vault

**Пример vars/vars.yml:**
```yaml
php_version: "8.1"
initapps_joomla_database_password: "{{ mysql_admin_password }}"
mysql_root_pass: "{{ mysql_root_password }}"
```

**Пример vaults/vault.yml (создаётся через ansible-vault):**
```yaml
vault_mysql_root_pass: "your_secure_password"
vault_initapps_joomla_database_password: "your_db_password"
```

---

## 📝 joomla-deploy.yml (playbook)

```yaml
- hosts: web01
  become: true
  vars_files:
    - vars/vars.yml
    - vaults/vault.yml
  roles:
    - mysql
    - php
    - nginx
    - joomla
    - initapps
```
