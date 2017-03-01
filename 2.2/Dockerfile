# CUPS build from source on Alpine 3.5
FROM alpine:3.5

# Security-related CFLAGS
ENV CFLAGS "-fstack-protector-strong -fpic -Wl,-O1 -O2 -Wl,--hash-style=both"
ENV CPPFLAGS "$CFLAGS"
ENV LDFLAGS "-O1 --hash-style=both"

ENV CUPS_VER=2.2.2

ENV CUPS_SRC="https://github.com/apple/cups/releases/download/v${CUPS_VER}/cups-${CUPS_VER}-source.tar.gz"
ENV CUPS_SIG="https://github.com/apple/cups/releases/download/v${CUPS_VER}/cups-${CUPS_VER}-source.tar.gz.sig"

RUN \
     apk add --no-cache --update openssl \
  && apk add --no-cache --virtual .build-deps libgcypt-dev gnutls-dev \
     zlib-dev libpaper-dev dbus-dev libjpeg-turbo-dev avahi-dev \
     linux-headers gcc libc-dev make g++ \
  && wget "${CUPS_SRC}" \
  && ./configure \
     --prefix=/usr/local \
     --with-cups-user=lp \
     --with-cups-group=lp \
     --with-system-groups=lpadmin \
     --without-php \
     --disable-pam \
     --enable-raw-printing \
     --enable-dbus \
     --with-dbusdir=/etc/dbus-1 \
     --enable-libpaper \
     --enable-ssl=yes \
     --enable-gnutls \
     --with-optim="$CFLAGS" \
  && make install 

# IPP
EXPOSE 631
