Для работы с kubernetes в данном примере используется minikube + kubectl

`minikube start` - запуск minikube

`minikube ssh` - зайти через ssh на контейнер/виртуальную машину minikube

`minikube status` - проверить состояние кластера

`minikube ip` - получить IP виртуальной машины minikube

`minikube update-check` - проверить версию minikube

`minikube stop` - остановить minikube

`minikube delete` - очистить среду (удалить кластер minikube)

`minikube dashboard` - запустить дашборд minikube (в браузере)

`kubectl get services` - получить список сервисов kubernetes

`kubectl get nodes` - получить список узлов kubernetes

`kubectl create deployment hello-minikube --image=kennethreitz/httpbin:latest --port=80` - создать развертывание

`kubectl get all` - получить информацию об основных объектах кластера

`kubectl expose deployment hello-minikube --type=NodePort` - открыть пор в развертывании

`minikube service hello-minikube --url` - получить IP для доступа к сервису

`curl -H foo:bar $(minikube service hello-minikube --url)/get` - можно встроить команды minikube в другие запросы

`kubectl delete service hello-minikube` - удалить сервис kubernetes

`kubectl delete deployment hello-minikube` - удалить развертывание kubernetes

`kubectl api-resources` - список всех возможных ресурсов kubernetes

`kubectl apply -f ./lazyraster-service.yaml` - развернуть конфигурацию из файла .yaml

`kubectl get pvc` - получить список PersistentVolumeClaim 

`curl http://<ip:port>/documents/docker-up-and-running-public/sample.pdf?page=1 --output ./output` - воспользоваться 
сервисом

`kubectl scale --replicas=2 deploy/lazyraster` - изменить масштаб развертывания

`kubectl get deployment/lazyraster` - получить сведения по конкретному объекту

`kubectl logs deployment/lazyraster` - получить логи объекта

`kubectl logs pod/lazyraster-ff47694d-tr67l` - получить логи конкретного пода (например)

`kubectl proxy` - запустить веб-API kubectl в локальной системе

`kubectl delete -f ./lazyraster-service.yaml` - удалить все имеющиеся артефакты конфигурации из файла
