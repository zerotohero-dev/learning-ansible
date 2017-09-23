```
# Documentation:
man ansible-doc

# List of modules that are documented.
# Better to dump it to a text file, it takes a while to generate:
ansible-doc -l
```

```
ansible all --list-hosts

cat /etc/ensible/hosts

ansible apacheweb -m ping

ansible newhosts -i hosts -m ping
```

Almost any configuration of ansible is overridable either from the command line, or from within the playbook.

```bash
# places that ansible looks for the configuration.
# this env var first: `export ANSIBLE_CONFIG=/new/config/path`
# ./ansible.cfg
# ~/ansible.cfg
# /etc/ansible/ansible.cfg
    roles_path = /home/ubuntu/playbooks/roles/roles:/etc/ansible/roles
```

## Setting Up Users

```
vim /etc/ansible/ansible.cfg

adduser ansible
passwd ansible

visudo

root 		ALL=(ALL)	ALL
ansible 	ALL=(ALL)	NOPASSWD: ALL

# add that user accounts to all the nodes that are going to run ansible.
```

Ansible playbook will fail if it has to pause and ask for a password.

```
su ansible -
```

```
su ansible -
ssh-keygen
ssh-copy-id ansibl@remote-host
```

Ansible is owned and administered by Red Hat.

* “Inventory” is groups of hosts etc.

* apt, ec2 etc etc are **module**s.

The documentation in https://docs.ansible.com/ is **really** good.

```
# -s: Elevate the privilege of the ansible user to sudo
ansible all -s -a "cat /var/log/messages"
```

```
# -m: module
# -a: attributes
# copy from local directory to remote host
ansible centos -m copy -a "src=test.txt" "dest=/tmp/test.txt"
```

```
# install packages
ansible ubuntu -m apt -a "name=elinks" "state=latest"
```

```
ansible centos -m setup # dumps useful facts

ansible centos -m setup -a 'filter=*ipv4*'

ansible centos -m setup --tree facts # saves to the facts directory.
```

## Variables

```
ansible-playbook telnet.yml --extra-vars "hosts=centos gather=no package=lynx"

ansible centos -s -m yum -a "name=telnet state=absent"

ansible centos -s -a "yum list installed | grep telnet"
```

## Debugging

Many modules, and all of the command and shell modules allow you to register arbitrary results.
