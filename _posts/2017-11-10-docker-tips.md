# Docker tips

## install for Mac with brew
```bash
# prerequisite
brew update
brew install docker
brew install docker-machine
brew cask install virtualbox

# optional
brew install docker-compose

# start
docker-machine create --driver virtualbox default
docker-machine create --engine-registry-mirror=https://luddtcwl.mirror.aliyuncs.com -d virtualbox default # create a docker machine with mirror
docker-machine ls # check the engine
docker-machine start default
docker-machine ls # check again
docker run -d hello-world
docker-machine env default # check the env command
eval $(docker-machine env default)
docker run hello-world # start a test docker


# stop
docker-machine stop default
```

## docker-machine commands
```bash
docker-machine active         # display the active docker machine.
docker-machine env $name      # display environment for accessing the docker machine.
eval $(docker-machine env $name)           # set the environment for the docker machine.
eval $(docker-machine env -u)              # clean the environment.
docker-machine ip $name       # get ip for the docker machine.
curl $(docker-machine ip $name):80         # curl a web page provided by the docker.
docker-machine ssh $name      # ssh to the docker machine.
docker-machine scp ~/localfiletxt $name:~/ # scp a local file to docker machine, or vice versa.
```

## curl Host in Container
There is a DNS name with "docker.for.mac.localhost" in Mac and "docker.for.win.localhost" in Windows. For instance:
```bash
curl http://docker.for.mac.localhost:8000
```
