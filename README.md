# Ansible Playbook: Ansible-argocd

## Description

- Install ArgoCD and its image updater service
```bash
ansible-vault encrypt inventory/group_vars/argocd.yml
ansible-playbook -i inventory/local.ini playbook/provision.yml --ask-vault-pass
```

- Remove ArgoCD
```bash
ansible-vault encrypt inventory/group_vars/argocd.yml
ansible-playbook -i inventory/local.ini playbook/destroy.yml --ask-vault-pass
```

## Requirements

- Python3 and pip installed on your machine

## Linting and syntax-check

You can run yamllint to lint your yaml via

```shell
yamllint .
```

You can run syntax-check to verify the playbook's imports via
```shell
ansible-playbook playbook/playbook.yml --syntax-check
```

## Configuration

- **Inventory File**:
- `inventory/local.ini`
  Used for installing ArgoCD
```ini
[argocd]
localhost
```
- **Variables**: `inventory/group_vars.yml`
  - `argocd.yml` - This file is encrypted, used for ArgoCD playbook, example config below.
  ```yaml
  ---
  # needed to tell ansible we are not ssh'ing and just running locally
  ansible_connection: local
  
  # variables
  ingress_subdomain: "argo.local.example.com"
  argo_cd_password: "password123"
  ```
- **Playbooks**: `playbook/*.yml`
  - `provision.yml` is the playbook to run to add ArgoCD.
  - `destroy.yml` is the playbook to run to remove ArgoCD.
- **roles**: `roles/{rolename}`
  - argocd_prereq- Role to run before installing ArgoCD. Installs Brew, Helm, ArgoCD
  - argocd- Role to run to install ArgoCD
  - reset- Role to destroy argocd 

## Extra config

### Allow kubectl from local machine
```shell
mkdir -p ~/.kube
scp kazzam@192.168.1.6:~/.kube/config ~/.kube/config
```
