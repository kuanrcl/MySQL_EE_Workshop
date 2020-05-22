# Using Yii Framework with MySQL

## Install Yii Framework

Make sure the system time is sync with NTP
```
sudo timedatectl set-ntp yes
sudo systemctl start ntpd
date
```

Need PHP 5.6
```
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
sudo yum-config-manager --enable remi-php56 
sudo yum install php php-mcrypt php-cli php-gd php-curl php-mysql php-ldap php-zip php-fileinfo
```

Or PHP 7.2
```
sudo yum-config-manager --enable remi-php72
sudo yum update
sudo yum install php
```
PHP 7.2 on Ubuntu 7.8
```
apt install php7.2-cli php7.2-dev php7.2-mysql php7.2-pdo php7.2-xml php-curl php7.2-mbstring
```
Install Yii Framework
```
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
composer create-project --prefer-dist yiisoft/yii2-app-basic basic
cd basic
vi config/web.php
# specify a secret key
'cookieValidationKey'=>'some-secret-key'
yii serve 192.168.56.41:8888
```



```
