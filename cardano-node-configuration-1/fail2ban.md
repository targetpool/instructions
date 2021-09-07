# Fail2Ban

```text
sudo apt-get install fail2ban
```

```text
sudo nano /etc/ssh/sshd_config
```

![](../.gitbook/assets/image%20%28107%29.png)

```text
sudo ufw allow proto tcp from any to any port 1234
```

```text
sudo ufw enable
```

```text
sudo systemctl restart sshd
```

```text
 sudo nano /etc/fail2ban/jail.conf
```

![](../.gitbook/assets/image%20%28106%29.png)

```text
sudo systemctl restart fail2ban
```

Setting up Cardano service



Create file cardano--node.service in /home/cardano and paste the following text 

```text
nano ~/cardano-node.service
```

\[Unit\] Description=Shelley Pioneer Pool After=multi-user.target

\[Service\] Type=simple EnvironmentFile=/home/cardano/cnode/config/cardano-node.environment ExecStart=/home/cardano/.local/bin/cardano-node run --config $CONFIG --topology $TOPOLOGY --database-path $DBPATH --socket-path $SOCKETPATH --host-addr $HOSTADDR --port $PORT KillSignal = SIGINT RestartKillSignal = SIGINT StandardOutput=syslog StandardError=syslog SyslogIdentifier=trgt\_event LimitNOFILE=32768

Restart=on-failure RestartSec=15s WorkingDirectory=~ User=cardano Group=users

\[Install\] WantedBy=multi-user.target

```text
 sudo cp ~/cardano-node.service /usr/lib/systemd/system
```

```text
nano ~/cnode/config/cardano-node.environment
```

and paste this text with your cardano port number instead of 1234

CONFIG="/home/cardano/cnode/config/mainnet-config.json" TOPOLOGY="/home/cardano/cnode/config/mainnet-topology.json" DBPATH="/home/cardano/cnode/db/" SOCKETPATH="/home/cardano/cnode/sockets/node.socket" HOSTADDR="0.0.0.0" PORT="1234"

