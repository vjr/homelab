FROM --platform=x86_64 centos:centos7

EXPOSE 3306

RUN systemctl stop firewalld
RUN systemctl disable firewalld

ADD xpand-6.0.3.el7.tar.bz2 /

RUN yum install -y bzip2 mdadm ntp ntpdate screen wget xz
RUN tar xvf xpand-5.3.21.el7.tar.bz2
RUN cd xpand-6.0.3.el7

USER xpand:xpand
USER xpandm:xpandm

RUN xpdnode_install.py --yes

RUN printf "\nexport PATH=$PATH:/opt/clustrix/bin\n" >> /home/xpandm/.bash_profile

RUN clx status
RUN clx dbstart

RUN mysql --user=root --no-auto-rehash -e "SET PASSWORD FOR root@127.0.0.1 = PASSWORD(\"dummy\");"
RUN mysql --user=root --password=dummy --no-auto-rehash -e "SET GLOBAL license = '{\"expiration\": \"2022-04-29 00:00:00\", \"company\": \"Individual\", \"email\": \"vishalrao@gmail.com\", \"person\": \"Vishal Rao\", \"signature\": \"redacted\"}';"
RUN mysql --user=root --password=dummy --no-auto-rehash -e "ALTER CLUSTER ADD '192.0.2.2', '192.0.2.3';"

VOLUME /data

COPY license.json /data
