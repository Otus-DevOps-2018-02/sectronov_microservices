# sectronov_microservices
sectronov microservices repository

## Homework 13: Технология контейнеризации. Введение в Docker.

Команды для работы с докером:
- `docker version` – версии docker client и server 
- `docker info` – информация о текущем состоянии docker daemon
- `docker ps` - список запущенных контейнеров
- `docker ps -a` - список всех контейнеров
- `docker images` - список сохранненных образов
- `docker create` - создание контейнера
- `docker start` -  запускает остановленный контейнер
- `docker attach` - подсоединяет терминал к созданному контейнеру
- `docker run image-name` — создание и запуск контейнера на основе образа `image-name` с тегом `latest` (`docker run = docker create + docker start + docker attach`)
  - `-i` – запускает контейнер в foreground режиме (docker attach)
  - `-d` – запускает контейнер в background режиме
  - `-t` создает TTY
- `docker exec` - запускает новый процесс внутри контейнера
- `docker commit` - создает image из контейнера
- `docker kill` - посылает `SIGKILL`
- `docker stop` - посылает `SIGTERM`, и через n секунд посылает `SIGKILL`
- `docker system df` - отображает сколько дискового пространства
занято образами, контейнерами и volume’ами
- `docker rm` - удаляет контейнер
  - `-f` удаление работающего контейнера (будет отправлен `SIGKILL`)
- `docker rmi` -  удаляет image, если от него не зависят запущенные контейнеры
