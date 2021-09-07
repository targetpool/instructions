# Setting Up Cardano service to start and stop the nodes



Now that we know that our node is functioning properly we need to start it using the service so it will always run in the background



Create file cardano--node.service in /home/cardano and paste the following text after running the command line

\[Unit\] Description=Shelley Pioneer Pool After=multi-user.target

\[Service\] Type=simple EnvironmentFile=/home/cardano/cnode/config/cardano-node.environment ExecStart=/home/cardano/.local/bin/cardano-node run --config $CONFIG --topology $TOPOLOGY --database-path $DBPATH --socket-path $SOCKETPATH --host-addr $HOSTADDR --port $PORT KillSignal = SIGINT RestartKillSignal = SIGINT StandardOutput=syslog StandardError=syslog SyslogIdentifier=trgt\_event LimitNOFILE=32768

Restart=on-failure RestartSec=15s WorkingDirectory=~ User=cardano Group=users

\[Install\] WantedBy=multi-user.target

```text
nano ~/cardano-node.service
```





We created the file in a temporary location because Cardano user does not have rights to create a file where we want it, so Now we need to copy it in its permanent place using sudo command. 

```text
sudo cp ~/cardano-node.service /usr/lib/systemd/system
```

Now we need to create a second file. and paste this text with your cardano port number instead of 1234 \(insert this text\)

CONFIG="/home/cardano/cnode/config/mainnet-config.json" TOPOLOGY="/home/cardano/cnode/config/mainnet-topology.json" DBPATH="/home/cardano/cnode/db/" SOCKETPATH="/home/cardano/cnode/sockets/node.socket" HOSTADDR="0.0.0.0" PORT="1234"

```text
nano ~/cnode/config/cardano-node.environment
```





```text
 sudo systemctl enable cardano-node
```

```text
 sudo systemctl start cardano-node
```

```text
 sudo systemctl status cardano-node
```

