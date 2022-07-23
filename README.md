# log-to-telegram Docker
<a href="https://hub.docker.com/r/dickverbunt/log-to-telegram" target="_blank" title="log-to-telegram docker hub">
  <img src="https://img.shields.io/docker/pulls/dickverbunt/log-to-telegram" alt="Docker Pulls"/>
</a>

A docker container for reading growing log files and sending notifications via the telegram API based on items in a trigger list.
Inspired by [JVCubed/log-to-telegram](https://github.com/JVCubed/log-to-telegram/). I mostly use it to send Pi-Hole events to Telegram.

## Pull the image 
        $ docker pull ghcr.io/dlts-fd/log-to-telegram:latest

## Setup Docker run
- Update the `config.yml`
    - Add TelegramBot api and chatID to `config.yml`
    - Add triggers to the list to `config.yml`
- Run


        $ docker run -v {location of config.yml}:/app/config.yml -v {location of log file}:/app/file.log ghcr.io/dlts-fd/log-to-telegram:latest

- Ready! When a line gets added to the log file and matches a trigger it will send this to Telegram.

## Docker compose
- Update the `config.yml`
    - Add TelegramBot api and chatID to `config.yml`
    - Add triggers to the list to `config.yml`
- Add to docker-compose.yml:
```
log-to-telegram:
    image: ghcr.io/dlts-fd/log-to-telegram:latest
    container_name: log-to-telegram_myApp
    volumes:
      - /{path-to}/config.yml:/app/config.yml
      - /{path-to-log-file}:/app/file.log
    restart: unless-stopped
```
- Run

        $ docker-compose up -d

- Ready! When a line gets added to the log file and matches a trigger it will send this to Telegram.

## Notes

- In `config.yml` it's possible to edit the timeout period.
- Logs will be printed to the terminal. There's also a `/app/debug.log` file.
- Some applications perform logfile rotation. In that case log-to-telegram will stop reporting new lines as the mounted file will no longer update. If your application does lofgile rotation, setup a cronjob to restart the container to recognize the changed files. 
