# Запуск ноды Кардано

Ранее мы запустили реле в активной ssh сессии, что означает, что как только мы закроем браузер, узел перестанет работать.поэтому нам нужно запустить его с помощью службы, чтобы он всегда работал в фоновом режиме

Создайте файл cardano--node.service в /home/cardano и вставьте следующий текст после выполнения командной строки

\[Unit\] Description=Shelley Pioneer Pool After=multi-user.target

\[Service\] Type=simple EnvironmentFile=/home/cardano/cnode/config/cardano-node.environment ExecStart=/home/cardano/.local/bin/cardano-node run --config $CONFIG --topology $TOPOLOGY --database-path $DBPATH --socket-path $SOCKETPATH --host-addr $HOSTADDR --port $PORT KillSignal = SIGINT RestartKillSignal = SIGINT StandardOutput=syslog StandardError=syslog SyslogIdentifier=trgt\_event LimitNOFILE=32768

Restart=on-failure RestartSec=15s WorkingDirectory=~ User=cardano Group=users

\[Install\] WantedBy=multi-user.target

```text
nano ~/cardano-node.service
```

Мы создали файл во временном месте, потому что у пользователя Cardano нет прав на создание файла там, где мы хотим, поэтому теперь нам нужно скопировать его в постоянное место с помощью команды sudo.

```text
sudo cp ~/cardano-node.service /usr/lib/systemd/system
```

Теперь нам нужно создать второй файл. и вставьте этот текст с номером порта вашей ноды кардано вместо 1234 \(вставьте этот текст\)

CONFIG="/home/cardano/cnode/config/mainnet-config.json" TOPOLOGY="/home/cardano/cnode/config/mainnet-topology.json" DBPATH="/home/cardano/cnode/db/" SOCKETPATH="/home/cardano/cnode/sockets/node.socket" HOSTADDR="0.0.0.0" PORT="1234"

```text
nano ~/cnode/config/cardano-node.environment
```

Теперь нам нужно активировать ноду cardano

```text
 sudo systemctl enable cardano-node
```

Теперь нам нужно запустить ноду. \(Чтобы остановить ноду, замените "start" на "stop"\) в командной строке

```text
 sudo systemctl start cardano-node
```

Теперь нам нужно проверить состояние. Если все работает правильно, вы увидите зеленый текст "Active Running".

```text
 sudo systemctl status cardano-node
```

