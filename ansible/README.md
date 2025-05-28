# Ansible Playbook: Install Docker and Clone BuggyPi

This Ansible playbook automates the installation of Docker and clones the BuggyPi repository on target hosts.

## Features

- **Cross-platform support**: Works on Ubuntu/Debian and CentOS/RHEL/Rocky Linux
- **Docker installation**: Installs Docker CE, Docker CLI, containerd, and Docker Compose plugin
- **User management**: Adds specified users to the docker group
- **Repository cloning**: Clones the BuggyPi repository from GitHub
- **Verification**: Verifies successful installation of Docker and Docker Compose

## Prerequisites

1. **Ansible installed** on your control machine:
   ```bash
   # On macOS
   brew install ansible
   
   # On Ubuntu/Debian
   sudo apt update && sudo apt install ansible
   
   # On CentOS/RHEL
   sudo yum install ansible
   ```

2. **SSH access** to target hosts (if not running locally)

3. **Sudo privileges** on target hosts

## Quick Start

### Running on localhost

To install Docker and clone the repository on your local machine:

```bash
cd ansible
ansible-playbook -i inventory.ini install-docker-and-clone.yml --ask-become-pass
```

### Running on remote hosts

1. Edit the `inventory.ini` file to add your target hosts:
   ```ini
   [servers]
   server1 ansible_host=192.168.1.10 ansible_user=ubuntu
   server2 ansible_host=192.168.1.11 ansible_user=ubuntu
   ```

2. Run the playbook:
   ```bash
   cd ansible
   ansible-playbook -i inventory.ini install-docker-and-clone.yml --ask-become-pass
   ```

## Configuration

### Variables

You can customize the following variables in the playbook:

- `repo_url`: GitHub repository URL (default: `https://github.com/sanjayBala/buggypi.git`)
- `repo_dest`: Destination directory for the cloned repository (default: `/opt/buggypi`)
- `docker_users`: List of users to add to the docker group (default: `{{ ansible_user }}`)

### Custom Variables

Create a `vars.yml` file to override default variables:

```yaml
# vars.yml
repo_dest: "/home/{{ ansible_user }}/buggypi"
docker_users:
  - ubuntu
  - developer
```

Then run with:
```bash
ansible-playbook -i inventory.ini install-docker-and-clone.yml --extra-vars "@vars.yml" --ask-become-pass
```

## Supported Operating Systems

- Ubuntu 18.04+
- Debian 9+
- CentOS 7+
- RHEL 7+
- Rocky Linux 8+

## What the Playbook Does

1. **Updates package cache** for the target OS
2. **Installs prerequisites** (curl, git, etc.)
3. **Adds Docker repository** and GPG keys
4. **Installs Docker CE** and related components
5. **Starts and enables** Docker service
6. **Adds users** to the docker group
7. **Creates destination directory** for the repository
8. **Clones the BuggyPi repository** from GitHub
9. **Sets proper ownership** of the cloned repository
10. **Verifies installation** and displays versions

## Post-Installation

After running the playbook:

1. **Log out and back in** (or run `newgrp docker`) to apply docker group membership
2. **Verify Docker works** without sudo:
   ```bash
   docker run hello-world
   ```
3. **Navigate to the cloned repository**:
   ```bash
   cd /opt/buggypi  # or your custom repo_dest
   ```

## Troubleshooting

### Common Issues

1. **Permission denied when running docker commands**:
   - Log out and back in to apply group membership
   - Or run: `newgrp docker`

2. **SSH connection issues**:
   - Ensure SSH keys are properly configured
   - Check firewall settings on target hosts

3. **Package installation failures**:
   - Verify internet connectivity on target hosts
   - Check if repositories are accessible

### Dry Run

To see what changes would be made without applying them:

```bash
ansible-playbook -i inventory.ini install-docker-and-clone.yml --check --diff
```

### Verbose Output

For detailed output during execution:

```bash
ansible-playbook -i inventory.ini install-docker-and-clone.yml -v --ask-become-pass
```

## Files

- `install-docker-and-clone.yml`: Main playbook
- `inventory.ini`: Host inventory file
- `ansible.cfg`: Ansible configuration
- `README.md`: This documentation

## License

This playbook is provided as-is for the BuggyPi project. 