# Running a Node

If you'd like someone else to setup and manage your GoChain node, please check out [Blockdeamon](https://blockdaemon.com/). With three clicks 
you'll have a node up and running and they'll make sure it stays running for you.

If you want to do it yourself, please continue below.

This directory contains instructions for configuring and running GoChain with `docker-compose` on the `testnet`, `mainnet`, a private network, or a local development instance.

Instructions for running a *signing* node are [here](../signers/nodes).

## Prerequisites

Install `docker` and `docker-compose`.

* Docker > 18.0 ([install](https://docs.docker.com/install/))
* Docker-compose ([install](https://docs.docker.com/compose/install/))

<details>
  <summary>Simple Install Instructions</summary>

Docker:

```sh
sudo rm /var/lib/apt/lists/*
sudo apt-get update
curl -fsSL https://get.docker.com/ | sudo sh
docker info
```

Docker Compose:

```sh
curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
docker-compose --version
```
</details>

## Initial Configuration

\**Note: If you are moving from the testnet to the mainnet, it is best to start fresh in a new folder.*

1. Copy `docker-compose.yml` into your folder from either the [`testnet`](testnet) or [`mainnet`](mainnet) directory.
2. (Optional) Create a file `.env` to override the default variables: (see [`example.env`](example.env) for more details)
```
GOCHAIN_TAG=2.1.16
GOCHAIN_CACHE=2048
```
3. Launch `docker-compose`

```sh
docker-compose up -d
```

4. Make sure that node works.

```sh
docker logs -f node
```

## Common Commands

- Start: `docker-compose up -d`
- Stop: `docker-compose down`
- Follow Logs: `docker logs -f --tail 100 node`
- Restart Container: `docker-compose restart node`
- Restart All: `docker-compose down && docker-compose up -d`
- Console Attach: `docker run --rm -it -v $PWD:/gochain -w /gochain gcr.io/gochain-core/gochain gochain --datadir /gochain/node attach`
- Update image: `docker-compose pull`

### Console Commands

- Enode: `admin.nodeInfo.enode`
- Balance: `eth.getBalance('0xabcd')`
- Coinbase Balance (rewards): `eth.getBalance(eth.coinbase)` 
- Send Transaction (transfer rewards): `eth.sendTransaction({from:eth.coinbase,to:'0xabcd',value:1000000000000000000})`

More info on the console is available here: https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console

## Troubleshooting

If you are unable to diagnose a problem, you can try these steps in escalating order:

1) Repair: `docker-compose up -d`
2) Restart node: `docker-compose restart node`
3) Restart all: `docker-compose down && docker-compose up -d`
4) Restart docker: `service docker restart && docker-compose up -d`
5) Reboot machine, `docker-compose up -d`
