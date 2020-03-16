# Miscellaneous Handy Tips
## Using Samba to exchange files on Windows
Enable samba on linux VM so that you can copy and paste files from your host Windows OS to Linux VM easily
### Install Samba on Linux VM
```
sudo yum install samba
vi /etc/samba/smb.conf
[root]
        path = /
        read only = no
        guest ok = yes
        create mask = 0755
        directory mask = 0755
        force user = root
        force group = root
 sudo systemctl restart smb
 ```
 ### Map Linux folder on Windows Explorer
 Run "Windows Explorer", Map Network Drive, Specify the IP and the samba folder directive, for example, **192.168.56.41/root**
