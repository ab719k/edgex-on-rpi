# edgex on Raspberry Pi

Steps to install EdgeX on Raspberry Pi

## Build EdgeX
Instructions to build EdgeX are available at EdgeXFoundry Github page [link](https://github.com/edgexfoundry/edgex-go) <br>
Prerequisites for building EdgeX, require instllation of following tools 

### Install go
Download latest file [go1.17.7.linux-armv6l.tar.gz](https://go.dev/dl/go1.17.7.linux-armv6l.tar.gz)]. Download the latest  [go<version>.linux-armv6l.tar.gz](https://go.dev/dl/) at the Go downloads page. Refer to [go install information](https://go.dev/doc/install). The current targeted version of the Go language runtime for release artifacts is v1.17.x
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
  
