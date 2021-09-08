# Конфигурация Cardano ноды

Давайте начнем с создания директорий для нашей ноды:

```text
cd 
mkdir -p cnode
cd cnode
mkdir -p config db sockets keys logs scripts  
cd config
```

Давайте скачаем файлы конфигурации

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

Ваш блок-продюсер \(БП\) должен связываться только с вашими реле. Ваши реле должны взаимодействовать с другими реле и вашим \( БП\).Все это настраивается в файле mainnet-topology.json. Ниже приведен пример для реле

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

Ваши реле должны быть на другом порту, нежели ваш БП. давайте настроим ваши реле на порт 2007 и вашего продюсера на порт 2008. когда вы запустите эту команду на продюсере, измените порт на 2007.

```text
cardano-node run --database-path /home/cardano/cnode/db --socket-path /home/cardano/cnode/sockets/node.socket --port 2008 --config /home/cardano/cnode/config/mainnet-config.json  --topology /home/cardano/cnode/config/mainnet-topology.jso
```

Если вы видите цветной текст, ваш нод был правильно сконфигурирован. Теперь вы можете отменить процесс, нажав Cntl+C

