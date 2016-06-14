FROM centos:latest

RUN yum install -y epel-release
RUN yum install -y mariadb-server mariadb
RUN yum install -y nrpe nagios-plugins-users nagios-plugins-load nagios-plugins-swap nagios-plugins-disk nagios-plugins-procs
RUN mysql_install_db --user=mysql --ldata=/var/lib/mysql/

RUN sed -i '/\[mysqld\]/aport=3306' /etc/my.cnf

RUN \
  echo "/usr/bin/mysqld_safe --basedir=/usr &" > /tmp/config && \
  echo "cat /var/log/mariadb/mariadb.log" >> /tmp/config && \
  echo "mysqladmin --silent --wait=30 ping || exit 1" >> /tmp/config && \
  echo "mysql -e 'GRANT ALL PRIVILEGES ON *.* TO \"root\"@\"%\";'" >> /tmp/config && \
  bash /tmp/config && \
  rm -f /tmp/config

VOLUME ["/var/lib/mysql"]

WORKDIR /data

CMD ["/usr/bin/mysqld_safe"]

EXPOSE 3306
