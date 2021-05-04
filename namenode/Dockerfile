FROM bde2020/hadoop-base:2.0.0-hadoop3.2.1-java8

MAINTAINER Ivan Ermilov <ivan.s.ermilov@gmail.com>

HEALTHCHECK CMD curl -f http://localhost:9870/ || exit 1

ENV HDFS_CONF_dfs_namenode_name_dir=file:///hadoop/dfs/name
RUN mkdir -p /hadoop/dfs/name
VOLUME /hadoop/dfs/name

RUN apt update
RUN apt install openssh-server sudo -y

RUN useradd -rm -d /home/ubuntu -s /bin/bash -g root -G sudo -u 1000 test 

RUN echo 'root:test' | chpasswd
RUN service ssh start
RUN echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
RUN echo "PermitUserEnvironment yes" >> /etc/ssh/sshd_config


RUN mkdir -p ~/.ssh
RUN chmod 700 ~/.ssh
RUN touch ~/.ssh/environment
COPY ./hadoop.env /tmp.txt
RUN cat /tmp.txt > ~/.ssh/environment
RUN printenv >> ~/.ssh/environment
RUN echo 'source ~/.ssh/environment' >> ./.bashrc

RUN apt install -y wget
RUN wget https://apache-mirror.rbc.ru/pub/apache/spark/spark-3.1.1/spark-3.1.1-bin-hadoop2.7.tgz
RUN tar xvzf spark-3.1.1-bin-hadoop2.7.tgz
RUN sudo mv spark-3.1.1-bin-hadoop2.7/ /opt/spark
RUN export SPARK_HOME=/opt/spark >> ~/.bashrc
RUN export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin >> ~/.bashrc


ADD namenode/run.sh /run.sh
RUN chmod a+x /run.sh

EXPOSE 9870
EXPOSE 22

CMD ["/run.sh"]
