# Reversed Proxy with Pen

## Build Docker image

edit `servers`, add/remove proxy servers  
run `./build.sh` will create image `xiaohuizhang/rproxy`

## Start docker

    docker run -it --rm -p 127.0.0.1:8080:8080 xiaohuizhang/rproxy [options]

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

    docker run -it --rm -p 127.0.0.1:8080:8080 xiaohuizhang/rproxy -r

Use your own servers for proxying:

    mkdir ~/rproxy
    cat <<LINES > ~/rproxy/servers
    server 0 address 62.210.169.105 port 2366
    server 1 address 62.210.169.105 port 2367
    LINES
    docker run -it --rm -p 127.0.0.1:8080:8080 -v "`realpath ~/rproxy/servers`:/etc/pen/servers" xiaohuizhang/rproxy -r

Keep docker rproxy always running:

    docker run -d --name rproxy --restart=always -p 8080:8080 xiaohuizhang/rproxy -r

## Check if it works

    http_proxy=http://localhost:8080 curl ip.cn
