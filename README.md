## Set up a clean enviroment for the workshop

### Prerequisites

As Ansible is not included in the RHEL8 repositories, you need to add a new repo

```
sudo subscription-manager repos --enable ansible-2-for-rhel-8-x86_64-rpms
```

Install Ansible on your Red Hat Enterprise Linux host.

```
sudo yum -y install ansible
```

Create an ansible user with sudo privileges on your managed host, which doesn't require a password to elevate privileges

```
sudo useradd ansible
sudo passwd ansible
sudo echo "ansible ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/ansible
```

Create ssh keys and copy the public key to all remote hosts you wish to manage. Replace **MYHOST** with your remote host e.g. 192.168.1.2

```
ssh-keygen -q -t rsa -N '' -f ~/.ssh/id_rsa_ansible
ssh-copy-id -i ~/.ssh/id_rsa_ansible.pub ansible@MYHOST
```

### Clone the repository

```
raphael@desktop:~$ git clone https://github.com/RapTho/vm-security-hardening.git
raphael@desktop:~$ cd vm-security-hardening
```

### Add your ansible managed vm

In the [inventory](inventory) file, replace the example IP address with your vm's IP or FQDN.

Do **NOT remove** the ansible_port or ansible_ssh_private_key_file.

```
[workstation]
192.168.122.136 ansible_port="{{ ssh_port }}" ansible_ssh_private_key_file=~/.ssh/id_rsa_ansible
```

### Set a password for the non-root users

You first need to **set a password for the Ansible vault**. When later running the playbook, you need this password again.

```
raphael@desktop:~$ ansible-vault create password.yml
New Vault password:
Confirm New Vault password:
```

Enter your password to the key **password** in the following format:

```yaml
password: myPassword
```

### Run the Ansible playbook

The remote host will be **reset** to its previous state. All workshop users and their home directories will be deleted!

```
raphael@desktop:~$ ansible-playbook playbook.yml --vault-id password.yml@prompt
```

The VM is now ready.
