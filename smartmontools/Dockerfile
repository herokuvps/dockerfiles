FROM alpine:3.8

ARG VERSION

LABEL maintainer="zgist" \
        org.label-schema.name="Smartmontools" \
        org.label-schema.version=$VERSION

# Let's roll
RUN set -xe && \
    apk add --no-cache \
    smartmontools ssmtp

ENTRYPOINT ["/usr/sbin/smartd", "--debug"]
