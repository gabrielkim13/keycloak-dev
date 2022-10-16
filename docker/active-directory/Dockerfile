FROM ubuntu:22.04

ENV SMB_ADMIN_PASSWORD=Pa\$\$w0rd

ARG DEBIAN_FRONTEND=noninteractive
RUN apt update && apt install -y \
    samba \
    krb5-config \
    winbind \
    smbclient \
    iproute2 \
    openssl \
 && rm /etc/krb5.conf \
 && mkdir -p /opt/ad-scripts

WORKDIR /opt/ad-scripts

COPY . .

CMD chmod +x *.sh && ./samba-ad-setup.sh && ./samba-ad-run.sh

EXPOSE 389
EXPOSE 636
