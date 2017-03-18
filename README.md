# Reversed Proxy with Pen

## Add proxy servers

edit `servers`, add/remove servers

## Build Docker image

run `./build.sh` will create image `rproxy`

## Start docker

  docker run -it --rm -p 127.0.0.1:8080:8080 rproxy -r 

## Check if it works

  http_proxy=http://localhost:8080 curl ip.cn
