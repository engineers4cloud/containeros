FROM alpine:latest AS build

ENV JRE_VER 
RUN apk add --no-cache wget

WORKDIR /jre
RUN wget -nv https://github.com/adoptium/temurin17-binaries/releases/download/jdk-17.0.3%2B7/OpenJDK17U-jre_x64_alpine-linux_hotspot_17.0.3_7.tar.gz \
&& ls -l \
&& tar xzf OpenJDK17U-jre_x64_alpine-linux_hotspot_17.0.3_7.tar.gz \
&& ls -l


FROM engineers4cloud/containeros:base-0.8.7
USER nopriv
CMD ["sh"]
