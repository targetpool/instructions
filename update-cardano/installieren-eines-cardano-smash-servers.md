# Installieren eines cardano SMASH-Servers

Warnung! Die Anweisungen sind korrekt und funktionsfähig, aber SMASH hat Speicherlecks, an deren Behebung wir zusammen mit dem technischen Team von IOHK arbeiten. Wir werden Sie auf dem Laufenden halten. Installieren des SMASH-Servers Zuerst müssen Sie die Pakete installieren, die für einen smash-server benötigt werden:

```text
sudo apt install libpq-dev postgresql mkdir cardano-db-sync && cd cardano-db-sync curl -s 
https://hydra.iohk.io/build/6077812/download/1/cardano-db-sync-9.0.0-
 linux.tar.gz -o cardano-db-sync-9.0.0-linux.tar.gz tar xzf cardano-db-sync-9.0.0-linux.tar.gz cp cardano-db-sync ~/.local/bin/ 
```

Als Nächstes empfehlen wir Ihnen, ein Verzeichnis zu erstellen \(falls Sie noch keins erstellt haben\), in das Projekte aus github geklont werden sollen. In unserem Fall ist dies das git-Verzeichnis.

```text
mkdir ~/git cd ~/git
```

Klonen Sie dann das Repository des Projekts und wechseln Sie in dessen Verzeichnis.

```text
git clone 
https://github.com/input-output-hk/smash
 cd smash/ 
```

Um auf Nummer sicher zu gehen, beginnen Sie mit der Aktualisierung der Cabal-Komponenten und erstellen Sie dann das Smash-Projekt

```text
cabal update cabal build smash && cabal install smash
```

Wenn alles gut gegangen ist, kann die ausführbare Datei des smash-server in den Ordner mit den restlichen cardano-Binärdateien kopiert werden.

```text
cp dist-newstyle/build/x86_64-linux/ghc-8.10.2/smash-1.4.0/x/smash- exe/build/smash-exe/smash-exe ~/.local/bin/
```

Erstellen Sie dann eine pgpass-Datei, die für die weitere Konfiguration und den Betrieb des smash-Servers notwendig ist. 

```text
nano ~/cnode/config/pgpass
```

 mit folgendem Text: /var/run/postgresql:5432:smash:: Kopieren Sie die Datenbankschemadateien, das Datenbankinitialisierungsskript und die Konfigurationsdatei für den smash-Server in das Verzeichnis, von dem aus wir dann den smash-Server starten werden:

```text
cp -R schema ~/cnode/ cp scripts/postgresql-setup.sh ~/cnode/scripts/ cp config/mainnet-config.yaml ~/cnode/config/smash-mainnet-config.yaml 
```

prüfen, ob "postgresql" gestartet ist:

```text
sudo systemctl status postgresql
```

 Wenn er nicht läuft, dann starten Sie ihn:

```text
sudo systemctl enable postgresql && sudo systemctl start postgresql
```

 Als nächstes müssen Sie als postgres-Benutzer angemeldet sein, um dem Benutzer, der smash ausführen wird, die Rechte zum Erstellen der Datenbank und der Tabellen zu geben \(in unserem Fall candano\):

```text
sudo -i
sudo -u postgres createuser cardano psql
```

 ALTER ROLE cardano WITH SUPERUSER ALTER ROLE cardano WITH CREATEDB quit exit exit Wenn Sie dies nicht tun, wird nichts funktionieren. Wechseln Sie in das Verzeichnis, von dem aus wir den smash-server starten werden:

```text
cd ~/cnode
```

 Erstellen Sie ein Verzeichnis für den Ledger-Zustand:

```text
mkdir state
```

 Erstellen Sie eine Datenbank und Tabellenstruktur für den Smash:

```text
SMASHPGPASSFILE=config/pgpass ./scripts/postgresql-setup.sh --createdb SMASHPGPASSFILE=config/pgpass smash-exe run-migrations --mdir ./schema
```

 tarten Sie den smash-server im Synchronisationsmodus \(in unserem Fall dauerte die Synchronisation mehr als einen Tag\):

```text
tmux SMASHPGPASSFILE=config/pgpass smash-exe run-app-with-db-sync --config config/smash-mainnet-config.yaml --socket-path sockets/node.socket --schema- dir schema/ --state-dir state/
```

 Lassen Sie den smash-server im sync-Modus laufen \(in unserem Fall dauerte die Synchronisation mehr als 24 Stunden \(in diesem Modus verbrauchen der smash-server und cardano-node im Relay-Modus während der Spitzenlast ca. 13GB RAM, cardano-node - 4. 3GB und smash-exe 8-8.5\) SMASHPGPASSFILE=config/pgpass smash-exe run-app-with-db-sync --config config/smash-mainnet-config.yaml --socket-path sockets/node. socket --schema- dir schema/ --state-dir state/ +RTS --disable-delayed-os-memory -return -RTS \(RAM-Verbrauch ~ 11,5GB\) SMASHPGPASSFILE=config/pgpass smash-exe run-app-with-db-sync --config config/smash-mainnet-config. yaml --socket-path sockets/node.socket --schema- dir schema/ --state-dir state/ +RTS -N2 -A16m \(RAM-Verbrauch ~ 11-11.5Gb, und meiner Meinung nach auf 2\) aktive Threads reduziert:\)

