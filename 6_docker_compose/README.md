`docker compose config` - проверка правильности конфигурации docker-compose

`docker compose build` - собрать все нужные сервисы для compose (сервисы без сборки пропускаются)

`docker compose up -d` - запустить сервисов в фоновом режиме

`docker compose logs` - получить логи всех сервисов

`docker compose logs rocketchat` - получить логи конкретного сервиса

`docker compose restart rocketchat` - перезапустить выбранный сервис

`docker compose top` - просмотр контейнеров и процессов в них

`docker compose exec mongodb bash` - выполнить команду/войти в определенный сервис (не надо передавать -it)

`docker compose start zmachine` - запустить сервис \
`docker compose stop zmachine` - остановить сервис \
`docker compose pause zmachine` - приостановить сервис \
`docker compose unpause zmachine` - возобновить сервис \

`docker compose down` - остановить все сервисы

`docker compose -f overrides/docker-compose-override.yaml -d up` - запустить сервисы из конкретного файла

В docker-compose.yaml можно использовать **значения по-умолчанию**: \
`MONGODB_INITIAL_PRIMARY_PORT_NUMBER: ${MONGODB_INITIAL_PRIMARY_PORT_NUMBER:-27017}` \
(если выставлена env переменная, то использовать ее значение, иначе - 27017. Пустая env
переменная обрабатывается как пустая строка)

Так же в docker-compose.yaml можно использовать **обязательные значения**: \
`ROCKETCHAT_PASSWORD: ${HUBOT_ROCKETCHAT_PASSWORD:?HUBOT_ROCKETCHAT_PASSWORD must be set!}` \
(если выставлена env переменная, то использовать ее значение, иначе - выдать ошибку конфигурации указанную после '?')

Можно использовать файл .env в той же директории, где и файл docker-compose.yaml

Приоритет проставления env переменных в docker-compose следующий:
1. Значение по умолчанию в docker-compose.yaml
2. .env файл
3. Переменные среды

(Сначала читаются значения по-умолчанию, потом перезаписываются из .env файла, потом перезаписываются переменными среды)

То есть, значения по умолчанию должны быть самыми общими; значения в .env - каждыми для отдельного пользователя у себя
в локальной системе; а уже переменные среды - для частных очень специфичных случаев
