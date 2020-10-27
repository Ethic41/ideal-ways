
# Commands to Never Forget in Preparation of Huawei ICT Competition Regional and Global Finals

## First things First

### **Update**  

* yum update -y  

## Setting up TMUX (optional)  

### **Installation**  

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

* yum install -y nano

## Setting up Nginx, Apache, PHP and MySQL on CentOS

### Setting up Nginx Repository

* rpm --import <https://nginx.org/keys/nginx_signing.key> [optional]  
* yum -y install <http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm>  

* [alternative to the above] wget <http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm>
* [then ] rpm -ivh [the downloaded file above]  

### Installing and Enabling

* yum install -y nginx  
* systemctl start nginx  
* systemctl enable nginx  
* systemctl status nginx  

### Configuring nginx to support PHP

* *edit file at* /etc/nginx/conf.d/default.conf
* *add index.php to support php*
* *uncomment the php section to this form* [./images/nginx-setup-php-section.png]

*

#### **Adding MySQL Repository**  

* rpm -Uvh <http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm>

#### **Adding PHP7 Repository**  

* yum install -y <http://rpms.remirepo.net/enterprise/remi-release-7.rpm>  [main method should be this]  
* yum install -y <https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm>  

#### **Install PHP 7.3**

* yum install -y yum-utils
* yum-config-manager --enable remi-php73 [this could be remp-php7[ 0-4 ] pending on required version]
* yum install -y php php-mcrypt php-cli php-gd php-curl php-mysql php-ldap php-zip php-fileinfo php-fpm php-xml php-bcmath php-intl php-opcache php-soap php-mbstring

* systemctl start php-fpm  
* systemctl enable php-fpm
* systemctl status php-fpm

## Installing Wordpress  

* *note for latest version go to the site, below is latest version at time of writing this repo*
* Download link [https://wordpress.org/latest.tar.gz]

### **Install Composer**

* curl -sS <https://getcomposer.org/installer> | php  
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

#### **MySQL Login**

* mysql -u *username* -h *host* -p

#### **POSTGRESQL Login**

* psql --no-readline -U *user* -h *host* -p *port* -d *datastore* -W

#### **Create Databse**

* CREATE *database_name*;  

#### **Create User**

* CREATE USER *username* IDENTIFIED BY *password*;  

#### **Grant User Access to DB**

* GRANT ALL ON <database_name>.* TO <username> IDENTIFIED BY <password>;  

## Setting up vsftpd on CentOS  

### Installation

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

### Setting up NFS  

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
* ***append the following command to /etc/fstab***  
* *sfs_address /local_path* nfs vers=3,timeo=600,nolock 0 0  

## Cloud Migration Services  

### Installing linux Server Migration Service (SMS)  

* wget -t 3 -T 15 <https://sms-agent-bucket.obs.cn-north-1.myhwclouds.com/SMS-Agent.tar.gz>  

## Storage

### Setting up iSCSI in CentOS

* ***install utils:*** yum install -y iscsi-initiator-utils  
* ***to view the initiator name:*** cat /etc/iscsi/initiatorname.iscsi  
* ***to discover service ports:*** iscsciadm --mode discoverydb --type sendtargets --portal *ip_address_here* --discover  
* ***to login to discovered ports:*** iscsiadm --mode node --login
* ***to install rescan-scsi-bus:*** yum -y install sg3_utils
* ***to scan for new luns:*** rescan-scsi-bus.sh
* ***to check lun mapped to host:*** fdisk -l
* ***alternatively:*** lsscsi  
* ***alternatively:*** lsblk
* ***alternatively:*** df -h
* ***in case ultrapath is not available:*** yum install -y device-mapper-multipath
* ***start the multipath program:*** systemctl start multipathd
* ***enable the multipath program:*** systemctl enable multipathd
* ***create partition for the lun:*** fdisk /dev/sdb [n,p,w]
* ***check the partition has been created:*** fdisk -l
* ***make file-system and format partition:*** mkfs -t ext4 /dev/sdb1
* ***mount the partition:*** mount /dev/sdb1 */local_path*
* ***to view the mounted device UUID:*** blkid
* ***to mount at boot-time:*** /drive/path  /localpath   _netdev  0  2

#### Network Setup

* ***show ip address and interface:*** ip address show  
* ***add ip address to specific instance:*** ip address add dev *interface_name* *ip_address*  
* ***remove ip address:*** ip address flush dev *interface_name*  
* ***private ip_addresses:*** 10.0.0.0/8-24, 172.16.0.0/12-24, 192.168.0.0/16-24  

## Cloud Container Engine

### Installing Docker

* ***remove older versions of docker if any:*** docker remove docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine
* ***to get the docker install script:*** curl -fsSL get.docker.com -o get-docker.sh  
* ***install docker:*** sh get-docker.sh
* ***if user is not root:*** sudo usermod -aG docker *username*
* ***install docker-compose:*** sudo curl -L "<https://github.com/docker/compose/releases/download/1.27.0/docker-compose>-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

### Install using repo

* ***first update, then:*** sudo yum -y install docker

## Big Data

### HBase Commands and Best Practices

* ***to disable HBase excessive INFO logs:*** cd $HBASE_HOME/conf ***this is HBase default dir***  
* ***use vi or nano to edit*** **log4j.properties** *file*  
* ***change the line with:*** **log4j.threshold=ALL** ***to:*** **log4j.threshold=WARN**  

## AI

### Initial Setup

* ***install dependency packages:*** yum -y groupinstall "Development tools"  
* ***install other dependencies:*** yum –y install openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel  

## Huawei Cloud Service Vitals

### Check ECS DNS Setup

* ***check dns config of ECS:*** cat /etc/resolv.conf

#### Setup and Generate SSL Certificate

* ***create RSA private key[2048 bits]:*** openssl genrsa -out privateKeyFileName.key 2048
* ***create certificate sign request (csr):*** openssl req -new -key privateKeyFileName.key -out certFileName.csr
* ***create ssl certificate (x509 format) from private key:*** openssl x509 -req -days [days count] -in certFileName.csr -signkey privateKeyFileName.key -out certificateName.crt
* ***create ssl certificate (x509 format) from private key (with optional hashing algo):*** openssl x509 -req -days [days count] -in certFileName.csr -signkey privateKeyFileName.key -out certificateName.crt -sha1
* ***create ssl certificate (x509 format) signed using CA certificate:*** openssl x509 -req -days [days count] -in certFileName.csr -out certificateName.crt -sha1 -CAcreateserial -days 5000 -CA certificateAuthority.crt -CAkey CAKey.key

### Setup Simple Python Server

#### Setup Index files Python2

* ***create index files:*** vim index.html
* ***create server script:*** vim simpleserver
* ***add this to the file:*** cd [index directory] \n python -m SimpleHTTPServer 80

### Docker Section

#### Useful docker commands

* ***test if docker is successfully deployed:*** docker run hello-world
* ***list images:*** docker image
* ***list containers:*** docker container ls -a
* ***remove container:*** docker rm *container_id*
* ***remove image:*** docker image rm image-name
* ***run container in detached mode:*** docker run -d *container_name*
* ***pull an image from hub:*** docker pull
* ***execute a command inside a container:*** docker exec <container_id>
