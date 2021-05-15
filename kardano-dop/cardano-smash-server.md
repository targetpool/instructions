# Cardano Smash Server

Установка SMASH-сервера

Для начала необходимо установить необходимые для работы smash-сервера пакеты: 

```text
sudo apt install libpq-dev postgresql mkdir cardano-db-sync && cd cardano-db-sync curl -s 
https://hydra.iohk.io/build/6077812/download/1/cardano-db-sync-9.0.0-
 linux.tar.gz -o cardano-db-sync-9.0.0-linux.tar.gz tar xzf cardano-db-sync-9.0.0-linux.tar.gz cp cardano-db-sync ~/.local/bin/ 
```

Далее рекомендуем создать директорию\(если она не создана\), в которую будут клонировать проекты с github. В нашем случае это директория git. 

```text
mkdir ~/git cd ~/git
```

 Затем клонируем реапозиторий проекта и переходим в его директорию. 

```text
git clone 
https://github.com/input-output-hk/smash
 cd smash/ 
```

На всякий случай запускаем обновление компонентов cabal, а затем сборку проекта smash 

```text
cabal update cabal build smash && cabal install smash
```

Если все прошло успешно, то исполняемый файл smash-сервера можно скопировать в папку с остальными бинарниками cardano. 

```text
cp dist-newstyle/build/x86_64-linux/ghc-8.10.2/smash-1.4.0/x/smash- exe/build/smash-exe/smash-exe ~/.local/bin/
```

Затем создаем файл pgpass необходмый для дальнейшей конфигурации и работы smash-сервера. 

```text
nano ~/cnode/config/pgpass
```

 со следующим текстом: /var/run/postgresql:5432:smash:_:_ Копируем файлы схемы БД, скрипт инициализации БД и файл конфигурации для smash-сервера в каталог, из которого потом будет запускаться smash-сервер: 

```text
cp -R schema ~/cnode/ cp scripts/postgresql-setup.sh ~/cnode/scripts/ cp config/mainnet-config.yaml ~/cnode/config/smash-mainnet-config.yaml 
```

проверяем запущен ли postgresql: 

```text
sudo systemctl status postgresql
```

 если не запущен то запускаем его: 

```text
sudo systemctl enable postgresql && sudo systemctl start postgresql
```

 Далее необходимо залогиниться пользователем postgres для того чтобы предоставить пользователю из под которого будет работать smash права на создание БД и таблиц\(в нашем случае это candano\): 

```text
sudo -i
sudo -u postgres createuser cardano psql
```

 ALTER ROLE cardano WITH SUPERUSER ALTER ROLE cardano WITH CREATEDB quit exit exit Если этого не сделать, то дальше ничего не будет работать. Переходим в директорию откуда будем запускать smash-сервер: 

```text
cd ~/cnode
```

 Создаем директорию для ledger-state: 

```text
mkdir state
```

 Создаем базу данных и структуру таблиц для smash: 

```text
SMASHPGPASSFILE=config/pgpass ./scripts/postgresql-setup.sh --createdb SMASHPGPASSFILE=config/pgpass smash-exe run-migrations --mdir ./schema
```

 Запускаем smash-сервер в режиме синхронизации\(в нашем случае синхронизация длилась более суток\): 

```text
tmux SMASHPGPASSFILE=config/pgpass smash-exe run-app-with-db-sync --config config/smash-mainnet-config.yaml --socket-path sockets/node.socket --schema- dir schema/ --state-dir state/
```

 \(в этом режиме smash-сервер и нода cardano в режиме релея во время пиковой нагрузки утилизируют около 13Гб ОЗУ, cardano-node — 4.3Гб и smash-exe — 8-8.5\) SMASHPGPASSFILE=config/pgpass smash-exe run-app-with-db-sync --config config/smash-mainnet-config.yaml --socket-path sockets/node.socket --schema- dir schema/ --state-dir state/ +RTS --disable-delayed-os-memory -return -RTS \(потребление ОЗУ ~ 11,5Гб\) SMASHPGPASSFILE=config/pgpass smash-exe run-app-with-db-sync --config config/smash-mainnet-config.yaml --socket-path sockets/node.socket --schema- dir schema/ --state-dir state/ +RTS -N2 -A16m \(потребление ОЗУ ~ 11-11,5Гб, и на мой взгляд уменьшено количество активных потоков до 2-х\)

