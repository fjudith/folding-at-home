FROM nvidia/cuda:9.2-base
LABEL maintainer="Florian JUDITH <florian.judith.b@gmail.com>"

# fahclient version
ENV FAH_VERSION_MINOR=7.5.1
ENV FAH_VERSION_MAJOR=7.5

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

# This Dockerfile adds a non-root user with sudo access.
ARG USERNAME=fah
ARG USERNAME_UID=1000
ARG USERNAME_GID=${USERNAME_UID}

ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES compute,utility

RUN apt-get -qq update &&\
    apt-get -y install --no-install-recommends \
        bzip2 \
        ca-certificates \
        clinfo \
        curl \
        ocl-icd-libopencl1 &&\
    mkdir --parents /etc/OpenCL/vendors &&\
        ln --symbolic /usr/lib/x86_64-linux-gnu/libOpenCL.so.1 /usr/lib/x86_64-linux-gnu/libOpenCL.so &&\
        echo "libnvidia-opencl.so.1" > /etc/OpenCL/vendors/nvidia.icd &&\
    curl --output /tmp/fah.deb https://download.foldingathome.org/releases/public/release/fahclient/debian-stable-64bit/v${FAH_VERSION_MAJOR}/fahclient_${FAH_VERSION_MINOR}_amd64.deb &&\
        mkdir --parents /etc/fahclient/ &&\
        touch /etc/fahclient/config.xml &&\
        dpkg --install /tmp/fah.deb &&\
    groupadd --gid ${USERNAME_GID} ${USERNAME} && \
    useradd -s /bin/bash --uid ${USERNAME_UID} --gid ${USERNAME_GID} -m ${USERNAME} && \
    echo ${USERNAME} ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/${USERNAME} && \
    chmod 0440 /etc/sudoers.d/${USERNAME} && \
    chmod 700 /home/${USERNAME}  &&\
    apt-get -qq remove --assume-yes curl &&\
        apt-get -qq autoremove --assume-yes &&\
        rm -rf /tmp/* /var/log/* /var/lib/apt/ /var/cache/apt/

# Web viewer
EXPOSE 7396

WORKDIR /home/${USERNAME}
USER ${USERNAME}

ENTRYPOINT ["FAHClient", "--web-allow=0/0:7396", "--allow=0/0:7396"]
CMD ["--user=Anonymous", "--team=0", "--power=full"]