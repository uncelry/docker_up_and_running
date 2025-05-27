### Создание базового Dockerfile для приложения на FastAPI с использованием менеджера зависимостей uv

- Сборка образа - ```docker image build -t <repository>/<image_name>:<tag> .```
- Запуск контейнера - ```docker container run --rm -d -p 80:80 <repository>/<image_name>:<tag>```
- Посмотреть запущенные контейнеры - ```docker ps```
- Посмотреть локальные образы - ```docker image list```
- Выгрузить локальный образ в удаленный репозиторий - ```docker push <repository>/<image_name>:<tag>````
