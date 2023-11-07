# [TP3 - Ansible](http://school.pages.takima.io/devops-resources/ch3-discover-ansible-tp/)


Up until now, we have primarily focused on preparing our applications for deployment, but we haven't yet deployed anything.

This is where Ansible comes into play. Ansible is a powerful tool for managing servers, provisioning them, and deploying applications.

It's worth noting that Ansible is not the only solution available in the market. Throughout your developer journey, you'll also encounter other tools like Chef, Puppet, and Terraform.

## Installation

```shell
python3 -m pip install --user ansible
ansible --version
````


# SSH remote connection

SSH basically means Secure Shell, it is both a software and a communication protocol that uses the protocol/port TCP/22 of your machines to communicate. It is called Secure because the communication is encrypted using your ssh key pair.

Before trying any command, you should know that your private key requires restricted permissions to be used. Change the rights of your key:
```shell
chmod 400 <path_to_your_key> 
```
Now your key can be used to ssh to your server. Go on and hit :
```shell
ssh -i <path_to_your_key> centos@<your_server_domain_name>
```

From the TP3 directory, you can run the following command to execute an Ansible playbook:

```shell
ansible-playbook -i ansible/inventories/setup.yml ansible/playbook.yml
```
Here's what each component of the command does:

 - ansible-playbook: This is the primary Ansible command used to execute playbooks. Playbooks are YAML files containing tasks and roles for automation.

 - -i inventories/setup.yml: This specifies the inventory to be used for the execution. An inventory is a file that lists the hosts (servers or machines) on which you want to perform Ansible tasks. In this example, inventories/setup.yml is the path to the inventory file.

 - playbook.yml: This is the name of the playbook file to execute. The playbook contains the tasks and roles you want to apply to the hosts listed in the inventory.

By using this role, you can keep your playbook clean and organized, with Docker installation tasks neatly encapsulated within the role. This allows for easy reuse of the role in other playbooks as needed.

# Answers to Questions

### 3-1 Document Your Inventory and Base Commands

Please refer to the `TP3/ansible/inventories/setup.yml` file for details. Comments within the file provide explanations.

### 3-2 Document Your Playbook

You can find the playbook in the `TP3/ansible/inventories/docker_playbook.yml` file.

You can execute it with the following command:

```shell
ansible-playbook -i ansible/inventories/setup.yml ansible/docker_playbook.yml
```


 - `hosts: all`: This line specifies the hosts (machines) on which the playbook's tasks will run. Here, "all" means the playbook runs on all hosts defined in the Ansible inventory.
 - `Become: yes` option indicates that Ansible should escalate privileges to become the superuser.

 - `roles`: This section indicates which Ansible roles should be executed in this playbook: 
   1. **docker**: Contains tasks for installing and configuring Docker.
   2. **network**: Manages network configuration and setup.
   3. **database**: Handles database setup and configuration.
   4. **app**: Deploys and configures the application.
   5. **proxy**: Configures a reverse proxy.
   6. **front**: Manages the front-end web server setup.


### Document Your docker_container Tasks Configuration

 - `name: Create a Network`: This is a user-friendly label for the task, helping identify it when viewing Ansible playbook output.

 - `docker_network`: This is an Ansible module designed for Docker network management.

 - `name: app-network`:roles This line specifies the name of the Docker network to be created. In this case, the network is named "app-network."

 - `vars`: This section allows you to set task-specific variables or environment options. Here, it specifies the Python interpreter to use.

 - `ansible_python_interpreter: /usr/bin/python3`: The task is instructed to use the Python 3 interpreter located at /usr/bin/python3. This is useful when multiple Python versions are available on the system.

 - `become: yes`: This line indicates that the task should be executed with superuser or root privileges. This is necessary because creating Docker networks typically requires elevated permissions.

 - `name`: This is the name you want to assign to the Docker container being created. In this example, it's "httpd."

 - `image`: This specifies the Docker image used to create the container. Here, it's "royerbantoine/tp-devops-httpd:1.0."

 - `recreate`: When set to true, it indicates that the container should be recreated if it already exists. If set to false, the task will only create the container if it doesn't already exist.

 - `pull`: This option tells Ansible to pull the Docker image from the repository if it's not already present on the host.

 - `ports`: You specify the port mapping here. It maps port 80 of the host to port 80 of the container, enabling external access to the container's web server.

 - `networks`: This option allows you to connect the container to one or more Docker networks. In this case, it connects the container to the "app-network."


