# Installing Cardano SMASH server

Warning! Instructions are correct and functional, but SMASH has memory leaks that we are working on fixing with the IOHK\#s technical team. We will keep you posted.

Installing the SMASH server   
First, you need to install the packages required for a smash-server:

```text
sudo apt install libpq-dev postgresql mkdir cardano-db-sync && cd cardano-db-sync curl -s 
https://hydra.iohk.io/build/6077812/download/1/cardano-db-sync-9.0.0-
 linux.tar.gz -o cardano-db-sync-9.0.0-linux.tar.gz tar xzf cardano-db-sync-9.0.0-linux.tar.gz cp cardano-db-sync ~/.local/bin/ 
```

Next, we recommend that you create a directory \(if you haven't created one\) where projects from github will be cloned. In our case, this is the git directory.

```text
mkdir ~/git cd ~/git
```

Then clone the project's repository and move to its directory.

```text
git clone 
https://github.com/input-output-hk/smash
 cd smash/ 
```

To be on the safe side, start updating the cabal components and then build the smash project

```text
cabal update cabal build smash && cabal install smash
```

If all went well, the executable file of the smash-server can be copied into the folder with the rest of the cardano binaries.

```text
cp dist-newstyle/build/x86_64-linux/ghc-8.10.2/smash-1.4.0/x/smash- exe/build/smash-exe/smash-exe ~/.local/bin/
```

Then create a pgpass file which is necessary for further configuration and operation of the smash-server. 

```text
nano ~/cnode/config/pgpass
```

 with the folowing text : /var/run/postgresql:5432:smash:_:_ Copy the database schema files, database initialization script and configuration file for the smash-server in the directory, from which we will then  run the smash-server:

```text
cp -R schema ~/cnode/ cp scripts/postgresql-setup.sh ~/cnode/scripts/ cp config/mainnet-config.yaml ~/cnode/config/smash-mainnet-config.yaml 
```

check if "postgresql" is launched: 

```text
sudo systemctl status postgresql
```

 If it is not running, then start it: 

```text
sudo systemctl enable postgresql && sudo systemctl start postgresql
```

 Next, you need to be logged in as postgres user to give the user who will run smash the rights to create the database and tables \(in our case candano\):

```text
sudo -i
sudo -u postgres createuser cardano psql
```

 ALTER ROLE cardano WITH SUPERUSER ALTER ROLE cardano WITH CREATEDB quit exit exit   
If you don't do this, nothing will work. Go to the directory from where we will run the smash-server:

```text
cd ~/cnode
```

 Create a directory for ledger-state:

```text
mkdir state
```

 Create a database and table structure for the smash:

```text
SMASHPGPASSFILE=config/pgpass ./scripts/postgresql-setup.sh --createdb SMASHPGPASSFILE=config/pgpass smash-exe run-migrations --mdir ./schema
```

 Start the smash-server in sync mode \(in our case, the synchronization lasted more than a day\):

```text
tmux SMASHPGPASSFILE=config/pgpass smash-exe run-app-with-db-sync --config config/smash-mainnet-config.yaml --socket-path sockets/node.socket --schema- dir schema/ --state-dir state/
```

 Run the smash-server in sync mode \(in our case, the synchronization lasted more than 24hours  \(in this mode, the smash-server and cardano-node in relay mode during the peak load utilize about 13GB of RAM, cardano-node - 4. 3GB and smash-exe 8-8.5\) SMASHPGPASSFILE=config/pgpass smash-exe run-app-with-db-sync --config config/smash-mainnet-config.yaml --socket-path sockets/node. socket --schema- dir schema/ --state-dir state/ +RTS --disable-delayed-os-memory -return -RTS \(RAM consumption ~ 11.5GB\) SMASHPGPASSFILE=config/pgpass smash-exe run-app-with-db-sync --config config/smash-mainnet-config. yaml --socket-path sockets/node.socket --schema- dir schema/ --state-dir state/ +RTS -N2 -A16m \(RAM consumption ~ 11-11.5Gb, and in my opinion reduced to 2\) active threads:\)

Translated with www.DeepL.com/Translator \(free version\)

