FROM centos/systemd
LABEL maintainer="takahiro.yamaki@example.com"

RUN set -x \
    && yum update  -y \
    && yum  install \
            openssh-server \
            sudo -y \
    && useradd -m jenkins \
    && echo "jenkins:jenkins" | chpasswd \
    && echo 'jenkins ALL=(ALL) ALL' >> /etc/sudoers \
    # for sshd
    && systemctl enable sshd.service

# RUN rm /var/run/nologin

EXPOSE 80
EXPOSE 8081
EXPOSE 8082

CMD ["/usr/sbin/init"]