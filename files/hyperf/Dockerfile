FROM hyperf/hyperf:7.4-alpine-v3.12-swoole
LABEL maintainer="ttyyqq"

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories
RUN apk update && apk add vim lsof
