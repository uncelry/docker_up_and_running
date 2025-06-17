Большинство команд здесь надо выполнять в реальности через `docker -H <ip-узла> ...`, но практика была только на 
локальной машине, поэтому почти везде это опущено

`docker swarm init --advertise-addr 172.17.0.1` - добавить в кластер новый узел в качестве менджера 
(ip - в сети docker0 текущего узла)

`docker swarm join-token --quiet worker` - получить новый токен для узлов swarm

`docker -H 172.17.0.1 system info` - выполнить команду (получение данных) на узле swarm (только не локально - 
порт может быть недоступен)

`docker node ls` - получить список узлов в кластере

`docker swarm join --token <token>
172.17.0.1:2377` - добавить рабочие узлы (workers) в кластер swarm. Если передать токен менеджера, то подключим менеджер

`docker swarm join-token manager` - выполнить на любом активном узле, чтобы получить токен для добавления менеджера

`docker network create --driver=overlay default-net` - создать сеть для взаимодействия swarm (есть и по умолчанию - 
ingress)

`docker service create --detach=true --name quantum --replicas 2 --publish published=80,target=8080 
--network default-net spkane/quantum-game:latest` - создать сервис в режиме swarm

`docker service ps quantum` - посмотреть контейнеры сервиса

`docker service ls` - посмотреть все сервисы

Получить доступ к сервису можно по ip любого из услов

`docker service inspect --pretty quantum` - получить подробную информацию о сервисе

`docker service scale --detach=false quantum=4` - масштабирование сервиса (изменяем кол-во реплик)

`docker service update --update-delay 10s --update-failure-action rollback --update-monitor 5s --update-order 
start-first --update-parallelism 1 --detach=false --image spkane/quantum-game:latest-plus quantum` - развернуть новый
выпуск сервиса

`docker service rollback quantum` - откатиться на предыдущую версию сервиса (если 2 раза подряд, то будем просто две 
версии менять по кругу)

`docker service rm quantum` - остановить и удалить сервис

На основе команды `service` есть команда `stack`, которая может развернуть стэк приложений из файла docker-compose.yaml

`docker stack deploy --compose-file docker-compose-stack.yaml rocketchat` - запустить стэк на основе docker-compose.yaml

`docker stack ls` - список стеков

`docker stack ps -f "desired-state=running" rocketchat` - посмотреть контейнеры, управляемые стеком

`docker stack rm rocketchat` - снести стек

`docker service ps -f "desired-state=running" quantum` - посмотреть контейнеры в статусе RUNNING, управляемые сервисом

`docker node update --availability drain kelry-MS-7D88` - вывести узел из-под нагрузки (увести в DRAIN). Далее с ним 
можно проводить работы, например

`docker node inspect --pretty kelry-MS-7D88` - узнать подробности про узел. Контейнеры с этого узла автоматически 
перенесутся на другой (если он есть)

`docker node update --availability active kelry-MS-7D88` - вернуть узел под нагрузку (например, после проведения работ)

`docker network rm default-net` - удалить сеть
