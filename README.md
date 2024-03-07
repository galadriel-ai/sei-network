# sei-network

### Deploying the network

Pre-requirements:
* docker and docker-compose
* make and gcc

```shell
git clone git@github.com:galadriel-ai/sei-network.git
cd sei-network
git clone https://github.com/sei-protocol/sei-chain.git
cd sei-chain
git checkout v4.1.4-evm-devnet
# fix the Makefile:
# * Line 137: rm -rf -> sudo rm -rf
# * Line 178: xargs docker kill -> xargs -r docker kill 
# * Line 140: depending on what host you are running, on MacOs:
#   * switch --network host with: -p 8545:8545 -p 8546:8546 -p 26657:26657
make build-docker-node
make run-local-node
cd ..
```

### Setup nginx proxy

```shell
docker compose up --build -d
```