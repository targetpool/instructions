# Ausführen eines Cardano Node

Da wir nun wissen, dass unser Knoten ordnungsgemäß funktioniert, müssen wir ihn über den Dienst starten, damit er immer im Hintergrund läuft

Erstellen Sie die Datei cardano--node.service in /home/cardano und fügen Sie den folgenden Text ein, nachdem Sie die Befehlszeile ausgeführt haben.

\[Unit\] Description=Shelley Pioneer Pool After=multi-user.target

\[Service\] Type=simple EnvironmentFile=/home/cardano/cnode/config/cardano-node.environment ExecStart=/home/cardano/.local/bin/cardano-node run --config $CONFIG --topology $TOPOLOGY --database-path $DBPATH --socket-path $SOCKETPATH --host-addr $HOSTADDR --port $PORT KillSignal = SIGINT RestartKillSignal = SIGINT StandardOutput=syslog StandardError=syslog SyslogIdentifier=trgt\_event LimitNOFILE=32768

Restart=on-failure RestartSec=15s WorkingDirectory=~ User=cardano Group=users

\[Install\] WantedBy=multi-user.target

```text
nano ~/cardano-node.service
```



Wir haben die Datei an einem temporären Ort erstellt, da der Cardano-Benutzer nicht die Rechte hat, eine Datei dort zu erstellen, wo wir sie haben wollen, also müssen wir sie jetzt mit dem Sudo-Befehl an ihren permanenten Ort kopieren.

```text
sudo cp ~/cardano-node.service /usr/lib/systemd/system
```

Nun müssen wir eine zweite Datei erstellen und diesen Text mit der Portnummer des Cardano-Knotens anstelle von 1234 einfügen \(diesen Text einfügen\)

CONFIG="/home/cardano/cnode/config/mainnet-config.json" TOPOLOGY="/home/cardano/cnode/config/mainnet-topology.json" DBPATH="/home/cardano/cnode/db/" SOCKETPATH="/home/cardano/cnode/sockets/node.socket" HOSTADDR="0.0.0.0" PORT="1234"

```text
nano ~/cnode/config/cardano-node.environment
```

Nun müssen wir den cardano-Node aktivieren

```text
 sudo systemctl enable cardano-node
```

Nun müssen wir den Knoten starten. \(um den Knoten zu stoppen, ersetzen Sie "start" durch "stop"\) in der Befehlszeile

```text
 sudo systemctl start cardano-node
```

Jetzt müssen wir den Status überprüfen. Wenn alles richtig funktioniert, sehen Sie den grünen Text "Active Running".

```text
 sudo systemctl status cardano-node
```



