docker container create --name="awesome-service" -l deployer=Ahmed -p 80:80 3_container_commands:latest \
docker container inspect awesome-service \
docker container ls -a -f label=deployer=Ahmed \
docker container start awesome-service  \
docker container rm -f awesome-service

docker container run --hostname="mycontainer.example.com" 3_container_commands:latest \
docker container run --rm -ti --mount type=bind,target=/mnt/session_data,source=/data 3_container_commands:latest

docker system info

docker container run --rm -p 80:80 --cpus=.25 --memory=512m --memory-swap=1024m --device-write-iops /dev/vda:256 3_container_commands:latest

ulimit -a

docker container run -p 80:80 --ulimit nofile=150:300 3_container_commands:latest

docker container start 092c5df85044

docker container run -p 80:80 --restart=on-failure:3 3_container_commands:latest \
docker container run -p 80:80 --restart=unless-stopped 3_container_commands:latest

docker container stop 9e52aae0ed2e #SIGTERM \
docker container stop -t 60 9e52aae0ed2e

docker container kill 06253df948b6 #SIGKILL or any other \
docker container kill --signal=USR1 06253df948b6

docker container pause cbae67b8288e \
docker container unpause cbae67b8288e

docker container ls -a \
docker image ls -a

docker container rm cbae67b8288e \
docker container rm -f cbae67b8288e \
docker image rm \
docker image rm -f 3_container_commands:latest

docker system prune \
docker system prune -a

docker container rm \$(docker container ls -a -q) # Delete all containers \
docker image rm \$(docker image ls -a -q) # Delete all images \
docker container rm \$(docker container ls -a -q --filter 'exited!=0') # Delete all containers with not 0 exit code
