FROM ubuntu:latest

# OpenSSH(SFTP server)
WORKDIR /tmp
RUN apt-get update && apt-get install -y curl wget git vim unar file
RUN apt-get update && apt-get install -y openssh-server
RUN mkdir -p /var/run/sshd
RUN ln -s /var/run/sshd /run/sshd
ARG ROOT_PASSWORD
ENV root_password=${ROOT_PASSWORD}
RUN echo "root:${root_password}" | chpasswd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN echo 'sshd: all' >> /etc/hosts.deny
RUN curl https://ipv4.fetus.jp/jp.txt | sed -e 's/^#.*//g' | grep -v '^$' | sed -e 's/^/sshd: /g' >> /etc/hosts.allow

#
WORKDIR /
RUN echo 'cd /' >> /root/.bashrc

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
