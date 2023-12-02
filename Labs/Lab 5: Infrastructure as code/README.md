# Infrastructure as Code (IaC)

This lab explores IaC with Vagrant and Ansible using imperative and declarative approaches.

## Functionality

1. VM provisioning using Vagrant and Ansible
2. GitLab installation
2. Health check configuration

## Installation

This application is written on NodeJS and it uses Redis database.

1. [Install VirtualBox](https://www.virtualbox.org/wiki/Downloads)

2. [Install Vagrant](https://www.vagrantup.com/downloads.html)

3. Download the `centos/7` Vagrant box for the **Virtualbox provider**, 
Run:

  ```
  vagrant box add centos/7
  ```

## Part 1. Imperative - Using Vagrant with Shell Provisioner

### 1. Prepare a virtual environment

After installing Vagrant, we will use a prepared `Vagrantfile` found in [`lab/part-1`]: 

```bash
cd lab/part-1
```

### 2. Create a virtual machine (VM)

We run the commands:

```bash
vagrant up
```
We also run the following commands to check VMs status, stop the VMs and destroy the VMs: 

```bash
# will check VMs status
vagrant status 

# stop the VMs
vagrant halt

# will destroy VMs
vagrant destroy
```

### 3. Check that everything is ok by connecting to the VM via SSH

1. We run this command to enter inside the VM via SSH:

```bash
vagrant ssh
```
 
2. We open the VirtualBox and check the installed virtual machine.

### 4. Play with different commands for Shell Provisioner

1. We configure the `/etc/hosts` file :

```ruby
# Start provisioning
config.vm.provision "shell",
  inline: "echo '127.0.0.1  mydomain-1.local' >> /etc/hosts"
```

We run:

```bash
vagrant provision
```

We read the `/etc/hosts` file content:

```bash
vagrant ssh
# ... entering to VM
cat /etc/hosts
```

Result: 

```bash
[vagrant@localhost ~]$ cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
127.0.0.1  mydomain-1.local
```

2. Replace in the `Vagrantfile` to read the current date into the `/etc/vagrant_provisioned_at` file :

```ruby
# Start provisioning
$script = <<-SCRIPT
echo I am provisioning...
date > /etc/vagrant_provisioned_at
SCRIPT

config.vm.provision "shell", inline: $script
```

Then, run:

```bash
vagrant provision
```

We read the `/etc/vagrant_provisioned_at` file content:

```bash
vagrant ssh
# ... entering to VM
cat /etc/vagrant_provisioned_at
```

Result: 

```bash
[vagrant@localhost ~]$ cat /etc/vagrant_provisioned_at
Mon Nov 27 18:42:09 UTC 2023
```


## Part 2. Declarative - GitLab installation using Vagrant and Ansible Provisioner 

### 1. Prepare a virtual environment

We will use a prepared `Vagrantfile` found in [`lab/part-2`]. 

### 2. Create and provision a virtual machine (VM)

We run the command:

```bash
vagrant up
```
### 3. Test the installation 

We open the URL - http://localhost:8080 in our browser. We see a GitLab sign in page and we log in by using the `root` username and the password that is stored in `/etc/gitlab/initial_root_password` inside the VM.
 
### 4. Instructions for updating playbooks

1. We update the playbooks on the VM using `vagrant upload`:

```bash
vagrant upload playbooks /vagrant/playbooks gitlab_server
```

2. We run provisioning again with the command:

```bash
vagrant provision
```

## Part 3. Declarative - Configure a health check for GitLab

1. We run a health check using `curl`:

  - We connect to the VM using `vagrant ssh`.
  - We run the command:
    ```bash
    curl http://127.0.0.1/-/health
    GitLab OK
    ```

Results:
```bash
[vagrant@localhost ~]$ curl http://127.0.0.1/-/health
Bad Gateway
``` 

2. We run the `gitlab/healthcheck` role:
  - We connect to the VM using `vagrant ssh`.
  - We run the playbooks using the right tag (replace `TAG`):
    ```bash
    ansible-playbook /vagrant/playbooks/run.yml --tags TAG -i /tmp/vagrant-ansible/inventory/vagrant_ansible_local_inventory
    ```

      
5. We run the 2 other kinds of health checks in the playbook (using the [uri module](https://docs.ansible.com/ansible/latest/modules/uri_module.html)):
  - [Readiness check](https://docs.gitlab.com/ee/user/admin_area/monitoring/health_check.html#readiness)
    Results:
    ```bash
    [vagrant@localhost ~]$ curl http://127.0.0.1/-/readiness
    {"error":"Bad Gateway","status":502}    
    ```

  - [Liveness check](https://docs.gitlab.com/ee/user/admin_area/monitoring/health_check.html#liveness)
    Results:
    ```bash
    [vagrant@localhost ~]$ curl http://127.0.0.1/-/liveness
    {"error":"Bad Gateway","status":502}
    ```
