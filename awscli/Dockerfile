FROM alpine:latest AS builder

RUN apk-add --no-cache aws-cli


FROM containeros:base-0.8.7
USER nopriv
CMD ["sh"]
