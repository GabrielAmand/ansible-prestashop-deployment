# Ansible Prestashop Deployment

## Objective

This project aims to automate the deployment of a Prestashop e-commerce application using Ansible.

The infrastructure includes:
- A web server (Apache + PHP)
- A MySQL database
- A complete automation using Ansible roles

---

## Stack

- Ansible
- Apache
- PHP
- MySQL
- Prestashop

---

## Project Structure

```
.
├── inventories/
│ └── dev/
│ ├── hosts.yml
│ └── group_vars/
├── playbooks/
│ ├── site.yml
│ └── test.yml
├── roles/
│ ├── mysql/
│ └── prestashop/
├── logs/
└── README.md

```

- `inventories/`: defines hosts and variables
- `playbooks/site.yml`: deploys the full stack
- `playbooks/test.yml`: validates the deployment
- `roles/mysql`: installs and configures MySQL
- `roles/prestashop`: installs and configures Prestashop
- `logs/`: contains deployment and test outputs
- `group_vars/`: stores environment variables (including encrypted secrets with Ansible Vault)


---
## Workflow

1. Setup environment (download Prestashop + configure Vault)
2. Deploy infrastructure
3. Run validation tests

---

## Prerequisites

⚠️ This project installs services directly on the host machine.

It is strongly recommended to run it on:
- a virtual machine
- or a dedicated test environment

---

## Ansible Vault

Secrets (database credentials) are stored using Ansible Vault.

To run the project, you will be prompted for the vault password.

For demonstration purposes, a simple password can be used.

In a production environment:
- use a strong password
- store it securely (password manager, CI/CD secrets, etc.)

---
## Setup

⚠️ You must complete all setup steps before running the playbooks.
Before running the project, a few manual steps are required.

### 1. Download Prestashop

Download the Prestashop archive from the official website:

https://www.prestashop.com/en/download

Place the archive in:

```
roles/prestashop/files/
```


---

### 2. Configure Ansible Vault

Create a Vault file to store database credentials:

```
ansible-vault create inventories/dev/group_vars/all/vault.yml
```

Then add the following variables:
```
vault_mysql_root_password: "your_root_password"
vault_mysql_user_password: "your_user_password"
```

You will be asked to define a Vault password.

⚠️ This password will be required each time you run the playbooks.

---
## How to Run

⚠️ You will be prompted for your sudo password during execution.

### Deploy the infrastructure

```
ansible-playbook playbooks/site.yml -K --ask-vault-pass
```

### Run validation tests

```
ansible-playbook playbooks/test.yml -K --ask-vault-pass
```


## Database Access

Although the exercise suggests using the root user, this project creates a dedicated MySQL user (`prestashop_user`).

This approach is more secure and closer to real-world practices.

---

## Future Improvements

- Containerization with Docker
- Deployment with Kubernetes
- CI/CD integration
- Use of a dedicated MySQL admin user instead of root
- Dynamic download of Prestashop instead of manual archive

---