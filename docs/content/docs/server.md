---
title: "Server"
type: docs
---

# Mooshak Server


## Setup

To setup a mooshak server you will need `git`, `docker` and `docker compose` installed.


Go to directory of your choice and clone the project.

```bash
$ git clone https://github.com/sepgh/mooshak
```

Next, go to newly cloned `mooshak` directory and then navigate to `server` directory. After that you can start `mooshak` by runing the docker compose:

```bash
$ cd mooshak  # cloned directory
$ cd server  # server scripts
$ docker compose up -d  # or: docker-compose up -d
```

Congradulations! You have mooshak ready to be used. The SSH server will be available on port `2255`, and websocket tunnel will be available on port `3344`.

To stop the mooshak server go to the same directory and then use:

```bash
$ docker compose down
```

## Run behind Nginx - Websocket

You can configure your Nginx setup to forward websocket connections to websocket port listened by Mooshak WsTunnel (`3344`).

Here is a sample path configuration to add to your Nginx setup:

```
location /mooshak {
        proxy_pass http://127.0.0.1:3344;
        proxy_http_version  1.1;
        proxy_set_header    Upgrade $http_upgrade;
        proxy_set_header    Connection "upgrade";
        proxy_set_header    Host $http_host;
        proxy_set_header    X-Real-IP $remote_addr;

        proxy_connect_timeout       10m;
        proxy_send_timeout          10m;
        proxy_read_timeout          90m;
        send_timeout                10m;
}
```

Reload your Nginx service and you are good to go.