![](https://images.microbadger.com/badges/version/gists/shadowsocks-libev.svg) ![](https://images.microbadger.com/badges/image/gists/shadowsocks-libev.svg) ![](https://img.shields.io/docker/stars/gists/shadowsocks-libev.svg) ![](https://img.shields.io/docker/pulls/gists/shadowsocks-libev.svg)

#### Environment:

| Environment | Default value |
|-------------|---------------|
| SERVER_ADDR | 0.0.0.0       |
| SERVER_PORT | 8388          |
| PASSWORD    | $(hostname)   |
| METHOD      | rc4-md5       |
| TIMEOUT     | 300           |
| DNS_ADDR    | 8.8.8.8       |
| DNS_ADDR_2  | 8.8.4.4       |

#### Creating an instance:

    docker run \
        -d \
        --name shadowsocks \
        -p 12345:8388 \
        -p 12345:8388/udp \
        -e PASSWORD=password \
        -e METHOD=chacha20-ietf-poly1305
        gists/shadowsocks-libev

#### Compose example:

    shadowsocks:
        image: gists/shadowsocks-libev
        ports:
            - "12345:8388/tcp"
            - "12345:8388/udp"
        environment:
            - PASSWORD=password
            - METHOD=chacha20-ietf-poly1305
      restart: always

#### Compose file whith own command

    shadowsocks:
        image: gists/shadowsocks-libev
        ports:
            - "12345:8388/tcp"
            - "12345:8388/udp"
        command: ss-server --fast-open -s 0.0.0.0 -p 12345 -k password -m chacha20-ietf-poly1305 -t 300 -d 8.8.8.8 --no-delay -u
        restart: always

#### Compose example with simple-obfs

    version: '3'

    services:
        shadowsocks:
            container_name: shadowsocks
            image: gists/shadowsocks-libev
            ports:
                - "12345:8388/udp"
            networks:
                overlay:
            environment:
              - PASSWORD=passowrd
              - METHOD=chacha20-ietf-poly1305
            restart: always

          simple-obfs:
            container_name: obfs
            image: gists/simple-obfs
            ports:
                - "12345:8388/tcp"
            environment:
                - FORWARD=shadowsocks:8388
            depends_on:
                - shadowsocks
            networks:
                overlay:
            restart: always

    networks:
        overlay:
            driver: bridge
