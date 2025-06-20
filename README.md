# sei-network

### Deploying the network

Pre-requirements:
* docker and docker-compose
* make and gcc

```shell
git clone git@github.com:galadriel-ai/sei-chain.git
cd sei-chain
# fix the Makefile:
# * Line 137: rm -rf -> sudo rm -rf
# * Line 178: xargs docker kill -> xargs -r docker kill 
# * Line 140: depending on what host you are running, on MacOs:
#   * switch --network host with: -p 8545:8545 -p 8546:8546 -p 26657:26657
make build-docker-node
make run-local-setup
make run-local-node
```

### Setup nginx proxy

```shell
git clone git@github.com:galadriel-ai/sei-network.git
cd sei-network
./deploy.sh
```

### Verify that proxy works

```shell
# Local - directly polling the node
curl -X POST localhost:8545 \
  -H "Content-Type: application/json" \
  --data \
    '
        {
          "jsonrpc": "2.0",
          "method": "eth_getBalance",
          "params": ["0xacD492cBFB5215bb44bAB69E64553a6a5164F8f7", "latest"],
          "id": 1
        }
    '
# Local - polling the node through nginx
curl -X POST localhost:80 \
  -H "Content-Type: application/json" \
  --data \
    '
        {
          "jsonrpc": "2.0",
          "method": "eth_getBalance",
          "params": ["0xacD492cBFB5215bb44bAB69E64553a6a5164F8f7", "latest"],
          "id": 1
        }
    '

# Anywhere
curl -X POST https://testnet.galadriel.com \
  -H "Content-Type: application/json" \
  --data \
    '
        {
          "jsonrpc": "2.0",
          "method": "eth_getBalance",
          "params": ["0xacD492cBFB5215bb44bAB69E64553a6a5164F8f7", "latest"],
          "id": 1
        }
    '

# get chainId
curl -X POST https://testnet.galadriel.com \
  -H "Content-Type: application/json"  \
  --data '{"method":"eth_chainId","params":[],"id":1,"jsonrpc":"2.0"}'
  
  
curl -X POST https://testnet.galadriel.com \
  -H "Content-Type: application/json"  \
  --data '{"jsonrpc":"2.0","method":"eth_getTransactionByHash","params":["0xd1011f25de7e234c3002fd476f090425cec2122fc0569168c3567a6178931431"],"id":1}'

```

### Admin

Send funds from validator admin:

```shell
# Send 100SEI?
# 100000000000000000000 =  100 GAL
# 1000000000000000000   =    1 GAL
seid tx evm send 0xacD492cBFB5215bb44bAB69E64553a6a5164F8f7 1000000000000000000 --from admin
seid tx evm send 0x3FABFC6ae7A14c9abBad96Ac2704a7F8D555a079 1000000000000000000 --from admin
seid tx evm send 0xAAA662c1cB3143f2B096279f48A70a15fbAf1CBd 100000000000000000000 --from admin
```
