# s6-php-nginx-fpm-base

## usage

This is a simple container with nginx and php-fpm based on alpine linux.
To install all the dependencies and build the container run:

```bash
make up
```

## What is this about
We often have the case that we need to deploy our PHP application to `$server`, but since PHP does not come with its own
webserver we always run a second container which passes the requests to our PHP process.

## What are the components
We installed just an index file in the `app` directory to showcase a running php application.

- Alpine
- s6-overlay
- nginx
- php-fpm

We use preconfigured user `www-data` and install the app in `var/www/html`.

## s6-overlay
- starts PID 1 with `ENTRYPOINT ["/init"]`
- starts all services and scripts that are defined in `/etc/s6-overlay/s6-rc.d/user/contents.d`

### Feature Flags
Sometimes you need to enable or disable services. 

> s6-rc wasn't made for dynamic service sets; it was made for reliability and predictability of the machine state, 
> which runtime changes are bad for - and you're going against its design by attempting to conditionally start services.

> The way to achieve what you want is by making sure that only the services you want to activate are in s6-rc's user 
> bundle, which contains everything that will be started. For that purpose, you have the S6_STAGE2_HOOK environment variable: 
> in it, put the path to a script, and that script will be run before s6-rc analyzes its service set. 
> That script should be the one to read your environment variables, and adjust the files of /etc/s6-overlay/s6-rc.d/user/contents accordingly.