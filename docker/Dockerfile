# borrowed https://docs.docker.com/engine/examples/running_ssh_service/
FROM ubuntu:18.04

# some useful debuging tools to troubleshoot on these containers
RUN apt-get update && apt-get install -y \
                        net-tools \
                        netcat \
                        openssh-server \
                        curl

# configure sshd to work as we need it in 18.04
# sets the rootpassword to ansibletest
RUN mkdir /var/run/sshd && \
    echo 'root:ansibletest' | chpasswd && \
    echo '\n#allow root\nPermitRootLogin yes\n' >> /etc/ssh/sshd_config

EXPOSE 22

ENTRYPOINT ["/usr/sbin/sshd"]
#ENTRYPOINT ["/usr/sbin/sshd", "-D", "-d"]
# for production system remove "-d"
#  -D      When this option is specified, sshd will not detach and does not become a daemon.
#          This allows easy monitoring of sshd.
#  -d      Debug mode.  The server sends verbose debug output to standard error, and does not put itself in the background.
#          The server also will not fork and will only process one connection.
#          This option is only intended for debugging for the server.
#          Multiple -d options increase the debugging level.  Maximum is 3.

# by default start sshd as background daemon
# CMD ["/usr/sbin/sshd", "-D" ]
CMD ["-D" ]
# used for debugging lets you pass options to sshd on startup
#CMD ["-D", '-d']