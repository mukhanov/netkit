FROM vimagick/dante

ARG USER=user
ARG PASS=pass

RUN useradd ${USER} \
    && echo ${USER}:${PASS} | chpasswd

COPY sockd.conf /etc/sockd.conf