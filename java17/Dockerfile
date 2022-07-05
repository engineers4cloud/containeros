FROM alpine:latest AS build

RUN apk add --no-cache wget

WORKDIR /jre
RUN wget -nv https://github.com/adoptium/temurin17-binaries/releases/download/jdk-17.0.3%2B7/OpenJDK17U-jre_x64_alpine-linux_hotspot_17.0.3_7.tar.gz \
&& ls -l \
&& tar xzf OpenJDK17U-jre_x64_alpine-linux_hotspot_17.0.3_7.tar.gz \
&& mv jdk-17* jre17 && rm -f *.tar.gz


FROM engineers4cloud/containeros:slim-0.8.7
COPY --from=build /jre /
COPY --from=build /lib/libz.so.1 /lib
ENV PATH /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/jre17/bin
USER nopriv
CMD ["sh"]
