# Configuration of the Cardano node

Let's start by creating directories for our node:

```text
cd 
mkdir -p cnode
cd cnode
mkdir -p config db sockets keys logs scripts  
cd config
```

Let's download the configuration files

```text
#checking the latest built for configs
export LAST_BUILD=$(curl -s https://hydra.iohk.io/job/Cardano/cardano-node/cardano-deployment/latest-finished/download/1/index.html | grep -e "This item has moved" |  sed -e 's/.*build\/\(.*\)\/download.*/\1/')
#downloading configs
wget -q -O mainnet-config.json https://hydra.iohk.io/build/${LAST_BUILD}/download/1/mainnet-config.json
wget -q -O mainnet-alonzo-genesis.json https://hydra.iohk.io/build/${LAST_BUILD}/download/1/mainnet-alonzo-genesis.json
wget -q -O mainnet-byron-genesis.json https://hydra.iohk.io/build/${LAST_BUILD}/download/1/mainnet-byron-genesis.json
wget -q -O mainnet-shelley-genesis.json https://hydra.iohk.io/build/${LAST_BUILD}/download/1/mainnet-shelley-genesis.json
wget -q -O mainnet-topology.json https://hydra.iohk.io/build/${LAST_BUILD}/download/1/mainnet-topology.json

#list downloaded files
ls -al mainnet*
```

Your block producer \( BP\) must only communicate with your relays. Your relays must communicate with other relays and your \( BP\).All this is configured in the file mainnet-topology.json. Here is an example for relays

```text
cat > mainnet-topology.json << EOF
{
  "Producers": [
    {
      "addr": "вставьте ip-адрес вашего БП",
      "port": 2008,
      "valency": 2
    },
    {
      "addr": "relays-new.cardano-mainnet.iohk.io",
      "port": 2007,
      "valency": 2
    },
    {
      "addr": "68.183.6.89",
      "port": 2007,
      "valency": 1
    }
  ]
}
EOF
```

Your relays should be on a different port than your BP. let's set your relays to port 2007 and your producer to port 2008. when you run this command on the producer, change the port to 2001.

```text
cardano-node run --database-path /home/cardano/cnode/db --socket-path /home/cardano/cnode/sockets/node.socket --port 2007 --config /home/cardano/cnode/config/mainnet-config.json  --topology /home/cardano/cnode/config/mainnet-topology.json
```

If you see colored text, your node has been configured correctly. Now you can cancel the process by pressing Cntl+C

