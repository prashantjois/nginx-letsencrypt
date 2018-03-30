# nginx proxy with docker-gen and lets encrypt

## Introduction

I have a container that has my application.

It's a web application that I want reachable from the internet.

Normally, I'd need to:
  * Choose the ports I want to expose locally
  * Create an nginx config to proxy to that local port
  * Get an SSL certificate somehow

There has to be a better way!

## Yep

Some smart people created:

 * [Container discover](https://github.com/jwilder/docker-gen)
 * [Automation to generate proxy configs for nginx](https://github.com/jwilder/nginx-proxy)
 * [Free SSL certificates](https://letsencrypt.org/)
 * [Automation to generate and maintain SSL certs using Let's Encrypt](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion)

Let's take a minute to be humbled by the enormous contributions these folks have made.

## So why am I here?

All this project does is give you one file to run in order to get all of the above set up.

The one and only file will run containers for `jwilder/nginx-proxy` and `JrCs/docker-letsencrypt-nginx-proxy-companion`

To get up and running, all you need to do in your container is:
  * Expose the port you want to serve your web app (either through the `Dockerfile` or through `--expose` runtime command)
  * Add the following environment variables to your runtime command
    *  `-e VIRTUAL_HOST="example.com"`
    * `-e LETSENCRYPT_HOST="example.com"`
    * `-e LETSENCRYPT_EMAIL="joe@example.com"`

Make sure you run your own container ***AFTER*** the nginx containers. 
The nginx containers listen for changes to the docker environment, if your container is already running then it will not detect it.

## Spell it out for me doc ...

Here's a sample run command

```bash
  docker run -d \
   --expose 80 \
   -e VIRTUAL_HOST="example.com" \
   -e LETSENCRYPT_HOST="example.com" \
   -e LETSENCRYPT_EMAIL="joe@example.com" \
   .
   .
   .
   my_image
```
