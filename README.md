
---

# Ansible Onboarding Project

## 1. Environment Details

| Item | Details |
|------|----------|
| OS | Windows 10 (WSL2 Ubuntu 22.04 LTS) |
| Python version | 3.10.12 |
| Ansible version | 2.17.14 |
| Ansible-lint version | 25.9.2 |
| YAML-lint version | 1.37.1 |

---

### Installation Steps (Summary)

#### Create project folder
```bash
mkdir -p ~/ansible-onboarding && cd ~/ansible-onboarding
````

#### Install python3.10-venv package

```bash
sudo apt install python3.10-venv
```

#### Create Python virtual environment

```bash
python3 -m venv .venv
```

#### Activate venv

```bash
source .venv/bin/activate
```

#### Upgrade pip

```bash
python -m pip install --upgrade pip
```

#### Install Ansible and lint tools

```bash
python -m pip install ansible ansible-lint yamllint
```

#### Verify Ansible and lint tools are installed

```bash
ansible --version
ansible-lint --version
yamllint --version
```

#### Save dependencies

```bash
python -m pip freeze > requirements.txt
```

---

## 2. VS Code Setup

### Extensions Installed

* **Red Hat Ansible** – syntax highlighting, snippets, autocompletion
* **YAML (Red Hat)** – validates YAML syntax
* **Python** – needed for venv and running Python scripts
* **EditorConfig** – enforces consistent code style

### VS Code Settings (`.vscode/settings.json`)

```json
{
  "python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python",
  "ansible.python.interpreterPath": "${workspaceFolder}/.venv/bin/python",
  "ansibleLint.enabled": true,
  "yaml.validate": true,
  "files.trimTrailingWhitespace": true,
  "editor.formatOnSave": true
}
```

### EditorConfig (`.editorconfig`)

```ini
root = true

[*]
charset = utf-8
end_of_line = lf
indent_style = space
indent_size = 2
insert_final_newline = true
trim_trailing_whitespace = true
```

---

## 3. Baseline Ansible Configuration

### ansible.cfg (in project root)

```ini
[defaults]
inventory            = inventories/
roles_path           = roles:./.ansible/roles
host_key_checking    = True
retry_files_enabled  = False
interpreter_python   = auto_silent
forks                = 10
timeout              = 30
stdout_callback      = yaml
bin_ansible_callbacks = True

[ssh_connection]
pipelining = True
ssh_args   = -o ControlMaster=auto -o ControlPersist=60s
```

---

## 4. SSH Setup

### Generate SSH key pair

```bash
ssh-keygen -t ed25519 -C "your.email@company.com" -N "" -f ~/.ssh/id_ed25519
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### SSH Config (`~/.ssh/config`)

```bash
Host *
  ServerAliveInterval 30
  ServerAliveCountMax 4
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_ed25519
  StrictHostKeyChecking ask
```

---

## 5. Git Configuration

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@company.com"
git config --global init.defaultBranch main
git config --global --list
```

### Pre-Commit Hooks (`.pre-commit-config.yaml`)

```yaml
repos:
  - repo: https://github.com/adrienverge/yamllint
    rev: v1.35.1
    hooks:
      - id: yamllint

  - repo: https://github.com/ansible/ansible-lint
    rev: v24.6.1
    hooks:
      - id: ansible-lint
```

### Install and activate hooks

```bash
python -m pip install pre-commit
git init
pre-commit install
pre-commit run --all-files
```

---

## 6. How to Use Environment

1. Open project folder:
   `~/ansible-onboarding`

2. Activate virtual environment:

   ```bash
   source .venv/bin/activate
   ```

3. Run lint checks manually (optional):

   ```bash
   ansible-lint playbooks/
   yamllint playbooks/
   ```

---

## 7. Corporate / Proxy / Certificates Notes

If behind a proxy, set environment variables:

```bash
export http_proxy="http://proxy.company.com:port"
export https_proxy="http://proxy.company.com:port"
```

If corporate CA certs are required, configure them in `~/.ssh/config` or `.bashrc`.

---

## 8. New Machine? Do This Checklist

1. Enable virtualization in BIOS
2. Install WSL2 and Ubuntu 22.04
3. Install Python3 and pip
4. Create `.venv` and activate it
5. Install Ansible, ansible-lint, yamllint
6. Install VS Code and required extensions
7. Configure VS Code settings and `.editorconfig`
8. Generate SSH key pair and configure `~/.ssh/config`
9. Start SSH agent and add your key
10. Configure Git user/email and default branch
11. Install and enable pre-commit hooks
12. Verify all commands work:

    ```bash
    ansible --version
    ansible-lint --version
    pre-commit run --all-files
    ```

```


