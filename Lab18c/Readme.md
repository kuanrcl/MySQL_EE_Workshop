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

## Allows people in group wheel to run all commands
%wheel  ALL=(ALL)       ALL

## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

ssh-keygen

# copy ssh public key to hosts ansible1, ansible2
ssh-copy-id ansible1
ssh-copy-id ansible2
```


```


