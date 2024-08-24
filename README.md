# Docker Controller Bot

[![](https://badgen.net/badge/icon/github?icon=github&label)](https://github.com/dgongut/docker-controller-bot)
[![](https://badgen.net/badge/icon/docker?icon=docker&label)](https://hub.docker.com/r/dgongut/docker-controller-bot)
[![](https://badgen.net/badge/icon/telegram?icon=telegram&label)](https://t.me/dockercontrollerbotnews)
[![Docker Pulls](https://badgen.net/docker/pulls/dgongut/docker-controller-bot?icon=docker&label=pulls)](https://hub.docker.com/r/dgongut/docker-controller-bot/)
[![Docker Stars](https://badgen.net/docker/stars/dgongut/docker-controller-bot?icon=docker&label=stars)](https://hub.docker.com/r/dgongut/docker-controller-bot/)
[![Docker Image Size](https://badgen.net/docker/size/dgongut/docker-controller-bot?icon=docker&label=image%20size)](https://hub.docker.com/r/dgongut/docker-controller-bot/)
![Github stars](https://badgen.net/github/stars/dgongut/docker-controller-bot?icon=github&label=stars)
![Github forks](https://badgen.net/github/forks/dgongut/docker-controller-bot?icon=github&label=forks)
![Github last-commit](https://img.shields.io/github/last-commit/dgongut/docker-controller-bot)
![Github last-commit](https://badgen.net/github/license/dgongut/docker-controller-bot)
![alt text](https://github.com/dgongut/pictures/blob/main/Docker-Controller-Bot/mockup.png)

Manage your Docker containers from a single place.

- ✅ List containers
- ✅ Start, stop, and remove containers
- ✅ Obtain logs both directly and through files
- ✅ Extract the docker-compose file for your containers
- ✅ Notifications when a container stops or starts
- ✅ Notifications when a container has a pending update
- ✅ Update containers
- ✅ Change the tag (rollback or update)
- ✅ Clean the system by removing unused containers, images, and other objects
- ✅ Language support (Spanish, English, Dutch, German)

Looking for it on [![](https://badgen.net/badge/icon/docker?icon=docker&label)](https://hub.docker.com/r/dgongut/docker-controller-bot)?

**NEW** News channel on [![](https://badgen.net/badge/icon/telegram?icon=telegram&label)](https://t.me/dockercontrollerbotnews)

🖼️ If you want to set an icon for the Telegram bot, you can download a high-resolution icon [here](https://raw.githubusercontent.com/dgongut/pictures/main/Docker-Controller-Bot/Docker-Controller-Bot.png). Just download it and send it to @BotFather using the BotPic option.

## Configuration in config.py

| KEY  | REQUIRED | VALUE |
|:------------- |:---------------:| :-------------|
|TELEGRAM_TOKEN |✅| Bot token |
|TELEGRAM_ADMIN |✅| Admin ChatId (you can obtain it by messaging the bot Rose and typing /id). Supports multiple admins separated by commas. For example 12345,54431,55944 |
|TELEGRAM_GROUP |❌| Group ChatId. If this bot is to be part of a group, you need to specify the ChatId of that group. The bot must be an admin of the group |
|TELEGRAM_THREAD |❌| Thread number within a supergroup; numeric value (2,3,4..). Default is 1. Used in conjunction with the TELEGRAM_GROUP variable |
|TELEGRAM_NOTIFICATION_CHANNEL |❌| Channel where container status changes will be exclusively published |
|CONTAINER_NAME |✅| Container name, what is set in container_name in the docker-compose file should also go here |
|TZ |✅| Timezone (e.g., Europe/Madrid) |
|CHECK_UPDATES |❌| If you want it to check for updates. 0 no - 1 yes. Default is 1 |
|CHECK_UPDATE_EVERY_HOURS |❌| Wait time in hours between update checks (default 4 hours) | 
|BUTTON_COLUMNS |❌| Number of button columns in the container lists (default 2 columns) | 
|LANGUAGE |❌| Language, can be ES / EN / NL / DE. Default is ES (Spanish) | 
|EXTENDED_MESSAGES |❌| If you want it to show more detailed information messages. 0 no - 1 yes. Default is 0 | 

### Notes
A volume mapping is required to store what the bot writes in /app/schedule

### Example Docker-Compose for Normal Execution

```yaml
version: '3.3'
services:
    docker-controller-bot:
        environment:
            - TELEGRAM_TOKEN=
            - TELEGRAM_ADMIN=
            - CONTAINER_NAME=docker-controller-bot
            - TZ=Europe/Madrid
            #- TELEGRAM_GROUP=
            #- TELEGRAM_THREAD=1
            #- TELEGRAM_NOTIFICATION_CHANNEL=
            #- CHECK_UPDATES=1
            #- CHECK_UPDATE_EVERY_HOURS=4
            #- BUTTON_COLUMNS=2
            #- LANGUAGE=ES
            #- EXTENDED_MESSAGES=0
        volumes:
            - /var/run/docker.sock:/var/run/docker.sock # DO NOT CHANGE
            - /path/to/save/schedules:/app/schedule # CHANGE LEFT SIDE
        image: dgongut/docker-controller-bot:latest
        container_name: docker-controller-bot
        restart: always
        network_mode: host
        tty: true
```

### Extra Functions via Labels on Other Containers

- By adding the label `DCB-Ignore-Check-Updates` to a container, updates for it will not be checked.
- By adding the label `DCB-Auto-Update` to a container, it will automatically update without asking.

### Acknowledgments

Dutch translation: [ManCaveMedia](https://github.com/ManCaveMedia)  
German translation: [shedowe19](https://github.com/shedowe19)

---

## For Developers Only - Running with Local Code

To run it locally and test new code changes, rename the `.env-example` file to `.env` with the necessary values for execution.
You must set a correct `TELEGRAM_TOKEN` and `TELEGRAM_ADMIN`, different from the normal execution ones.

The folder structure should be:

```
docker-controller-bot/
    ├── .env
    ├── LICENSE
    ├── README.md
    ├── config.py
    ├── docker-controller-bot.py
    ├── Dockerfile_local
    ├── docker-compose.yaml
    └── locale
        ├── en.json
        ├── es.json
        └── nl.json
```

To start it, run `docker compose up -d` in that directory.

To stop it and test new changes, run `docker compose down --rmi`.