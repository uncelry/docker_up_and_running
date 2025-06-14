`docker container top nginx-debug` - получить список процессов внутри контейнера \
`sudo strace -p 12624` - наблюдать за процессом (в том числе внутри контейнера)

Вообще процессы в контейнерах выполняются в отдельном namespace, но мы можем получить к ним доступ с хоста 
(из той же системы)

`sudo lsof -p 12530` - информация о файлах, используемых процессами (в том числе внутри контейнера)

Можно запустить новй отладочный контейнер, который будет иметь доступ к процессам: \
`docker container run --rm -ti --cap-add=SYS_PTRACE --pid=container:<container_to_debug_name> ubuntu:latest bash`

`docker container run --rm -it --init ubuntu:latest bash` - запустить контейнер с tini процессом PID 1 (это 
полезно в случае, если в контейнере будут порождаться, форкаться и управляться несколько процессов. tini - это
мастер-процесс, который будет следить за тем, чтобы дочерние/форкнутые процессы завершались корректно, а не
становились зомби). В контейнере получим: `ps -ef`: `root   1    0  1 16:25 pts/0   00:00:00 /sbin/docker-init -- bash`

`docker network ls` - список имеющихся сетей Docker \
`docker network inspect bridge` - получить данные по сети Docker \
У контейнеров с сетью bridge сеть настроена через docker-proxy. У сети host - прокси нет. none - нет сети вообще

`docker image history ubuntu:latest --no-trunc` - получить историю сборки образа (по слоям). Полезно для оценки размеров 
и слоев сборки

`sudo ls /var/lib/docker/containers` - здесь храняться контейнеры по их хэшам. Внутри директории лежат примонтированные
файлы конфигурации контейнера, логи и т.п.

`docker container diff nginx-debug` - посмотреть изменения в файловой системе контейнера (A - add; C - change)
