FROM alpine:latest AS build

RUN set -eux; \
	apk add --no-cache \
		bash \
		gcc \
		linux-headers \
		make \
		musl-dev \
	;

WORKDIR /toybox

# https://landley.net/toybox/downloads/?C=M;O=D
## https://github.com/landley/toybox/releases
ENV TOYBOX_VERSION 0.8.7

RUN set -eux; \
	wget -O toybox.tgz "https://landley.net/toybox/downloads/toybox-$TOYBOX_VERSION.tar.gz"; \
	tar -xf toybox.tgz --strip-components=1; \
	rm toybox.tgz; \

# Make root-fs
  make root BUILTIN=1; \

# Building nopriv group/user for Container
  echo "nopriv:x:101:nopriv" >> /toybox/root/host/fs/etc/group; \
  echo "nopriv:x:101:101:nopriv:/dev/null:/sbin/nologin" >> /toybox/root/host/fs/etc/passwd; \

# adjusting root-filesystem permissions
  chmod -R 700 /toybox/root/host/fs/root; \
  
# rm sbin-commands
  rm -f /toybox/root/host/fs/usr/sbin/*

FROM scratch
COPY --from=build /toybox/root/host/fs/ /
USER nopriv
CMD ["sh"]
