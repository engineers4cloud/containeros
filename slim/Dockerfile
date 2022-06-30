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
# https://github.com/landley/toybox/releases
ENV TOYBOX_VERSION 0.8.7

RUN set -eux; \
	wget -O toybox.tgz "https://landley.net/toybox/downloads/toybox-$TOYBOX_VERSION.tar.gz"; \
	tar -xf toybox.tgz --strip-components=1; \
	rm toybox.tgz

# Make root-fs
RUN make defconfig
RUN make root BUILTIN=1

# Building nopriv group/user for Container
RUN echo "nopriv:x:101:nopriv" >> /toybox/root/host/fs/etc/group
RUN echo "nopriv:x:101:101:nopriv:/dev/null:/sbin/nologin" >> /toybox/root/host/fs/etc/passwd

# rm links and compiling new binaries
WORKDIR /toybox/root/host/fs/usr/sbin
RUN rm -rf *
WORKDIR /toybox/root/host/fs/usr/bin
RUN rm -rf *
WORKDIR /toybox
RUN make change sh \
&& make change ls \
&& make change cat \
&& make change ps \
&& make change id \
&& make change env

# cp new binaries and lib to fs
RUN cp sh ls cat ps id env /toybox/root/host/fs/usr/bin/ \
&& cp /lib/ld-musl-x86_64.so.1 /toybox/root/host/fs/lib/

# adjusting root-filesystem permissions
RUN chmod -R 700 /toybox/root/host/fs/root

FROM scratch
COPY --from=build /toybox/root/host/fs/ /
USER nopriv
CMD ["sh"]
