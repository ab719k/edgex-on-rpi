# edgex on Raspberry Pi

Steps to install EdgeX on Raspberry Pi

## Build EdgeX
Instructions to build EdgeX are available at EdgeXFoundry Github page [link](https://github.com/edgexfoundry/edgex-go) <br>
Prerequisites for building EdgeX, require instllation of following tools 

### Install go
Latest version  [go1.17.7.linux-armv6l.tar.gz](https://go.dev/dl/go1.17.7.linux-armv6l.tar.gz) from the [downloads Page](https://go.dev/dl/). Refer to [go install information](https://go.dev/doc/install). The current targeted version of the Go language runtime for release artifacts is v1.17.x
The minimum supported version of the Go language runtime is v1.17.x

### ZeroMQ
Several EdgeX Foundry services depend on ZeroMQ for communications by default. The easiest way to get and install ZeroMQ on Linux is to use this setup script. 
  Another option to build from source
```  
sudo apt-get update && \
sudo apt-get install -y libtool pkg-config build-essential autoconf automake uuid-dev cmake
git clone https://github.com/zeromq/libzmq.git
cd libzmq/
git checkout v4.3.4
mkdir _build && cd _build/
cmake ../.
make
sudo make install
ldconfig -p | grep zmq
```
another shortcut to install zeromq is to run the apt-get install command

```
apt-get install libzmq3-dev

```

### Install Redis
```
wget http://download.redis.io/redis-stable.tar.gz
tar xvzf redis-stable.tar.gz
cd redis-stable
make
```
  
### Install Consul
```
apt-get install consul
```
  
### Install docker
```
curl -fsSL https://get.docker.com -o get-docker.sh
DRY_RUN=1 sh ./get-docker.sh
bash get_docker.sh
```

### install docker-compose

```
apt-get install -y libffi-dev libssl-dev python3-dev
apt-get install -y python3 python3-pip
pip3 install docker-compose

```
  
  
### Installation and Execution
EdgeX is organized as Go Modules; there is no requirement to set the GOPATH or GO111MODULE envrionment variables nor is there a requirement to root all the components under ~/go (or $GOPATH) and use the go get command. In other words,

```
git clone git@github.com:edgexfoundry/edgex-go.git
cd edgex-go
make build
```  
If you do want to root everthing under $GOPATH, you're free to use that pattern as well
```
GO111MODULE=on && export GO111MODULE
go get github.com/edgexfoundry/edgex-go
cd $GOPATH/src/github.com/edgexfoundry/edgex-go
make build
```

To build the docker images, we need some modifications and follow the instructions at the [link](https://github.com/noobtechie/edgex-go/blob/main/RD-DeployingEdgeXthroughDockeronBeagleboneBlack-280122-1304-35.pdf). The most important instructions for compiling is to change the file edgex-go/cmd/sys-mgmt-agent/Dockerfile line#30
-FROM docker:20.10.10
+FROM mazzolino/docker:20
```
cd edgex-go
make docker
```

After the build is successfull doing docker image ls show the following
```
docker image ls
REPOSITORY                                TAG                                        IMAGE ID       CREATED             SIZE
edgexfoundry/security-bootstrapper        0.0.0-dev                                  d710ebe4c102   8 minutes ago       15.5MB
edgexfoundry/security-bootstrapper        c1fb6d23c38d8e4135415aa828943646775f2bb3   d710ebe4c102   8 minutes ago       15.5MB
<none>                                    <none>                                     f6c167b8d3a2   8 minutes ago       1.17GB
edgexfoundry/security-secretstore-setup   0.0.0-dev                                  e98ce80aa206   9 minutes ago       23.7MB
edgexfoundry/security-secretstore-setup   c1fb6d23c38d8e4135415aa828943646775f2bb3   e98ce80aa206   9 minutes ago       23.7MB
<none>                                    <none>                                     500162741d3c   10 minutes ago      1.18GB
edgexfoundry/security-proxy-setup         0.0.0-dev                                  0673ecb7588e   10 minutes ago      22.2MB
edgexfoundry/security-proxy-setup         c1fb6d23c38d8e4135415aa828943646775f2bb3   0673ecb7588e   10 minutes ago      22.2MB
<none>                                    <none>                                     428956dccdf7   11 minutes ago      1.17GB
edgexfoundry/support-scheduler            0.0.0-dev                                  cbb0262afd60   12 minutes ago      13.1MB
edgexfoundry/support-scheduler            c1fb6d23c38d8e4135415aa828943646775f2bb3   cbb0262afd60   12 minutes ago      13.1MB
<none>                                    <none>                                     93576791a3c0   12 minutes ago      1.17GB
edgexfoundry/sys-mgmt-agent               0.0.0-dev                                  9ba3ae623077   13 minutes ago      268MB
edgexfoundry/sys-mgmt-agent               c1fb6d23c38d8e4135415aa828943646775f2bb3   9ba3ae623077   13 minutes ago      268MB
<none>                                    <none>                                     c29135b3fb3d   14 minutes ago      1.17GB
edgexfoundry/support-notifications        0.0.0-dev                                  c3029202ce6f   11 hours ago        13.7MB
edgexfoundry/support-notifications        c1fb6d23c38d8e4135415aa828943646775f2bb3   c3029202ce6f   11 hours ago        13.7MB
<none>                                    <none>                                     7084ff5e20a2   11 hours ago        1.17GB
edgexfoundry/core-command                 0.0.0-dev                                  d0f56c4ea970   11 hours ago        12.9MB
edgexfoundry/core-command                 c1fb6d23c38d8e4135415aa828943646775f2bb3   d0f56c4ea970   11 hours ago        12.9MB
<none>                                    <none>                                     6161aa5fbba1   11 hours ago        1.17GB
edgexfoundry/core-metadata                0.0.0-dev                                  4affba18b84d   11 hours ago        13.6MB
edgexfoundry/core-metadata                c1fb6d23c38d8e4135415aa828943646775f2bb3   4affba18b84d   11 hours ago        13.6MB
edgexfoundry/core-data                    0.0.0-dev                                  dd2e50b1eabf   11 hours ago        15.8MB
edgexfoundry/core-data                    c1fb6d23c38d8e4135415aa828943646775f2bb3   dd2e50b1eabf   11 hours ago        15.8MB
golang                                    1.17-alpine3.15                            25233bb43442   6 days ago          296MB
mazzolino/docker                          20                                         3c4003cbd4b0   6 weeks ago         193MB
alpine                                    3.14                                       30a002f9ce29   3 months ago        3.83MB
```

You can remove all the big 1+GB files to save space
```
docker image rm <id>
```

# Steps to configrue docker-compose.yml
Go to compose-builder folder 
```
cd edgex-compose/compose-builder
sudo make gen no-secty dev
```

This will generate a docker-compose.yml file in the current directory. The docker file generated is already in the github with the modifications. Remove the lines containing ":z" flags for the volume bindings. This is only for non debian Linux distros. Additionally if you want to expose your microservices to the network other than the host change the IP address of the microservices from "127.0.0.1" to "0.0.0.0". Use the sed command for this purpose.
```
sed -i -e 's/127.0.0.1/0.0.0.0/g' docker-compose.yml
```
# Add random-integer


# Add device virtual

# Add app-service-configurable
Add the add-asc-mqtt-export.yml and asc-mqtt-export.env for the MQTT forwarding to the broker.

# Running the EdgeX docker
Now run the docker-compose.yml file using the command 
```
root@raspberrypi:~/edgex-on-rpi# docker-compose -p edgex up -d
Creating edgex-ui-go       ... done
Creating edgex-redis       ... done
Creating edgex-core-consul ... done
Creating edgex-kuiper                ... done
Creating edgex-support-scheduler     ... done
Creating edgex-support-notifications ... done
Creating edgex-core-metadata         ... done
Creating edgex-core-command          ... done
Creating edgex-core-data             ... done
Creating edgex-app-rules-engine      ... done
Creating edgex-app-mqtt              ... done
Creating edgex-sys-mgmt-agent        ... done
Creating edgex-device-random         ... done
```

This might take a few minutes on the first run.

Start and stop docker edgex commands
```
sudo docker-compose -p edgex up -d
sudo docker-compose -p edgex down
```

List of docker images

```
root@raspberrypi:~/edgex-on-rpi/edgex-compose/compose-builder# docker image ls
REPOSITORY                                TAG                                        IMAGE ID       CREATED        SIZE
edgexfoundry/device-random                0.0.0-dev                                  a84405dd9421   2 hours ago    16.4MB
edgexfoundry/device-random                b4b7d92ebf9d87d8d84b804aa68b575b4f0789af   a84405dd9421   2 hours ago    16.4MB
edgexfoundry/device-virtual               0.0.0-dev                                  88911395a9ef   4 days ago     16.3MB
edgexfoundry/device-virtual               7f10fc8ede43ab0a1e02c0d4ad73e0798a225963   88911395a9ef   4 days ago     16.3MB
edgexfoundry/edgex-ui                     0.0.0-dev                                  dd517a0bd708   4 days ago     20.6MB
edgexfoundry/edgex-ui                     246039579fefe65770a4c1017b136de77c7f7cec   dd517a0bd708   4 days ago     20.6MB
edgexfoundry/app-service-configurable     0.0.0-dev                                  8a7968bba5e7   4 days ago     20.3MB
edgexfoundry/app-service-configurable     426cd0b3f460e98e9262508614d112ce01f12faa   8a7968bba5e7   4 days ago     20.3MB
edgexfoundry/security-bootstrapper        0.0.0-dev                                  0c79d90bfb0c   4 days ago     15.5MB
edgexfoundry/security-bootstrapper        c1fb6d23c38d8e4135415aa828943646775f2bb3   0c79d90bfb0c   4 days ago     15.5MB
edgexfoundry/security-secretstore-setup   0.0.0-dev                                  efe26a92495b   4 days ago     23.7MB
edgexfoundry/security-secretstore-setup   c1fb6d23c38d8e4135415aa828943646775f2bb3   efe26a92495b   4 days ago     23.7MB
edgexfoundry/security-proxy-setup         0.0.0-dev                                  579484c4fc79   4 days ago     22.2MB
edgexfoundry/security-proxy-setup         c1fb6d23c38d8e4135415aa828943646775f2bb3   579484c4fc79   4 days ago     22.2MB
edgexfoundry/support-scheduler            0.0.0-dev                                  1bf99c38c48a   4 days ago     13.1MB
edgexfoundry/support-scheduler            c1fb6d23c38d8e4135415aa828943646775f2bb3   1bf99c38c48a   4 days ago     13.1MB
edgexfoundry/sys-mgmt-agent               0.0.0-dev                                  b87a4dae5544   4 days ago     268MB
edgexfoundry/sys-mgmt-agent               c1fb6d23c38d8e4135415aa828943646775f2bb3   b87a4dae5544   4 days ago     268MB
edgexfoundry/support-notifications        0.0.0-dev                                  c78a7ec1aca0   4 days ago     13.7MB
edgexfoundry/support-notifications        c1fb6d23c38d8e4135415aa828943646775f2bb3   c78a7ec1aca0   4 days ago     13.7MB
edgexfoundry/core-command                 0.0.0-dev                                  00b51169c29a   4 days ago     12.9MB
edgexfoundry/core-command                 c1fb6d23c38d8e4135415aa828943646775f2bb3   00b51169c29a   4 days ago     12.9MB
edgexfoundry/core-metadata                0.0.0-dev                                  e0ed358fb43b   4 days ago     13.6MB
edgexfoundry/core-metadata                c1fb6d23c38d8e4135415aa828943646775f2bb3   e0ed358fb43b   4 days ago     13.6MB
edgexfoundry/core-data                    0.0.0-dev                                  0ee8441b417f   4 days ago     15.8MB
edgexfoundry/core-data                    c1fb6d23c38d8e4135415aa828943646775f2bb3   0ee8441b417f   4 days ago     15.8MB
golang                                    1.17-alpine3.15                            25233bb43442   8 days ago     296MB
consul                                    1.10                                       7146de82d58c   2 weeks ago    101MB
python                                    3.7-alpine                                 4f8f681365a0   3 weeks ago    39.2MB
ubuntu                                    latest                                     0e749921cacc   3 weeks ago    49.8MB
redis                                     latest                                     21d4c18eccbd   4 weeks ago    82.3MB
mazzolino/docker                          20                                         3c4003cbd4b0   6 weeks ago    193MB
lfedge/ekuiper                            1.4.0-slim                                 3ec0092e40fc   2 months ago   271MB
redis                                     6.2-alpine                                 73ab2bdcba74   2 months ago   24.4MB
redis                                     alpine                                     73ab2bdcba74   2 months ago   24.4MB
alpine                                    3.12                                       4b128ebb086e   3 months ago   3.79MB
alpine                                    3.14                                       30a002f9ce29   3 months ago   3.83MB
golang                                    1.16-alpine3.12                            4a431095cd91   8 months ago   280MB
```

#Running EdgeX

## Starting Edgex

```
root@raspberrypi:~/edgex-on-rpi# docker-compose -p edgex up -d
Creating edgex-ui-go       ... done
Creating edgex-redis       ... done
Creating edgex-core-consul ... done
Creating edgex-kuiper                ... done
Creating edgex-support-scheduler     ... done
Creating edgex-support-notifications ... done
Creating edgex-core-metadata         ... done
Creating edgex-core-command          ... done
Creating edgex-core-data             ... done
Creating edgex-app-rules-engine      ... done
Creating edgex-app-mqtt              ... done
Creating edgex-sys-mgmt-agent        ... done
Creating edgex-device-random         ... done
```

## Listing EdgeX container that are running
```
root@raspberrypi:~/edgex-on-rpi# docker ps
CONTAINER ID   IMAGE                                             COMMAND                  CREATED         STATUS         PORTS                                                                                         NAMES
06cfac3722ea   edgexfoundry/sys-mgmt-agent:0.0.0-dev             "/sys-mgmt-agent -cp…"   2 minutes ago   Up 2 minutes   0.0.0.0:58890->58890/tcp                                                                      edgex-sys-mgmt-agent
d9cc665c0ed1   edgexfoundry/device-random:0.0.0-dev              "/device-random --cp…"   2 minutes ago   Up 2 minutes   0.0.0.0:49988->49988/tcp, 0.0.0.0:59988->59988/tcp                                            edgex-device-random
33f7dce15954   edgexfoundry/app-service-configurable:0.0.0-dev   "/app-service-config…"   2 minutes ago   Up 2 minutes   48095/tcp, 0.0.0.0:59702->59702/tcp                                                           edgex-app-mqtt
77f08457d97c   edgexfoundry/app-service-configurable:0.0.0-dev   "/app-service-config…"   2 minutes ago   Up 2 minutes   48095/tcp, 0.0.0.0:59701->59701/tcp                                                           edgex-app-rules-engine
e58ad3cd9ba3   edgexfoundry/core-data:0.0.0-dev                  "/core-data -cp=cons…"   2 minutes ago   Up 2 minutes   0.0.0.0:5563->5563/tcp, 0.0.0.0:59880->59880/tcp                                              edgex-core-data
e7b636675082   edgexfoundry/core-command:0.0.0-dev               "/core-command -cp=c…"   2 minutes ago   Up 2 minutes   0.0.0.0:59882->59882/tcp                                                                      edgex-core-command
c5554b8ce2b6   edgexfoundry/core-metadata:0.0.0-dev              "/core-metadata -cp=…"   2 minutes ago   Up 2 minutes   0.0.0.0:59881->59881/tcp                                                                      edgex-core-metadata
e87c4089a8b4   edgexfoundry/support-scheduler:0.0.0-dev          "/support-scheduler …"   2 minutes ago   Up 2 minutes   0.0.0.0:59861->59861/tcp                                                                      edgex-support-scheduler
4b816e86eb6c   edgexfoundry/support-notifications:0.0.0-dev      "/support-notificati…"   2 minutes ago   Up 2 minutes   0.0.0.0:59860->59860/tcp                                                                      edgex-support-notifications
ce6153e120f0   lfedge/ekuiper:1.4.0-slim                         "/usr/bin/docker-ent…"   2 minutes ago   Up 2 minutes   9081/tcp, 20498/tcp, 0.0.0.0:59720->59720/tcp                                                 edgex-kuiper
5fe40fe3001a   consul:1.10                                       "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   8300-8302/tcp, 8301-8302/udp, 8600/tcp, 8600/udp, 0.0.0.0:8500->8500/tcp, :::8500->8500/tcp   edgex-core-consul
be46c1877c4e   redis:6.2-alpine                                  "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   0.0.0.0:6379->6379/tcp                                                                        edgex-redis
2aae848f3781   edgexfoundry/edgex-ui:0.0.0-dev                   "./edgex-ui-server -…"   2 minutes ago   Up 2 minutes   0.0.0.0:4000->4000/tcp, :::4000->4000/tcp                                                     edgex-ui-go
```

## Stoping EdgeX services
```
root@raspberrypi:~/edgex-on-rpi# docker-compose -p edgex down
Stopping edgex-sys-mgmt-agent        ... done
Stopping edgex-device-random         ... done
Stopping edgex-app-mqtt              ... done
Stopping edgex-app-rules-engine      ... done
Stopping edgex-core-data             ... done
Stopping edgex-core-command          ... done
Stopping edgex-core-metadata         ... done
Stopping edgex-support-scheduler     ... done
Stopping edgex-support-notifications ... done
Stopping edgex-kuiper                ... done
Stopping edgex-core-consul           ... done
Stopping edgex-redis                 ... done
Stopping edgex-ui-go                 ... done
Removing edgex-sys-mgmt-agent        ... done
Removing edgex-device-random         ... done
Removing edgex-app-mqtt              ... done
Removing edgex-app-rules-engine      ... done
Removing edgex-core-data             ... done
Removing edgex-core-command          ... done
Removing edgex-core-metadata         ... done
Removing edgex-support-scheduler     ... done
Removing edgex-support-notifications ... done
Removing edgex-kuiper                ... done
Removing edgex-core-consul           ... done
Removing edgex-redis                 ... done
Removing edgex-ui-go                 ... done
Removing network edgex_edgex-network
```
