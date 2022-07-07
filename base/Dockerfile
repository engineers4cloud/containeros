FROM alpine:latest AS build

ENV TOYBOX_VERSION 0.8.7
ENV DERIVATE base

RUN set -eux; \
	apk add --no-cache \
		bash \
		gcc \
		linux-headers \
		make \
		musl-dev \
	;

WORKDIR /toybox

RUN set -eux; \
	wget -O toybox.tgz "https://landley.net/toybox/downloads/toybox-$TOYBOX_VERSION.tar.gz"; \
	tar -xf toybox.tgz --strip-components=1; \
	rm toybox.tgz

# Make root-fs
RUN make root BUILTIN=1

# Building nopriv group/user for Container
RUN echo "nopriv:x:101:nopriv" >> /toybox/root/host/fs/etc/group \
&& echo "nopriv:x:101:101:nopriv:/dev/null:/sbin/nologin" >> /toybox/root/host/fs/etc/passwd

# rm links
RUN rm -f /toybox/root/host/fs/usr/sbin/* \
&& rm -f /toybox/root/host/fs/usr/bin/*

# cp new binaries and lib to fs
COPY binaries/$DERIVATE/* /toybox/root/host/fs/usr/bin/
RUN cp /lib/ld-musl-x86_64.so.1 /toybox/root/host/fs/lib/ \
&& chmod 555 /toybox/root/host/fs/usr/bin/*

# adjusting root-filesystem permissions
RUN chmod -R 700 /toybox/root/host/fs/root

FROM scratch
LABEL vendor="engineers4cloud" name="containeros" derivate="$DERIVATE" version="$TOYBOX_VERSION"
COPY --from=build /toybox/root/host/fs/ /
USER nopriv
CMD ["sh"]
