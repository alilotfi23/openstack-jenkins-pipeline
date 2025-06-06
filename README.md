
# Jenkins-Powered OpenStack Deployment with Kolla Ansible

This project automates the deployment of OpenStack services using [Kolla Ansible](https://docs.openstack.org/kolla-ansible/latest/) via a Jenkins pipeline. It sets up a local Python environment, installs required tools, configures infrastructure, and deploys OpenStack on target servers.

## Features

- Full Jenkins pipeline for end-to-end OpenStack deployment
- Local Python virtual environment setup
- Installs Ansible and Kolla Ansible from source
- Prepares OpenStack configuration structure
- Bootstraps and validates target servers
- Automates secret generation and deployment steps

## Requirements

- Jenkins with permission to run shell commands
- Ubuntu-based environment with `sudo` privileges
- Docker installed (or installed by pipeline)
- Access to target servers (inventory file)
- Network access to download pip packages and git repositories

## Pipeline Overview

The Jenkins pipeline contains the following stages:

1. **Setup Local Environment**  
   Installs system dependencies and creates a virtual environment.

2. **Installing PIP**  
   Upgrades pip inside the virtual environment.

3. **Installing Ansible**  
   Installs a compatible version of `ansible-core`.

4. **Installing Kolla Ansible**  
   Installs Kolla Ansible from a Git repository and its dependencies.

5. **Preparing Infrastructure**  
   Prepares `/etc/kolla` and copies default config and inventory files.

6. **Secrets Setup**  
   Generates required OpenStack service passwords.

7. **Bootstrap Servers**  
   Prepares target nodes using `kolla-ansible bootstrap-servers`.

8. **Infrastructure Pre-Checks**  
   Validates configuration and connectivity before deployment.

9. **Deploy Infrastructure**  
   Executes the actual OpenStack deployment.

## Usage

### 1. Clone this repository

```bash
git clone https://github.com/alilotfi23/openstack-jenkins-pipeline
cd openstack-jenkins-pipeline
````

### 2. Setup Jenkins Job

* Create a new Jenkins pipeline job.
* Point the pipeline definition to the `Jenkinsfile` in this repo.
* Configure Jenkins to run on a compatible Ubuntu node with Docker.

### 3. Customize Inventory

Update the inventory file located at:

```
/usr/local/share/kolla-ansible/ansible/inventory/multi_packtpub_prod
```

To reflect your actual OpenStack deployment nodes.

### 4. Trigger the Pipeline

Start the pipeline from Jenkins UI. Each step will log its output, and errors will halt the process.

## Notes

* The Git repository URL for Kolla Ansible must be updated in the `Jenkinsfile` if different from the placeholder:

  ```
  git+https://opendev.org/openstack/kolla-ansible@master
  ```
* Make sure the Jenkins node has network access to the OpenStack deployment targets.

## License

MIT License

---
