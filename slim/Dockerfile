FROM alpine:latest AS build

RUN set -eux; \
	apk add --no-cache \
		bash \
		gcc \
		linux-headers \
		make \
		musl-dev \
	;

# This is for testing filesystem-settings
#RUN ls -l /etc/

WORKDIR /toybox

# https://landley.net/toybox/downloads/?C=M;O=D
# https://github.com/landley/toybox/releases
ENV TOYBOX_VERSION 0.8.7

RUN set -eux; \
	wget -O toybox.tgz "https://landley.net/toybox/downloads/toybox-$TOYBOX_VERSION.tar.gz"; \
	tar -xf toybox.tgz --strip-components=1; \
	rm toybox.tgz

# Loading .config
RUN make defconfig && make list
RUN make sh \
&& make ls \
&& make cat \
&& make chown \
&& make chmod \
&& make cp \
&& make pwd \
&& make ps \
&& make rm \
&& make id \
&& make env \
&& ls
#COPY /slim/config.slim /toybox/
#RUN mv config.slim .config

# Make root-fs
RUN make root BUILTIN=1

# Building nopriv group/user for Container
RUN echo "nopriv:x:101:nopriv" >> /toybox/root/host/fs/etc/group
RUN echo "nopriv:x:101:101:nopriv:/dev/null:/sbin/nologin" >> /toybox/root/host/fs/etc/passwd

# rm links and generate new binaries
WORKDIR /toybox/root/host/fs/usr/sbin
RUN rm -rf *
WORKDIR /toybox/root/host/fs/usr/bin
RUN ls -l
#RUN find . -type l -delete
RUN rm -rf *
WORKDIR toybox
RUN cp ls cat chmod chown cp ps pwd rm mv sh id env /toybox/root/host/fs/usr/bin/
RUN ls -l /toybox/root/host/fs/usr/bin/

# adjusting filesystem-permissions
RUN chmod -R 700 /toybox/root/host/fs/root

FROM scratch
COPY --from=build /toybox/root/host/fs/ /
USER nopriv
WORKDIR /
CMD ["sh"]
