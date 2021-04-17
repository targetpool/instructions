# Апгрейд с версии 1.25.1 на 1.26.1

#### Апгрейд с версии 1.25.1 на 1.26.1

Если вы используете версию 1.25.1, вам необходимо обновить ваш пул. Пожалуйста, воспользуйтесь приведенной ниже инструкцией, чтобы обновить ваш пул. вам нужно будет создать резервную копию ваших двоичных файлов.

```text
cd .local/bin/​# let's create a folder with the version numbermkdir -p $(cardano-node version | grep -oP '(?<=cardano-node )[0-9\.]+')​# copying files to the created foldercp cardano-node $(cardano-node version | grep -oP '(?<=cardano-node )[0-9\.]+')/cp cardano-cli $(cardano-node version | grep -oP '(?<=cardano-node )[0-9\.]+')/
```

теперь нам нужно обновить и модернизировать пакеты

```text
sudo apt-get update -y
sudo apt-get upgrade -y​
```

Теперь нам нужно создать каталог, в который мы будем загружать код.

```text
cd ~
mkdir -p source
cd source
```

```text
rm -rf cardano-node
```

давайте загрузим источник из GIT

```text
git clone https://github.com/input-output-hk/cardano-node.git

cd cardano-node
git fetch --all --recurse-submodules --tags

git checkout tags/1.26.1
```

####  обновите Кабал и установите с помощью GHC

```text
cabal clean
cabal update

cabal configure --with-compiler=ghc-8.10.2
```

Добавление флагов для библиотеки Libsodium

```text
echo "package cardano-crypto-praos" >>  cabal.project.local
echo "  flags: -external-libsodium-vrf" >>  cabal.project.local
```

```text
build all
```

​

  перед выполнением команд, приведенных ниже ВЫ ДОЛЖНЫ ОСТАНОВИТЬ ВАШУ НОДУ!

```text
mkdir -p ~/.local/bin/
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-cli-1.26.1/x/cardano-cli/build/cardano-cli/cardano-cli ~/.local/bin/
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-node-1.26.1/x/cardano-node/build/cardano-node/cardano-node ~/.local/bin/
```

Проверьте, правильно ли вы все установили.

```text
cardano-node --version
cardano-cli --version# let's check if we have successfully installed the latst cardano-node and cardano-cli versions.which cardano-node && which cardano-clicardano-node --versioncardano-cli --version
```

> cardano-node --version cardano-node 1.26.1 - linux-x86\_64 - ghc-8.10

> cardano-cli --version cardano-cli 1.26.1 - linux-x86\_64 - ghc-8.10

теперь вы можете скопировать архив на другие машины, производителю и другим реле, если они у вас есть.

```text
tar -czvf archive.tar.gz ~/cnode/db ~/.local/bin/cardano-node ~/.local/bin/cardano-cli
```

 Скопируйте файл на сервер, который вы хотите обновить \(вставьте IP другой машины\).

```text
 scp archive.tar.gz cardano@your_remote_server_ip:/home/cardano/
```

если вы хотите подключиться к серверу с помощью ssh \(замените также открытый ключ на ваше имя\)

```text
scp -i ~/.ssh/id_rsa archive.tar.gz cardano@your_ip:.
```

Теперь вам нужно распаковать файлы на сервере, куда вы их скопировали.

```text
tar -xvf archive.tar.gz 
```

Теперь остановите узел кардано и выполните следующие команды. Вам нужно удалить старый db, переместить новый db и заменить двоичные файлы.

```text
cd
rm -rf ~/cnode/db 
mv home/cnode/db ~/cnode/db
mv home/.local/bin/* ~/.local/bin 
```

Мы закончили. Вы можете запустить Ноду.

​

