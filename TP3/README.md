# Discover Ansible

Till now we have only been preparing our applications to be deployed. However we did not deploy anything.

That’s where Ansible takes place. Ansible is basically a tool to manage your servers, provision them and deploy your applications on them.

This is not the only solution on the market, you’ll hear also about Chef, Puppet, Terraform during your developper life.

installation
```agsl
$ python3 -m pip install --user ansible
$ ansible --version
```

# SSH remote connection

SSH basically means Secure Shell, it is both a software and a communication protocol that uses the protocol/port TCP/22 of your machines to communicate. It is called Secure because the communication is encrypted using your ssh key pair.

Before trying any command, you should know that your private key requires restricted permissions to be used. Change the rights of your key:
```agsl
$ chmod 400 <path_to_your_key> 
```
Now your key can be used to ssh to your server. Go on and hit :
```agsl
$ ssh -i <path_to_your_key> centos@<your_server_domain_name>
```

depuis le répertoire TP3:
```agsl
ansible-playbook -i ansible/inventories/setup.yml ansible/playbook.yml
```
La commande ansible-playbook -i inventories/setup.yml playbook.yml est utilisée pour exécuter un playbook Ansible en spécifiant un inventaire et un fichier de playbook. Voici ce que chaque composant de la commande fait :

ansible-playbook: C'est la commande principale d'Ansible pour exécuter des playbooks, qui sont des fichiers YAML contenant des tâches et des rôles pour l'automatisation.

-i inventories/setup.yml: Cela spécifie l'inventaire à utiliser pour cette exécution. L'inventaire est un fichier qui répertorie les hôtes (serveurs ou machines) sur lesquels vous souhaitez exécuter vos tâches Ansible. Dans cet exemple, inventories/setup.yml est le chemin vers le fichier d'inventaire.

playbook.yml: C'est le nom du fichier de playbook à exécuter. Le playbook contient les tâches et les rôles que vous souhaitez appliquer aux hôtes répertoriés dans l'inventaire.


By using this role, you can keep your playbook clean and organized, and the Docker installation tasks are encapsulated within the role. You can reuse this role in other playbooks as needed.
# QUESTIONS

3-1 Document your inventory and base commands

See the /inventories/setup.yml file, comments are explaining the file

3-2 Document your playbook

See the /inventories/docker_playbook.yml

In this Ansible playbook:

to play it:
```agsl
$ ansible-playbook -i ansible/inventories/setup.yml ansible/docker_playbook.yml 
```

name: Playbook to install Docker: This line provides a descriptive name for the playbook. It doesn't impact the execution of the playbook but is useful for identifying its purpose when running or reading it.

hosts: all: This line specifies the hosts (machines) on which the tasks in the playbook will be executed. Here, "all" means the playbook runs on all hosts defined in the Ansible inventory. You can also specify a host group defined in your inventory, e.g., hosts: web_servers, to run it only on hosts in that group.

roles: This section indicates which Ansible roles should be executed in this playbook. In this excerpt, there's a single role specified, which is the Docker role. Roles are pre-packaged sets of tasks and configurations that you can reuse in different playbooks. In this case, the Docker role is meant to install Docker on the hosts.

Running this playbook will trigger the tasks from the Docker role on the specified hosts, resulting in the installation of Docker on those hosts. The actual tasks for installing Docker are defined in the tasks/main.yml file of the Docker role.

Document your docker_container tasks configuration.

- name: Create a network: This is a user-friendly name or label for the task. It helps in identifying the task when viewing the output of an Ansible playbook run.

docker_network: This is an Ansible module specifically designed for managing Docker networks.

name: app-network: This line specifies the name of the Docker network to be created. In this case, the network will be named "app-network."

vars: This section allows you to set variables or environment options for the task. In this case, it specifies the Python interpreter to use.

ansible_python_interpreter: /usr/bin/python3: The task is instructed to use the Python 3 interpreter located at /usr/bin/python3. This is useful when different Python versions are available on the system.

become: yes: This line indicates that the task should be executed with superuser or root privileges. This is necessary because creating Docker networks typically requires elevated permissions.
true

name: This is the name you want to give to the Docker container you are creating. In this example, it's "httpd."

image: This specifies the Docker image that you want to use to create the container. In this case, it's "royerbantoine/tp-devops-httpd:1.0."

recreate: When set to true, it indicates that the container should be recreated if it already exists. If set to false, the task will only create the container if it doesn't already exist.

pull: This option tells Ansible to pull the Docker image from the repository if it's not already present on the host.

ports: Here, you specify the port mapping. It maps port 80 of the host to port 80 of the container, allowing external access to the container's web server.

networks: This option allows you to connect the container to one or more Docker networks. In this case, it connects the container to the "app-network."

These options collectively define how the Docker container will be created and run within your Docker environment. The docker_container module in Ansible uses these options to manage the Docker containers on your host.
