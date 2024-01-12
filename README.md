# WebUI-Aria2

![Main interface](/screenshots/overview.png?raw=true)

Цель этого проекта — создать лучший и самый популярный в мире интерфейс для взаимодействия с aria2. aria2 — лучший в мире загрузчик файлов, но иногда командная строка дает больше возможностей, чем необходимо. Первоначально проект создавался как часть схемы GSOC, однако он быстро разросся и изменился благодаря огромной поддержке и отзывам сообщества aria2.

Очень прост в использовании, никаких скриптов сборки и скриптов установки. Сначала запустите aria2 в фоновом режиме либо на локальном, либо на удаленном компьютере. Вы можете сделать это следующим образом:

```bash
aria2c --enable-rpc --rpc-listen-all
```

Если aria2 не установлена ​​на вашем локальном компьютере, перейдите к https://aria2.github.io/ и следуйте инструкциям там.

Затем, чтобы использовать WebUI-Aria2,

- Вы можете скачать этот репозиторий и открыть index.html из `docs` папки.
- Или вы могли бы просто отправиться https://ziahamza.github.io/webui-aria2 и начните скачивать файлы! После того, как вы посетили URL-адрес благодаря [Прогрессивные веб-приложения](https://developers.google.com/web/progressive-web-apps/) вы можете открыть тот же URL-адрес, даже если вы не в сети.
- Или вы также можете использовать NodeJS для создания простого сервера, используя следующую команду из папки проекта.

```bash
node node-server.js
```

# Советы

1. Вы всегда можете выбрать, какие файлы скачивать в случае торрентов или металинков. Просто приостановите загрузку, и рядом с кнопкой настроек должен появиться значок списка. Чтобы выбрать файлы для загрузки перед началом загрузки, установите флажок --pause-metadata в aria2. Смотрите [link](https://aria2.github.io/manual/en/html/aria2c.html#cmdoption--pause-metadata)

# Конфигурация

Читать и редактировать [configuration.js](src/js/services/configuration.js).

## DirectURL

Эта функция позволяет пользователям загружать файлы, которые они загружают с aria2, прямо с панели управления WebUI. Если вы знакомы с тем, как работают веб-серверы, настройте http-сервер, который указывает на настроенный каталог загрузки aria2, проверьте разрешения. Затем укажите полный URL-адрес: `http://server:port/` в конфигурации webui DirectURL.

Если вышеизложенное не очевидно, продолжайте читать, о чем идет речь в разделе [directurl.md](directurl.md)

# Зависимости

Ну, вам нужна aria2. И веб-браузер (если это вообще имеет значение!)

# Docker support (Поддержка Докера)

В этом проекте есть два файла Dockerfile, один из них — обычный файл Dockerfile, который можно использовать для **testing purpose**.<br>
The second is a **production ready** Dockerfile for arm32v7 platforms (including Raspberry Pi).

### For testing purpose

You can also try or use webui-aria2 in your LAN inside a Docker sandbox.

Build the image

```bash
sudo docker build -t yourname/webui-aria2 .
```

..and run it! It will be available at: `http://localhost:9100`

```bash
sudo docker run -v /Downloads:/data -p 6800:6800 -p 9100:8080 --name="webui-aria2" yourname/webui-aria2
```

`/Downloads` is the directory in the host where you want to keep the downloaded files

### Production ready (ARM platform)

This image contains both aria2 and webui-aria2.

Build it (may take several hours due to the aria2 compilation process. Don't panic and grab a coffee).

```
docker build -f Dockerfile.arm32v7 -t yourname/webui-aria2 .
```

This command will ends up building three images:

- The first one is just about compiling aria2 and goreman binaries. It MUST be deleted each time the `ARIA2_VERSION` is changed in the Dockerfile, otherwise you won't benefit from the update.
- The second is about building and downloading some go dependencies (goreman and gosu).
- The second one is the acutal aria2 container, the one you must use.

<br />
Prepare the host volume:
This image required few file to be mounted in the container.
```
/home/aria/aria2/session.txt  (empty file)
/home/aria/aria2/aria2.log    (empty file)
/home/aria/aria2/aria2.conf   (aria2 configuration file, not webui-aria2 conf) must contains at least `enable-rpc=true` and `rpc-listen-all=true`
/data/downloads/        (where the downloaded files goes)
```

Run it

```
docker run --restart=always \
        -v /home/<USER>/data/aria2/downloads:/data/downloads \
        -v /home/<USER>/data/aria2/.aria2:/home/aria/.aria2 \
        -p 6800:6800 -p 9100:8080 \
        --name="webui-aria2" \
        -d yourname/webui-aria2
```

# Contributing

Checkout [contributor's guide](CONTRIBUTING.md) to know more about how to contribute to this project.

# Deploy to Heroku

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

# Support

For any support, feature request and bug report add an issue in the github project. [link](https://github.com/ziahamza/webui-aria2/issues)

# License

Refer to the LICENSE file (MIT License). If the more liberal license is needed then add it as an issue
