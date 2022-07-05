FROM alpine:latest AS build

RUN apk add --no-cache wget

WORKDIR /jre
RUN wget OpenJDK17U-jre_x64_alpine-linux_hotspot_17.0.3_7.tar.gz && ls -l


FROM engineers4cloud/containeros:base-0.8.7
USER nopriv
CMD ["sh"]
