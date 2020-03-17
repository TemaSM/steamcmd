############################################################
# Dockerfile that contains SteamCMD
############################################################
FROM debian:buster-slim

LABEL maintainer="docker.tema@smirnov.one"
ARG PUID=1000

ENV STEAMCMDDIR /usr/local/bin/steamcmd

# Install, update & upgrade packages
# Create user for the server
# This also creates the home directory we later need
# Clean TMP, apt-get cache and other stuff to make the image smaller
# Create Directory for SteamCMD
# Download SteamCMD
# Extract and delete archive
RUN set -x \
  # Add i386 so we can install Wine downstream
  && dpkg --add-architecture i386 \
  && apt-get update \
  && apt-get install -y --no-install-recommends --no-install-suggests \
    lib32gcc1 \
    lib32stdc++6 \
    wget \
    ca-certificates \
  && useradd -u $PUID -m steam \
  && su steam -c \
    "mkdir -p ${STEAMCMDDIR} \
    && cd ${STEAMCMDDIR} \
    && wget -qO- 'https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz' | tar zxf -" \
  && chmod +x steamcmd.sh
  && apt-get remove --purge -y \
    wget \
  && apt-get clean autoclean \
  && apt-get autoremove -y \
  && rm -rf /var/lib/apt/lists/*

# Switch to user steam
USER steam

WORKDIR $STEAMCMDDIR

VOLUME $STEAMCMDDIR
