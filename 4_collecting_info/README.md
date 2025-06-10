`docker version` - версия Docker (клиента и сервера) \
`docker system info` - подробные данные о сервере Docker (и частично клиенте)

`docker image pull ubuntu:latest` - обновить образ \
*даже если в Dockerfile указать тег latest, автоматически обновляться образ не будет - надо руками*

`docker pull ubuntu@sha256:b59d21599a2b151e23eea5f6602f4af4d7d31c4e236d22bf0b62b86d2e386b8f` - обновить образ до версии

`docker container inspect 976dca76c354` - получить подробные данные по контейнеру

`docker container run --rm ubuntu:22.04 cat /etc/passwd` \
`docker container run --rm ubuntu:22.04 /bin/false` - если просто запустить Docker контейнер с конечной командой, \
ее результат выполнения будет доступен через `echo $?`

`docker container run --rm ubuntu:22.04 cat /etc/passwd | wc -l` - в случае конвейера, `wc -l` здесь выполнится локально\
`docker container run --rm ubuntu:22.04 bash -c "cat /etc/passwd | wc -l"` - вот здесь `wc -l` выполнится в контейнере

`docker container exec -it e30cea2b /bin/bash` - выполнить команду в запущенном контейнере \
`docker container exec -it -d e30cea2b sleep 100` - выполнить команду в запущенном контейнере в фоновом режиме

`docker volume ls` - получить список томов монтирования (пере используемых между запусками контейнеров) \
`docker container run --rm -d -v /tmp:/tmp ubuntu:22.04 sleep 120` - не создаст том монтирования, о котором речь выше

`docker volume create my-data` - создать том \
`docker volume inspect my-data` - получить данные по тому (в том числе куда он примонтирован)

`docker container run --rm -d --mount source=my-data,target=/app ubuntu:22.04 touch /app/some-data` - примонтировать том
`docker container run --rm --mount source=my-data,target=/app fedora:latest ls -lFa /app/some-data` - том выше тут будет

`docker volume rm my-data` - удалить том

`docker container logs nginx-test` - получить логи по контейнеру (если он поддерживает эту команду) \
`docker container logs --since 2025-06-08T16:04:08.704445866Z --tail 5 nginx-test` - логи с определенного момента, 5 шт\
Логи будут храниться /var/lib/docker/containers/<container_id>/<container_id>-json.log\
`docker container logs -f nginx-test` - смотреть логи в риал-тайме\
**Подумать про ротацию журналов логов. Как лучше собирать локально логи с множества контейнеров**\
`-log-opt max-size`, `--log-opt max-file` в `daemon.json`

`docker container stats tst` - смотреть статистику контейнера в риал-тайме \
`docker container stats --no-stream tst` - посмотреть статистику контейнера \
`curl --no-buffer -XGET --unix-socket /var/run/docker.sock http://docker/containers/tst/stats` - статистика контейнера 
в риал-тайме через API endpoint /stats \
`curl --no-buffer -XGET --unix-socket /var/run/docker.sock http://docker/containers/tst/stats | head -n 1 | jq` - бьюти

`docker container run --rm -d --name mongo-hc mongo-with-check:4.4` - запуск контейнера (если есть healthcheck в 
Dockerfile, то есть опции) \
`--health-start-period 60` - подождать 60 секунд до начала проверок healthcheck \
`--health-interval 10` - частота проверок \
`--health-retries 3` - сколько раз до признания неработоспособным \
`--no-healthcheck` - отключить проверку

`docker system events` - получать события сервера Docker в риал-тайме\
`curl --no-buffer -XGET --unix-socket /var/run/docker.sock http://docker/events` - то же самое через API\
Тут важно отслеживать события `container oom` (OOM) и 
`container exec_create`, `container exec_start`, `container exec_die` (вход в контейнер через exec)

Для настройки Prometheus в простом варианте - в daemon.json добавить:
```
{
    "experimental": true,
    "metrics-addr": "0.0.0.0:9323"
}
```
И перезапустить демон (сервер) Docker: `sudo systemctl restart docker`

Метрики сервера Docker должны быть доступны по адресу: `curl -s http://localhost:9323/metrics | head -15`

Создаем файл prometheus.yaml с настройками:
```
# Сбор метрик каждые 5 секунд, монитор будет называться stats-monitor
global:
    scrape_interval: 5s
    external_labels:
        monitor: 'stats-monitor'

# Задание будет называться DockerStats, мы подключимся к docker0 (через ip address)
scrape_configs:
  - job_name: 'DockerStats'
    static_configs:
      - targets: ['172.17.0.1:9323']
```

И запускаем контейнер с prometheus:\
`docker container run --rm -d -p 9090:9090 -v /tmp/prometheus/prometheus.yaml:/etc/prometheus.yaml prom/prometheus \
--config.file=/etc/prometheus.yaml`

**Надо закрывать выдачу метрик (порт 9090 в данном случае) от внешнего доступа**

Доступ получаем по порту 9090 (для локалки - `localhost:9090`)

`docker container cp 29e3083de0d5:/var/log/alternatives.log 
/etc/docker_up_and_running/4_collecting_info/logalternatives.log` - скопировать файлы из контейнера на хост (или 
наоборот)

`docker image save ubuntu:latest -o /etc/docker_up_and_running/4_collecting_info/image_save.tar` - сохранить образ в 
TAR

`docker image import /etc/docker_up_and_running/4_collecting_info/image_save.tar my_ubuntu:latest` - загрузка образа
из TAR файла
