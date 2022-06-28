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

#RUN make defconfig && cat .config
#RUN rm -f .config
#COPY /slim/config /toybox/
#RUN mv config .config

# Make root-fs
RUN make root BUILTIN=1

# Building nopriv group/user for Container
RUN echo "nopriv:x:101:nopriv" >> /toybox/root/host/fs/etc/group
RUN echo "nopriv:x:101:101:nopriv:/dev/null:/sbin/nologin" >> /toybox/root/host/fs/etc/passwd

# rm links and generate new ones
WORKDIR /toybox/root/host/fs/usr/sbin
RUN rm -rf *
WORKDIR /toybox/root/host/fs/usr/bin
RUN ls -l
RUN find . -type l -delete
RUN ln -s ../../bin/toybox id \
&& ln -s toybox bash \
&& ln -s toybox ls \
&& ln -s toybox cat \
&& ln -s toybox chmod \
&& ln -s toybox chown \
&& ln -s toybox cp \
&& ln -s toybox ps \
&& ln -s toybox pwd \
&& ln -s toybox rm \
&& ln -s toybox mv \
&& ln -s ../../bin/toybox env
RUN ls -l

# adjusting filesystem-permissions
RUN chmod -R 700 /toybox/root/host/fs/root

FROM scratch
COPY --from=build /toybox/root/host/fs/ /
USER nopriv
CMD ["bash"]
