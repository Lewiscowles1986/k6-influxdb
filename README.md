# K6 to influxDB (2.0) with Grafana

This repo is me messing around with K6, influxDB and Grafana to attempt to automate some of the mundane setup, and document.

## Pre requisites

* A *nix system (Linux or OSX should do)
* Homebrew or relevant package manager, `dpkg`, `rpm`, `yum`, `apk`, `aur`, `pacman`, etc
* build-essential
* golang
* docker
* k6

### Build required binary

### Golang setup
```shell
go_version=$(go version | cut -d ' ' -f 3 | sed 's/go//')
export GOPATH=$HOME/go
export GOROOT=/opt/homebrew/Cellar/go/${go_version}/libexec
export PATH=$PATH:$GOPATH/bin
export PATH=$PATH:$GOROOT/bin
```

### Build k6

```shell
go install go.k6.io/xk6/cmd/xk6@latest
xk6 build --with github.com/grafana/xk6-output-influxdb
```

## using

### starting docker

```shell
docker compose up -d
```

### executing

```
K6_INFLUXDB_ORGANIZATION="my-org" \
K6_INFLUXDB_BUCKET="k6" \
K6_INFLUXDB_TOKEN="my-super-secret-auth-token" \
K6_INFLUXDB_ADDR="http://127.0.0.1:8086" \
K6_INFLUXDB_INSECURE=true ./k6 run script.js --out "xk6-influxdb"
```

### Cleaning up

```shell
docker compose down --remove-orphans
rm -rf data/grafana
git checkout main data
docker volume prune -f
```
