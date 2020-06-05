# Manage MySQL with Ansible

## Install Ansible

Make sure you have EPEL repository enabled  

Ansible architecture consists of 

* controller node with Ansible
* managed nodes (need sudo-user, ssh key)

```
sudo cat /etc/os-release
cat /etc/yum.repos.d/epel-yum-ol7.repo
[ol7_epel]
name=Oracle Linux $releasever EPEL ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/developer_EPEL/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=1

sudo yum install ansible
sudo useradd ansible
sudo visudo

# allow ansible to run sudo
root    ALL=(ALL)       ALL
mysql   ALL=(ALL)       ALL
ansible ALL=(ALL)       ALL

## Allows members of the 'sys' group to run networking, software,
## service management apps and more.
# %sys ALL = NETWORKING, SOFTWARE, SERVICES, STORAGE, DELEGATING, PROCESSES, LOCATE, DRIVERS

## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

ssh-keygen

# copy ssh public key to hosts ansible1, ansible2
ssh-copy-id ansible1
ssh-copy-id ansible2
```

# Ansible basics

```
# inventory file 
[db]
secondary

# ansible.cfg
[defaults]
remote_user=ansible
host_key_checking=false
inventory=inventory

[privilege_escalation]
become = True
become_method = sudo
become_user = root
become_ask_pass = False
```

```
ansible all -i inventory --list-hosts
ansible all -m command -a "useradd bob"
ansible all -m command -a "id bob"
ansible all -m shell -a "cat /etc/passwd | grep ansible"
ansible all -m package -a "name=vsftpd state=installed"
```

# Ansible modules

```
ansible-doc -l
ansible-doc user

ansible secondary -m user -a "name=linda shell=/bin/bash"
[WARNING]: Platform linux on host secondary is using the discovered Python interpreter at /usr/bin/python, but
future installation of another Python interpreter could change this. See
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.
secondary | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "comment": "",
    "create_home": true,
    "group": 1004,
    "home": "/home/linda",
    "name": "linda",
    "shell": "/bin/bash",
    "state": "present",
    "system": false,
    "uid": 1004
}
```

## Ansible Playbook

```
vim .vimrc

# .vimrc
autocmd FileType yaml setlocal ai ts=2 sw=2 et
```

## YAML
No <tab>

```
vim test.yaml

# test.yaml
---
- item1: value
  name: item1
  - sub1
    name: sub1
  - sub2
    name: sub2
- item2: value
  name: items
    prop1: value3
    prop2: value4
...
```

# Sample Playbook

```
#vsftpd.yaml

---
- name: deploy vsftpd
  hosts: all
  tasks:
  - name: install vsftpd
    package:
      name: vsftpd
      state: latest
  - name: enable vsftpd
    service: name=vsftpd enabled=true
  - name: create readme file
    copy:
      content: "welcome to my friendly server\n"
      dest: /var/ftp/pub/README
      force: no
      mode: 0444
...
```

## Results
```
PLAY [deploy vsftpd] ************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************
[WARNING]: Platform linux on host secondary is using the discovered Python interpreter at /usr/bin/python, but
future installation of another Python interpreter could change this. See
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.
ok: [secondary]

TASK [install vsftpd] ***********************************************************************************************
ok: [secondary]

TASK [enable vsftpd] ************************************************************************************************
changed: [secondary]

TASK [create readme file] *******************************************************************************************
changed: [secondary]

PLAY RECAP **********************************************************************************************************
secondary                  : ok=4    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```












