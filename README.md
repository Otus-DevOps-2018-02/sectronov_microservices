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

## Homework 14: Docker контейнеры. Docker под капотом

Команда для создания инстанса с `docker` на `GCE`:
```
docker-machine create --driver google \
 --google-machine-image https://www.googleapis.com/compute/v1/projects/ubuntu-os-cloud/global/images/family/ubuntu-1604-lts \
 --google-machine-type n1-standard-1 \
 --google-zone europe-west1-b \
 docker-host 
 ```

Команды:

- `docker-machine ls` - список инстансов, созданых через `docker-machine`
- `eval $(docker-machine env docker-host)` — для запусака команд `docker` на инстансе
- `docker build -t reddit:latest .` — билд `docker`-образа с названием `reddit` и тегом `latest`. `.` указыает что надо искать `Dockerfile` в текущей директории
- `docker images -a` — список образов включая промежуточные
- `docker run --name reddit -d --network=host reddit:latest` — запуск контейнера
- `docker tag reddit:latest sectronov/otus-reddit:1.0` — протегировать образ
- `docker push sectronov/otus-reddit:1.0` — запушить образ на `Docker Hub`
- `docker logs reddit -f` — вывод логов контейнера с названием `reddit`
- `docker exec -it reddit bash` — выполнить команду `bash` в контейнере `reddit`
- `docker start reddit` — звпуск контейнера `reddit`
- `docker stop reddit && docker rm reddit` — остановка и удаление контейнера
- `docker inspect sectronov/otus-reddit:1.0 -f '{{.ContainerConfig.Cmd}}'` — вывести запускаемую в контейнере команду

Разница между командами:

- `docker run --rm -ti tehbilly/htop` — команда работает в своём собственном неймспесе, поэтому `htop` имеет `pid` равный `1`
- `docker run --rm --pid host -ti tehbilly/htop` — работает в нейспесей хоста, поэтому `htop` имеет хостовой `pid`

Для задания со `*` в `docker-monolith/infra` добавлены:
- `terraform` — конфиги для создания инстансы в `GCE`
- `ansible` — плейбуки для установки `docker` и деплоя образа с приложением внутри. Хосты конфигурируются динамически через `gce.py`
- `packer` — билд образа `ubuntu` с `docker` внутри, в качестве `provision` используется плейбук `ansible/playbooks/install-docker.yml`
