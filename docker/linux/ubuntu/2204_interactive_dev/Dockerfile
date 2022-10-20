FROM ubuntu:22.04

# All of these may be passed as build arguments; --build-arg USER=myusername
# User name may be passed as an argument:
ARG USER=jdoe
# Add any openssh public keys you want to be able to authenticate with
ARG AUTH_KEY_FILE=authorized_keys
ARG GROUP=${USER}
ARG UID=901
ARG GID=${UID}
ARG TZ="America/Los_Angeles"
ARG SSH_PORT=22

RUN apt update && apt install vim less unzip python3.10 wget gpg tzdata sudo \
    openssh-client openssh-server software-properties-common -y
RUN echo $TZ > /etc/timezone

RUN dpkg-reconfigure -f noninteractive tzdata
RUN apt install awscli git -y

RUN wget -O- https://apt.releases.hashicorp.com/gpg | \
    gpg --dearmor | \
    tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
RUN gpg --no-default-keyring \
    --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
    --fingerprint
RUN echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
    https://apt.releases.hashicorp.com jammy main" | \
    tee /etc/apt/sources.list.d/hashicorp.list
RUN apt-add-repository ppa:ansible/ansible
RUN apt update && DEBIAN_FRONTEND=noninteractive apt install terraform \
    packer ansible -y

RUN groupadd -r ${USER} -g ${GID}
RUN useradd -m -s /bin/bash -d /home/${USER}/ -u ${UID} -g ${GROUP} \
    -G sudo ${USER}

RUN mkdir -p /home/${USER}/.ssh
COPY authorized_keys /home/${USER}/.ssh/authorized_keys

RUN echo "%sudo ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/sudo_grp

RUN mkdir /run/sshd
EXPOSE $SSH_PORT/tcp

CMD ["/usr/sbin/sshd", "-D"]