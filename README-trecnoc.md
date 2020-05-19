# Trecnoc Production Docker deployment for Mattermost

## Installation using Docker Compose

The following instructions deploy Mattermost in a production configuration using multi-node Docker Compose set up.

### Requirements

* [docker] (version `1.12+`)
* [docker-compose] (version `1.10.0+` to support Compose file version `3.0`)

### Starting/Stopping Docker

#### Configure Variables

Create an env file for storing the Mattermost environment variables required for Nginx and the LetsEncrypt containers

`vim mattermost-variables.env`

Add the following variables to the file:

* VIRTUAL_HOST
* LETSENCRYPT_HOST

Both should be set to the same value being the DNS entry for the Mattermost server.

Create an env file for LetsEncrypt

`vim letsencrypt-variables.env`

Add the following variable to the file:

* DEFAULT_EMAIL

This should contain an email address for receiving LetsEncrypt notifications

#### Start
If you are running docker with non root user, make sure the UID and GID in app/Dockerfile are the same as your current UID/GID
```
mkdir -p ./volumes/app/mattermost/{data,logs,config,plugins}
mkdir -p ./volumes/proxy/{nginx_certs,nginx_conf,nginx_html,nginx_vhost}
chown -R 1000:1000 ./volumes/
docker-compose build
docker-compose up -d
```

#### Stop
```
docker-compose down
```

## Update Mattermost to latest version

First, shutdown your containers to back up your data.

```
docker-compose down
```

Back up your mounted volumes to save your data. If you use the default `docker-compose.yml` file proposed on this repository, your data is on `./volumes/` folder.

Then run the following commands.

```
git pull
docker-compose build
docker-compose up -d
```

Your Docker image should now be on the latest Mattermost version.

[docker]: http://docs.docker.com/engine/installation/
[docker-compose]: https://docs.docker.com/compose/install/
