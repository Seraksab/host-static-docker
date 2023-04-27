# Host static websites in Docker

This repository contains an example docker-compose setup on how to host multiple static websites using a dedicated
lighttpd container for each site behind a nginx proxy with automated SSL support. Includes an SFTP server to manage the
content of the websites.

## Disclaimer

This setup is based on a configuration that we successfully deployed and that is meeting our requirements so far.  
This is NOT necessarily the most ideal or perfect solution. It's shared solely with the intent to provide an
example/entrypoint on how to setup multiple static websites behind a proxy in docker. It's expected that you tweak the
configuration for your specific needs.

## Configuration

The given configuration assumes that the containers are deployed on a Linux host and the hosted websites are simple
static HTML pages. PHP is not supported in this configuration.

The expires headers for cache control are set pretty far in the future. If you don't control versioning with
filename-based cache busting, lower the cache time for those resources.

The [nginx config](nginx/nginx.conf) as well as the [lighttpd config](lighttpd/lighttpd.conf) contain custom adaptations
and are **NOT the default configuration**.  
Depending on your use case these might need to be adapted.

## Usage

1. If required: Add additional lighttpd instances in [docker-compose.web.yml](docker-compose.web.yml) depending on the
   number of sites to be hosted.  
   Make sure to also add a volume for each new lighttpd instance and mount that volume to
   the SFTP container.
2. Edit the [.env](.env) file and add your mail, domains and chosen SFTP credentials.
3. Bring up the nginx stack first: `docker-compose -p nginx -f docker-compose.nginx.yml --env-file .env up -d`
4. Bring up the web stack: `docker-compose -p web -f docker-compose.web.yml --env-file .env up -d`
5. Access the SFTP server by your selected credentials and upload the content for your websites.
