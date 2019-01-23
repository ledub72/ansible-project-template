# Docker compose ansible test sshd

Simple docker compose project to get started with testing your ansible code

## Quick start

```bash
cd ./docker
docker-compose.exe up --build
```

## set root password

Setting the root password below `echo 'root:ansibletest' | chpasswd && \` on line 14
change the part after the `'root:'` to `'root:yournewpass'`

## ssh into your docker container

```bash
ssh -o 'StrictHostKeyChecking=no' -o 'UserKnownHostFile=/dev/null' -p 2022 -l root 127.0.0.1
```

https://askubuntu.com/questions/87449/how-to-disable-strict-host-key-checking-in-ssh/385187#385187

## Docker file

The docker file looks like this.

```dockerfile
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

```

## Inspired by

http://devo.ps/blog/vagrant-docker-and-ansible-wtf/