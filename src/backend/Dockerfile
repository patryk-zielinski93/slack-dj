FROM node:carbon
ARG LOCAL_USER_ID

RUN echo "deb http://ftp.fr.debian.org/debian jessie-backports main" >> /etc/apt/sources.list
RUN apt-get update -y && apt-get upgrade -y && apt-get install -y --fix-missing build-essential gcc g++ ca-certificates curl dos2unix ffmpeg sox libsox-fmt-mp3 mp3info alsa-base alsa-utils libasound2-dev

# Handle permissions issues on unix systems
RUN gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4
RUN curl -o /usr/local/bin/gosu -SL "https://github.com/tianon/gosu/releases/download/1.4/gosu-$(dpkg --print-architecture)" \
    && curl -o /usr/local/bin/gosu.asc -SL "https://github.com/tianon/gosu/releases/download/1.4/gosu-$(dpkg --print-architecture).asc" \
    && gpg --verify /usr/local/bin/gosu.asc \
    && rm /usr/local/bin/gosu.asc \
    && chmod +x /usr/local/bin/gosu

RUN npm install -g 'nodemon@^1.17.5' 'typescript@2.7.2' 'forever'
RUN mkdir -p /app

COPY ./docker /var/docker
RUN find /var/docker -name "*.sh" | xargs dos2unix

# Handle permissions
WORKDIR /var/docker/dev
RUN ./handle-permissions.sh

WORKDIR /opt
RUN curl -L http://ftp.pl.debian.org/debian/pool/main/m/mp3gain/mp3gain_1.5.2-r2-2+deb7u1_amd64.deb > mp3gain.deb
RUN dpkg -i mp3gain.deb
WORKDIR /app

EXPOSE 58585
EXPOSE 58586
EXPOSE 58587
EXPOSE 8888

ENTRYPOINT ["/var/docker/dev/entrypoint.sh"]

CMD ["sh", "/var/docker/dev/start.sh"]
