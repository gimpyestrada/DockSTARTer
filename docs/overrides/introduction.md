# Overrides Intro

The YML files included with and generated by DockSTARTer are NOT meant to be modified.

- Updating DockSTARTer will overwrite the YML files in `~/.docker/compose/.apps/`.
- The `~/.docker/compose/docker-compose.yml` file is generated and rewritten by DockSTARTer when you use the Configuration menu or run `ds -c`.

If you would like to make some adjustments, the best way is to use a `docker-compose.override.yml` file.

Docker Compose will look for `~/.docker/compose/docker-compose.override.yml`. Anything you set in this file will be merged in and take priority over the regular configurations.

## Types of Overrides

You can use overrides to:

- Modify an existing app (such as changing which image the app uses)
- Add an entirely new app that's not included in DockSTARTer

### Modify an Existing App

The example below will change Sonarr to use hotio's image for Sonarr and add a /media volume. Everything else from the original config, such as the remaining volumes and environment variables, will be preserved and merged with the override config.

#### Partial Override Merge Example

```yaml
services:
  sonarr:
    image: cr.hotio.dev/hotio/sonarr
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - ${DOCKERCONFDIR}/sonarr:/config
      - ${DOCKERSTORAGEDIR}:/storage
```

### Add a New App

The example below will use the override file to add an app not included in DockSTARTer. With this approach, you must add all of the Docker Compose YAML configuration required for the app.

#### Full App Override Example

```yaml
services:
  alltube:
    container_name: alltube
    environment:
      - PGID=1000
      - PUID=1000
    image: rudloff/alltube
    logging:
      driver: json-file
      options:
        max-file: ${DOCKERLOGGING_MAXFILE}
        max-size: ${DOCKERLOGGING_MAXSIZE}
    ports:
      - "1234:80"
    restart: unless-stopped
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - ${DOCKERCONFDIR}/alltube:/var/www/html/config
      - ${DOCKERSTORAGEDIR}:/storage
```

**Make sure to run `ds -c` or `ds -c up <appname>` after you make changes to your override file.**
