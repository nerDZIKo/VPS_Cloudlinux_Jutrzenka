https://operavps.com/docs/install-directadmin-on-cloudlinux/

sudo yum install wget
sudo yum install psmisc net-tools systemd-devel libdb-devel perl-DBI
setenforce 0
sudo wget http://www.directadmin.com/setup.sh
sudo chmod 755 setup.sh
sudo ./setup.sh
sudo firewall-cmd --permanent --zone=public --add-port=2222/tcp