# Ansible Workstation Setup

## Completed By

**Joshua Chibuisi**

## Project Objective

This project sets up a consistent, reliable workstation for developing and running Ansible automation.

It brings together Ansible, linting tools, Git, pre-commit checks, Python isolation, and SSH configuration in one controlled environment, so Ansible development can be validated locally using practices that also work in a team setting.

## Controller Environment

The Ansible controller runs in **WSL2** using Ubuntu.

| Component        | Version / Details                   |
| ---------------- | ----------------------------------- |
| Operating System | Ubuntu 26.04 LTS (Resolute Raccoon) |
| Environment      | WSL2                                |
| Python           | 3.14.4                              |
| Ansible Core     | 2.21.4                              |
| Architecture     | Linux/WSL2                          |

The project is located at:

```text
~/ansible-onboarding
```

## Installed Tools

The following tools were installed and configured as part of the workstation setup:

| Tool         | Version |
| ------------ | ------- |
| Ansible      | 2.21.4  |
| Ansible Lint | 26.8.0  |
| YAML Lint    | 1.38.0  |
| Pre-commit   | 4.6.2   |
| Git          | 2.53.0  |
| OpenSSH      | 10.2p1  |

Ansible and the Python-based development tools are installed inside the project's Python virtual environment, not globally.

## Python Virtual Environment

The project uses a Python virtual environment named `.venv`.

`.venv` keeps the project's Python packages isolated from the system Python installation. This prevents project dependencies from interfering with other Python projects or with packages managed by the operating system.

To activate the environment:

```bash
cd ~/ansible-onboarding
source .venv/bin/activate
```

Once activated, commands such as `ansible`, `ansible-lint`, and `yamllint` use the versions installed for this project.

The Ansible executable is located at:

```text
~/ansible-onboarding/.venv/bin/ansible
```

## VS Code Configuration

Visual Studio Code is the development environment for the project.

The following extensions are installed:

* **Ansible**: syntax highlighting, language support, and editing assistance.
* **YAML**: syntax support and validation.
* **Python**: language support and virtual-environment integration.
* **EditorConfig**: forces Visual Studio Code to follow the formatting rules defined in a project's .editorconfig file.

VS Code is configured to use the project's `.venv` Python interpreter:

```text
~/ansible-onboarding/.venv/bin/python
```

This means Python-related tools and extensions use the same environment as the project rather than the system-wide Python installation.

## Ansible Configuration

The project contains an `ansible.cfg` file that provides project-level configuration for Ansible.

The configuration file defines settings such as inventory location, SSH behavior, and other Ansible defaults consistently for the project.

The workstation disables SSH host-key checking through the Ansible configuration. This is intentional for the **controlled training environment**, where the setup is used for learning and testing.

This setting should not carry over to production environments, where proper host-key verification matters for SSH security.

## SSH Configuration

SSH is what Ansible uses to connect securely to managed hosts.

The SSH private key is stored in the user's SSH directory:

```text
~/.ssh/
```

The private key itself is not included in this repository or README.

The **SSH agent** loads the private key into memory, so SSH-based authentication doesn't require repeatedly entering the key's passphrase.

The `known_hosts` file stores the identities of SSH hosts contacted previously. It lets SSH recognize known hosts and detect unexpected changes to their host keys.

Typical SSH files used by the workstation include:

```text
~/.ssh/
├── <private-key>
├── <public-key>.pub
└── known_hosts
```

Private-key contents should never be committed to Git or shared publicly.

## Git and Pre-commit

Git tracks the project and manages changes to the Ansible workstation configuration.

### `.gitignore`

The `.gitignore` file prevents local and sensitive files from accidentally being committed to the repository.

Examples of files that should remain local include:

* `.venv/`
* SSH private keys
* Local environment files
* Python cache files
* Other machine-specific files

This keeps the repository focused on the configuration and files needed to reproduce the workstation, rather than personal or sensitive data.

### Pre-commit Hooks

Pre-commit is configured to check files automatically before they're committed.

The configured hooks include:

* **Ansible Lint**: checks Ansible files for common problems and recommended practices.
* **YAML Lint**: checks YAML files for formatting and syntax issues.
* **Trailing whitespace checks**: detects unnecessary whitespace.
* **End-of-file checks**: ensures files have the expected ending format.
* **Git-related file checks**: helps identify problematic files before they enter the repository.

The hooks run automatically when a Git commit is attempted.

They can also be run manually across the repository with:

```bash
pre-commit run --all-files
```

## Verification Commands

The following commands were used to verify that the workstation tools are installed and working correctly.

### Verify Ansible

```bash
ansible --version
```

Expected output includes:

```text
ansible [core 2.21.4]
```

The output also confirms that Ansible is running from the project's `.venv`.

### Verify Ansible Lint

```bash
ansible-lint --version
```

Expected:

```text
ansible-lint 26.8.0
```

### Verify YAML Lint

```bash
yamllint --version
```

Expected:

```text
yamllint 1.38.0
```

### Verify Pre-commit

```bash
pre-commit --version
```

Expected:

```text
pre-commit 4.6.2
```

### Verify Git

```bash
git --version
```

Expected:

```text
git version 2.53.0
```

### Verify OpenSSH

```bash
ssh -V
```

Expected output includes:

```text
OpenSSH_10.2p1
```

### Verify the SSH Agent

Check whether the SSH agent is running and whether keys have been loaded:

```bash
ssh-add -l
```

If the agent is running and a key has been added, the command displays the available key fingerprints.

If no keys are loaded, add one with:

```bash
ssh-add ~/.ssh/<private-key>
```

## Team-Friendly Feature

The most useful team-friendly feature of this setup is project-level configuration and dependency isolation.

The `ansible.cfg`, `.gitignore`, pre-commit configuration, and `.venv` work together to make the development environment predictable. Team members can clone the project, create their own virtual environment, install the required dependencies, and work with the same Ansible configuration and automated quality checks.

This reduces differences between individual development environments and catches common problems before changes are committed.

## Pitfall Avoided

One pitfall avoided was **installing Ansible and its dependencies globally**.

Installing everything directly into the system Python environment can cause dependency conflicts and make it difficult to reproduce the same setup on another machine.

`.venv` keeps the project's dependencies isolated.

The setup also avoids other common mistakes by:

* Keeping `.venv` out of Git through `.gitignore`.
* Keeping SSH private-key contents out of the repository.
* Avoiding unnecessary replacement or overwriting of existing SSH keys.
* Running linting and validation checks before commits.
* Restricting disabled SSH host-key checking to the controlled training environment.
* Using a broken `venv`

## New Machine? Do This

Use the following checklist to reproduce the Ansible workstation from scratch:

* [ ] Prepare the operating system and ensure Ubuntu 26.04 LTS is available through WSL2.
* [ ] Verify that Python, Git, and OpenSSH are installed and working.
* [ ] Create the Ansible workspace and navigate into it:

  ```bash
  mkdir -p ~/ansible-onboarding
  cd ~/ansible-onboarding
  ```
* [ ] Initialize the workspace as a Git repository:

  ```bash
  git init
  ```
* [ ] Create a `.gitignore` file and exclude `.venv`, SSH keys, environment files, and other local or sensitive files.
* [ ] Create the Python virtual environment:

  ```bash
  python3 -m venv .venv
  ```
* [ ] Activate the virtual environment:

  ```bash
  source .venv/bin/activate
  ```
* [ ] If creating the virtual environment returns an error about `venv` or `ensurepip`, install the required Python package and recreate the environment:

  ```bash
  sudo apt update
  sudo apt install python3-venv
  rm -rf .venv
  python3 -m venv .venv
  source .venv/bin/activate
  ```
* [ ] Install Ansible, Ansible Lint, YAML Lint, and Pre-commit inside the virtual environment.
* [ ] Install and configure the required VS Code extensions, and select `.venv/bin/python` as the project's Python interpreter.
* [ ] Create `ansible.cfg` with the required project-level Ansible settings, including disabled host-key checking for the controlled training environment.
* [ ] Prepare the SSH key, start the SSH agent, add the key to the agent, and verify `known_hosts` is configured without exposing the private key.
* [ ] Install the pre-commit hooks and run the final verification commands to confirm Ansible, linting tools, Git, Pre-commit, and SSH are working correctly.
