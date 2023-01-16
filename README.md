
# Nextcloud Images with a Twist

Contains different flavors of the latest Nextcloud images with added features. Feel free to use them as you please or clone this repo and build them yourself

The images themselves can be found [here on Docker Hub](https://hub.docker.com/r/bambam018/nextcloud)

**Flavors:**
- [nc-full-storj-app-apache](/nc-apache-full-storjapp/)
  - Adds the required dependencies for all optional packages suggested by nextcloud to the official nextcloud base image.
  - Adds cron functionality via supervisor so no extra container is needed.
  - Adds and enables the Storj app so that it can be used as a main storage option if desired.
  - [Example docker-compose file](/nc-apache-full-storjapp/docker-compose.yml) with redis and postgres for the backend
  - [Dockerfile is here](/nc-apache-full-storjapp/Dockerfile) if you would like to inspect.
  - click here for more [details on github](https://github.com/bamtests/nextcloud)

Credit to [Storj](https://github.com/storj-thirdparty/nextcloud-app) and [Nextcloud](https://github.com/nextcloud/docker) for creating awesome software, I just put it together in to one.
