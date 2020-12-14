# Aitubox

Based on seedbox repo from jfroment

## Included Applications

| Application          | Web Interface              | Docker image                                                           | Version (image tag) | Notes               |
-----------------------|----------------------------|------------------------------------------------------------------------|-------------------------|---------------------|
| Plex                 | plex.yourdomain.com        | [linuxserver/plex](https://hub.docker.com/r/linuxserver/plex)          | *latest*                | Media Streaming     |
| Deluge               | deluge.yourdomain.com      | [linuxserver/deluge](https://hub.docker.com/r/linuxserver/deluge)      | *latest*                | Torrents downloader |
| Sonarr               | sonarr.yourdomain.com      | [linuxserver/sonarr](https://hub.docker.com/r/linuxserver/sonarr)      | *preview*               | TV Shows monitor    |
| Radarr               | radarr.yourdomain.com      | [linuxserver/radarr](https://hub.docker.com/r/linuxserver/radarr)      | *develop*                | Movies monitor      |
| Bazarr               | bazarr.yourdomain.com      | [linuxserver/bazarr](https://hub.docker.com/r/linuxserver/bazarr)      | *latest*                | Subtitles monitor   |
| Lidarr               | lidarr.yourdomain.com      | [linuxserver/lidarr](https://hub.docker.com/r/linuxserver/lidarr)      | *preview*               | Music monitor       |
| Jackett              | jackett.yourdomain.com     | [linuxserver/jackett](https://hub.docker.com/r/linuxserver/jackett)    | *latest*                | Tracker indexer     |
| JDownloader          | jdownloader.yourdomain.com | [jlesage/jdownloader-2](https://hub.docker.com/r/jlesage/jdownloader-2)| *latest*                | Direct downloader   |
| Tautulli (plexPy)    | tautulli.yourdomain.com    | [linuxserver/tautulli](https://hub.docker.com/r/linuxserver/tautulli)  | *latest*                | Plex stats and admin|
| Tdarr            | tdarr.yourdomain.com   | [haveagitgat/tdarr](https://hub.docker.com/r/haveagitgat/tdarr)  | *latest*                | Re-encode files |
| NextCloud            | nextcloud.yourdomain.com   | [linuxserver/nextcloud](https://hub.docker.com/r/linuxserver/nextcloud)  | *latest*                | Files management    |
| NextCloud-db (MariaDB) | not reachable   | [mariadb](https://hub.docker.com/r/_/mariadb)  | *10*                | DB for Nextcloud    |
| Portainer            | portainer.yourdomain.com   | [portainer/portainer](https://hub.docker.com/r/portainer/portainer)    | *latest*                | Container management|
| Netdata              | netdata.yourdomain.com     | [netdata/netdata](https://hub.docker.com/r/netdata/netdata)            | *latest*                | Server monitoring   |
| Duplicati            | duplicati.yourdomain.com   | [linuxserver/duplicati](https://hub.docker.com/r/linuxserver/duplicati)| *latest*                | Backups             |

The front-end reverse proxy (Traefik) routes based on the lowest level subdomain
 (e.g. `deluge.example.com` would route to deluge). Since this is how the router
works, it is recommended for you to get a top level domain. If you do not have
one, you can edit your domains locally by changing your hosts file or use a
browser plugin that changes the host header.

Traefik takes care of valid Let's Encrypt certificates and auto-renewal.

Note: Plex is also available directly through the `32400` port without going
through the reverse proxy.

## Dependencies

- [Docker](https://github.com/docker/docker) >= 1.13.0
    + Install guidelines for Ubuntu 20.04: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04-fr
- [Docker Compose](https://github.com/docker/compose) >=v1.10.0
    + Install guidelines for Ubuntu 20.04: https://www.digitalocean.com/community/tutorials/how-to-install-docker-compose-on-ubuntu-20-04-quickstart-fr
- [local-persist Docker plugin](https://github.com/CWSpear/local-persist): installed directly on host (not in container). This is a volume plugin that extends the default local driver’s functionality by allowing you specify a mountpoint anywhere on the host, which enables the files to always persist, even if the volume is removed via `docker volume rm`. Use *systemd* install for Ubuntu 16.04.

## Configuration

Before running, please create the volumes which will be statically mapped to the ones on the host:

```sh
sudo su -c "mkdir /data && mkdir /data/config && mkdir /data/torrents && mkdir /data/config/traefik && touch /data/config/traefik/acme.json"
./init.sh
```

Edit the `.env` file and change the variables as desired.
The variables are all self-explanatory.

## Running & updating

```sh
./update-all.sh
```

docker-compose should manage all the volumes and network setup for you. If it
does not, verify that your docker and docker-compose version is updated.

Make sure you install the dependencies and finish configuration before doing
this.

## Where is my data?

All data is saved in the docker volumes `seedbox_config` or
`seedbox_torrents`.
These volumes are mapped to the `config` and `torrents` folders located in `/data` on the host. You can change these static paths in the docker-compose.yml file.
Thanks to the **local-persist** Docker plugin, the data located in these volumes is persistent, meaning that volumes are not deleted, even when using the ```docker-compose down``` command. It would be a shame to loose everything by running a simple docker command ;-)
