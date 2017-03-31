# Reversed Proxy with Pen

## Add proxy servers

edit `servers`, add/remove servers

## Build Docker image

run `./build.sh` will create image `rproxy`

## Start docker

    docker run -it --rm -p 127.0.0.1:8080:8080 rproxy [options]

Options is passing to `pen`, useful options from `pen:0.34.1`:

  -T sec    tracking time in seconds (0 = forever) [0]
  -H	add X-Forwarded-For header in http requests
  -b sec    blacklist time in seconds [30]
  -c N      max number of clients [2048]
  -d	debugging on (repeat -d for more)
  -r	bypass client tracking in server selection
  -t sec    connect timeout in seconds [3]
  -x N      max number of simultaneous connections [500]

`-r` is useful for load balance:

    docker run -it --rm -p 127.0.0.1:8080:8080 rproxy -r

## Check if it works

    http_proxy=http://localhost:8080 curl ip.cn
