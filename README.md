[<img src="https://hotio.dev/img/overseerr.png" alt="logo" height="130" width="130">](https://github.com/sct/overseerr)

[![GitHub Source](https://img.shields.io/badge/github-source-ffb64c?style=flat-square&logo=github&logoColor=white&labelColor=757575)](https://github.com/hotio/overseerr)
[![GitHub Registry](https://img.shields.io/badge/github-registry-ffb64c?style=flat-square&logo=github&logoColor=white&labelColor=757575)](https://github.com/orgs/hotio/packages/container/package/overseerr)
[![Docker Pulls](https://img.shields.io/docker/pulls/hotio/overseerr?color=ffb64c&style=flat-square&label=pulls&logo=docker&logoColor=white&labelColor=757575)](https://hub.docker.com/r/hotio/overseerr)
[![Discord](https://img.shields.io/discord/610068305893523457?style=flat-square&color=ffb64c&label=discord&logo=discord&logoColor=white&labelColor=757575)](https://hotio.dev/discord)
[![Upstream](https://img.shields.io/badge/upstream-project-ffb64c?style=flat-square&labelColor=757575)](https://github.com/sct/overseerr)
[![Website](https://img.shields.io/badge/website-hotio.dev-ffb64c?style=flat-square&labelColor=757575)](https://hotio.dev/containers/overseerr)

## Starting the container

Just the basics to get the container running:

```shell hl_lines="4 5 6 7 8 9"
docker run --rm \
    --name overseerr \
    -p 5055:5055 \
    -e PUID=1000 \
    -e PGID=1000 \
    -e UMASK=002 \
    -e TZ="Etc/UTC" \
    -e ARGS="" \
    -e DEBUG="no" \
    -v /<host_folder_config>:/config \
    hotio/overseerr
```

The [highlighted](https://hotio.dev/containers/overseerr) variables are all optional, the values you see are the defaults. In most cases you'll need to add an additional volume (`-v`) or more, depending on your own personal preference, to get access to additional files.

## Tags

| Tag                | Upstream                           | Version | Build |
| -------------------|------------------------------------|---------|-------|
| `release` (latest) | GitHub releases                    | ![version](https://img.shields.io/badge/dynamic/json?color=f5f5f5&style=flat-square&label=&query=%24.version&url=https%3A%2F%2Fraw.githubusercontent.com%2Fhotio%2Foverseerr%2Frelease%2FVERSION.json) | ![build](https://img.shields.io/github/workflow/status/hotio/overseerr/build/release?style=flat-square&label=) |
| `nightly`          | Every commit to develop branch | ![version](https://img.shields.io/badge/dynamic/json?color=f5f5f5&style=flat-square&label=&query=%24.version&url=https%3A%2F%2Fraw.githubusercontent.com%2Fhotio%2Foverseerr%2Fnightly%2FVERSION.json) | ![build](https://img.shields.io/github/workflow/status/hotio/overseerr/build/nightly?style=flat-square&label=) |

You can also find tags that reference a commit or version number.

## Configuration location

Your overseerr configuration inside the container is stored in `/config/app`, to migrate from another container, you'd probably have to move your files from `/config` to `/config/app`.

## Using a secure Plex connection

If you want to keep using secure connections within Plex, but don't want to buy your own domain and keep the connection between Overseerr and Plex inside of their Docker network. Follow the below procedure.

We'll use Google Chrome in this example. Visit `https://app.plex.tv` and make sure you are logged in. Open Chrome DevTools (usually F12) and open the `Console` tab, then refresh your browser window. One of the very first lines you will see is `[Servers] Initialize server with token, ...`, in that message you should see some url that looks like `https://10-1-0-100.xxxxxxxxxxxxx.plex.direct:32400`. Part of that url can be used in your Overseerr settings, the part `10-1-0-100.xxxxxxxxxxxxx.plex.direct`is what you'll need to copy/paste, the port is in a seperate input box and enable SSL. You should however give the Plex container a static IP if you don't wanna do this every 5 minutes.

## Executing your own scripts

If you have a need to do additional stuff when the container starts or stops, you can mount your script with `-v /docker/host/my-script.sh:/etc/cont-init.d/99-my-script` to execute your script on container start or `-v /docker/host/my-script.sh:/etc/cont-finish.d/99-my-script` to execute it when the container stops. An example script can be seen below.

```shell
#!/usr/bin/with-contenv bash

echo "Hello, this is me, your script."
```

## Troubleshooting a problem

By default all output is redirected to `/dev/null`, so you won't see anything from the application when using `docker logs`. Most applications write everything to a log file too. If you do want to see this output with `docker logs`, you can use `-e DEBUG="yes"` to enable this.
