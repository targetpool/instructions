# Konfiguration des Cardano Node

Beginnen wir damit, die Verzeichnisse für unseren Knoten zu erstellen:

```text
cd 
mkdir -p cnode
cd cnode
mkdir -p config db sockets keys logs scripts  
cd config
```

Lassen Sie uns die Konfigurationsdateien herunterladen

```text
#checking the latest built for configs
export LAST_BUILD=$(curl -s https://hydra.iohk.io/job/Cardano/cardano-node/cardano-deployment/latest-finished/download/1/index.html | grep -e "This item has moved" |  sed -e 's/.*build\/\(.*\)\/download.*/\1/')
#downloading configs
wget -q -O mainnet-config.json https://hydra.iohk.io/build/${LAST_BUILD}/download/1/mainnet-config.json
wget -q -O mainnet-byron-genesis.json https://hydra.iohk.io/build/${LAST_BUILD}/download/1/mainnet-byron-genesis.json
wget -q -O mainnet-shelley-genesis.json https://hydra.iohk.io/build/${LAST_BUILD}/download/1/mainnet-shelley-genesis.json
wget -q -O mainnet-topology.json https://hydra.iohk.io/build/${LAST_BUILD}/download/1/mainnet-topology.json
#list downloaded files
ls -al mainnet*
```

Ihr Blockproducer \(BP\) darf nur mit Ihren Relais kommunizieren. Ihre Relais müssen mit anderen Relais und Ihrem \(BP\) kommunizieren, was in der Datei mainnet-topology.json konfiguriert wird. Nachfolgend finden Sie ein Beispiel für ein Relais

```text
cat > mainnet-topology.json << EOF
{
  "Producers": [
    {
      "addr": "Insert ip-адрес of your BP",
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

Ihre Relais sollten auf einem anderen Port liegen als Ihr BP. Setzen Sie Ihre Relais auf Port 2007 und Ihren Producer auf Port 2008. Wenn Sie diesen Befehl auf dem Producer ausführen, ändern Sie den Port auf 2001.

```text
cardano-node run --database-path /home/cardano/cnode/db --socket-path /home/cardano/cnode/sockets/node.socket --port 2007 --config /home/cardano/cnode/config/mainnet-config.json  --topology /home/cardano/cnode/config/mainnet-topology.jso
```

Wenn Sie farbigen Text sehen, wurde Ihr Knoten korrekt konfiguriert. Jetzt können Sie den Vorgang mit der Tastenkombination Strg+C abbrechen

