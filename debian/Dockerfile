FROM debian:stable-slim

LABEL maintainer="Florian JUDITH <florian.judith.b@gmail.com>"

# fahclient version
ARG FAHCLIENT_MINOR_VERSION=7.5.1
ARG FAHCLIENT_MAJOR_VERSION=7.5

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

# This Dockerfile adds a non-root user with sudo access.
ARG USERNAME=fah
ARG USERNAME_UID=1000
ARG USERNAME_GID=${USERNAME_UID}

RUN apt-get update && \
    apt-get -y install --no-install-recommends sudo apt-utils dialog 2>&1 && \
    apt-get install -y curl ca-certificates bzip2 && \
    curl -o /tmp/fahclient.deb https://download.foldingathome.org/releases/public/release/fahclient/debian-stable-64bit/v${FAHCLIENT_MAJOR_VERSION}/fahclient_${FAHCLIENT_MINOR_VERSION}_amd64.deb && \
    mkdir -p /etc/fahclient/ && \
    touch /etc/fahclient/config.xml && \
    dpkg --install /tmp/fahclient.deb && \
    groupadd --gid ${USERNAME_GID} ${USERNAME} && \
    useradd -s /bin/bash --uid ${USERNAME_UID} --gid ${USERNAME_GID} -m ${USERNAME} && \
    echo ${USERNAME} ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/${USERNAME} && \
    chmod 0440 /etc/sudoers.d/${USERNAME} && \
    chown ${USERNAME}:${USERNAME} /etc/fahclient/config.xml && \
    rm -f /tmp/fahclient.deb && \
    apt-get autoremove -y && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/*

USER ${USERNAME}

# Switch back to dialog for any ad-hoc use of apt-get
ENV DEBIAN_FRONTEND=dialog

EXPOSE 36330

ENTRYPOINT ["sudo", "FAHClient", "--web-allow=0/0:7396", "--allow=0/0:36330"]
CMD ["--user=Anonymous", "--team=0", "--gpu=false", "--smp=true", "--power=full"]