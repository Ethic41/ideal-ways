
# Commands to Never Forget in Preparation of Huawei ICT Competition Regional and Global Finals   

## Setting up TMUX (optional)  
#### **Installation**
* yum install -y tmux  

#### **Change the config for ease of use**  
Place the config in the ~/.tmux.conf file  

    #remap prefix to Control + a  
    set -g prefix C-a  
    unbind C-b  
    bind C-a send-prefix  

    # force a reload of the config file  
    unbind r  
    bind r source-file ~/.tmux.conf  

    # quick pane cycling  
    unbind ^A  
    bind ^A select-pane -t :.+  
  
#### **Basic Usage Commands**  
* tmux new -s *session_name*  
create a new session
* tmux attach -t *session_name*  
attach to an existion session
* tmux switch -t *session_name*  
switch to another session
* tmux list-sessions  
list sessions
* tmux detach (*prefix* + d)  
detach from current session, prefix based on above config is ***Ctrl + a***  
* *prefix* + %  
split screen vertically  
* *prefix* + "  
split screen horizontally  

#### Install Nano text editor (cos it's simple to use)
* yum install nano

## Setting up Apache, PHP and MySQL on CentOS  

#### **Adding MySQL Repository**  
* rpm -Uvh http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm  

#### **Adding PHP7 Repository**  
* yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm  
* yum install -y http://rpms.remirepo.net/enterprise/remi-release-7.rpm  

#### **Update**  
* yum update  

#### **Install PHP 7.3**
* yum install -y yum-utils
* yum-config-manager --enable remi-php73
* yum install -y php php-mcrypt php-cli php-gd php-curl php-mysql php-ldap php-zip php-fileinfo php-fpm php-xml php-bcmath php-intl php-opcache php-soap php-mbstring
* systemctl start php-fpm
* systemctl enable php-fpm  

#### **Install Composer**
* curl -sS https://getcomposer.org/installer | php  
make it globally accessible
* 

#### **Install Apache**
* yum install -y httpd
* systemctl start httpd
* systemctl enable httpd

#### **Install MySQL**
* yum install -y mysql mysql-server  

#### **MySQL Secure Setup**
* systemctl start mysqld
* systemctl enable mysqld  

> get the temporary mysql password
* grep "temporary password" /var/log/mysqld.log
* mysql_secure_installation  

#### **Create Databse**
* CREATE *database_name*;  

#### **Create User**
* CREATE USER *username* IDENTIFIED BY *password*;  

#### **Grant User Access to DB**
* GRANT ALL ON *database_name*.* TO *username* IDENTIFIED BY *password*;  

## Setting up Nginx on CentOS  

#### Installing
* wget http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
* rpm -ivh nginx-release-centos-7-0.el7.ngx.noarch.rpm
* yum install nginx
* systemctl start nginx
* systemctl enable nginx

## Setting up vsftpd on CentOS  

#### Installation
* yum update
* yum install vsftpd
* systemctl start vsftpd
* systemctl enable vsftpd  
  
check the port the service is running on
* netstat -antup | grep "ftp"  

#### Creating FTP user
add new ftp user  
* useradd *ftpadmin*
* passwd *yourpassword*

#### Creating FTP directory
* mkdir /var/ftp/work01
#### Granting ownership of the ftp directory to ftpadmin
* chown -R ftpadmin:ftpadmin /var/ftp/work01  

## Linux disks, file systems and file service commands  
#### Setting up NFS  
* rpm -qa | grep nfs  (*CentOS*)  
* dpkg -l nfs-common (*Debian*)
* yum install nfs-utils  (*CentOS*)  
* apt install nfs-common (*Debian*)  
#### Network Commands  
* yum install -y bind-utils  
#### SFS Commands  
* mount -t nfs -o vers=3,timeo=600,nolock shared_path local_path  
* mount -l  
Automatically mounting an **SFS path**
* *sfs_address /local_path* nfs vers=3,timeo=600,nolock 0 0  
## Cloud Migration Services  
#### Installing linux Server Migration Service (SMS)  
* wget -t 3 -T 15 *https://sms-agent-bucket.obs.cn-north-1.myhwclouds.com/SMS-Agent.tar.gz*  

## Storage
#### Setting up iSCSI in CentOS
* ***install utils:*** yum install -y iscsi-initiator-utils  
* ***to view the initiator name:*** cat /etc/iscsi/initiatorname.iscsi  
* ***to discover service ports:*** iscsciadm --mode discoverydb --type sendtargets --portal *ip_address_here* --discover  
* ***to login to discovered ports:*** iscsiadm --mode node --login  
* ***to check lun mapped to host:*** fdisk -l  
* ***alternatively:*** lsscsi
* ***in case ultrapath is not available:*** yum install device-mapper-multipath  
* ***start the multipath program:*** systemctl start multipathd  
* ***enable the multipath program:*** systemctl enable multipathd  
* ***create partition for the lun:*** fdisk /dev/sdb [n,p,w]  
* ***check the partition has been created:*** fdisk -l  
* ***make file-system and format partition:*** mkfs -t ext4 /dev/sdb1  
* ***mount the partition:*** mount /dev/sdb1 */local_path*  

#### Network Setup
* ***show ip address and interface:*** ip address show  
* ***add ip address to specific instance:*** ip address add dev *interface_name* *ip_address*  
* ***remove ip address:*** ip address flush dev *interface_name*  
* ***private ip_addresses:*** 10.0.0.0/8-24, 172.16.0.0/12-24, 192.168.0.0/16-24  

## Cloud Container Engine
##### Installing Docker
* ***to get the docker install script:*** curl -fsSL get.docker.com -o get-docker.sh  
* ***install docker:*** sh get-docker.sh
##### Install using repo
* ***first update:*** sudo 

##### working docker image for magento2
* ***dahir kindly edit this I'm horrible in markup(or is it markdown language) Lolx
* ***docker pull alexcheng/magento2:latest

## Big Data
#### HBase Commands and Best Practices
* ***to disable HBase excessive INFO logs:*** cd $HBASE_HOME/conf ***this is HBase default dir***  
* ***use vi or nano to edit *** **log4j.properties** *file*  
* ***change the line with:*** **log4j.threshold=ALL** ***to:*** **log4j.threshold=WARN**  
