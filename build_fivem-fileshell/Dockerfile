FROM ubuntu:latest

# OpenSSH(SFTP server)
RUN apt-get update && apt-get install -y openssh-server
RUN mkdir -p /var/run/sshd
RUN ln -s /var/run/sshd /run/sshd
ARG ROOT_PASSWORD
ENV root_password=${ROOT_PASSWORD}
RUN echo "root:${root_password}" | chpasswd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

RUN echo 'sshd: all' >> /etc/hosts.deny
RUN echo 'sshd: 49.96.0.0/12' >> /etc/hosts.allow

#
RUN apt-get update && apt-get install -y curl
RUN apt-get update && apt-get install -y wget
RUN apt-get update && apt-get install -y git

#
RUN curl https://ipv4.fetus.jp/jp.txt | sed -e 's/^#.*//g' | grep -v '^$' | sed -e 's/^/sshd: /g' >> /etc/hosts.allow

#
RUN echo 'cd /' >> /root/.bashrc

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
