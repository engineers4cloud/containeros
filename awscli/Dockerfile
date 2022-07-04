FROM alpine:latest AS builder

RUN apk add --no-cache aws-cli


FROM engineers4cloud/containeros:base-0.8.7
USER nopriv
CMD ["sh"]
