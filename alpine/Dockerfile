FROM amd64/alpine:3

LABEL maintainer="Florian JUDITH <florian.judith.b@gmail.com>"

ARG FAH_MAJOR_VERSION=7.5
ARG FAH_MINOR_VERSION=7.5.1

ENV HOMEDIR=/home/fah \
    PATH=${PATH}:${HOMEDIR}

RUN adduser -D fah && \
    apk add --no-cache curl ca-certificates bzip2 && \
    mkdir -p ${HOMEDIR}/bin /etc/fahclient

WORKDIR ${HOMEDIR}

RUN curl https://download.foldingathome.org/releases/public/release/fahclient/debian-stable-64bit/v${FAH_MAJOR_VERSION}/fahclient_${FAH_MINOR_VERSION}-64bit-release.tar.bz2 | tar jxvf - && \
    mv -vf fahclient_${FAH_MINOR_VERSION}-64bit-release/FAHClient ./bin/ && \
    mv -vf fahclient_${FAH_MINOR_VERSION}-64bit-release/FAHCoreWrapper ./bin/ && \
    chown fah:fah ./bin/FAH* && \
    chmod +x ./bin/FAH* && \
    mv -vf fahclient_${FAH_MINOR_VERSION}-64bit-release/sample-config.xml /etc/fahclient/config.xml && \
    rm -vrf fahclient_${FAH_MINOR_VERSION}-64bit-release

USER fah

# Web viewer
EXPOSE 7396
EXPOSE 36330

ENTRYPOINT ["./bin/FAHClient", "--web-allow=0/0:7396", "--allow=0/0:36330"]
CMD ["--user=Anonymous", "--team=0", "--gpu=false", "--smp=true", "--power=full"]