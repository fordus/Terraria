FROM alpine:3.11.6 AS base

#TShock URL
ENV TSHOCKURL="https://github.com/Pryaxis/TShock/releases/download/v4.5.17/TShock4.5.17_Terraria_1.4.3.6.zip"

RUN apk add --update-cache \
    unzip
ADD $TSHOCKURL /tshock.zip
RUN unzip /tshock.zip -d /tshock && \
    rm tshock.zip && \
    chmod +x /tshock/TerrariaServer.exe

FROM mono:6.10.0.104-slim

RUN apt-get update -y && \
    apt-get install apt-transport-https ca-certificates \
    apt-get install -y nuget && \
    rm -rf /var/lib/apt/lists/* /tmp/*

COPY --from=base /tshock/ /tshock/

RUN echo "mono --server --gc=sgen -O=all /tshock/TerrariaServer.exe \
    -configpath /root \
    -worldselectpath /root \
    -logpath /root/logs \
    $@" >> /tshock/START

VOLUME ["/root"]
EXPOSE 7777
WORKDIR /tshock
ENTRYPOINT [ "/bin/sh", "START"]
LABEL maintainer="github.com/fordus"
