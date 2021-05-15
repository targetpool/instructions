# Обновление с версии 1.26.2 на 1.27.0

Если вы используете версию 1.26.2, вам необходимо обновить свой пул до версии 1.27.0. Если у вас есть возможность сделать снапшот вашей машины, сделайте это сейчас, на всякий случай. Вам нужно будет создать резервную копию бинарных файлов

```text
cd .local/bin/​# let's create a folder with the version numbermkdir -p $(cardano-node version | grep -oP '(?<=cardano-node )[0-9\.]+')​# copying files to the created foldercp cardano-node $(cardano-node version | grep -oP '(?<=cardano-node )[0-9\.]+')/cp cardano-cli $(cardano-node version | grep -oP '(?<=cardano-node )[0-9\.]+')/
```

теперь нам нужно обновить пакеты

```text
sudo apt-get update -y
sudo apt-get upgrade -y​
```

Теперь нам нужно создать директорию, в которую мы будем загружать новую версию

```text
cd ~
mkdir -p source
cd source
```

```text
rm -rf cardano-node
```

давайте загрузим исходники с git

```text
git clone https://github.com/input-output-hk/cardano-node.git

cd cardano-node
git fetch --all --recurse-submodules --tags

git checkout tags/1.27.0
```

####  обновить cabal и установить с помощью GHC

```text
wget  https://downloads.haskell.org/~cabal/cabal-install-latest/cabal-install-3.4.0.0-x86_64-ubuntu-16.04.tar.xz
tar -xf cabal-install-3.4.0.0-x86_64-ubuntu-16.04.tar.xz
rm cabal-install-3.4.0.0-x86_64-ubuntu-16.04.tar.xz
mkdir -p ~/.local/bin
mv cabal ~/.local/bin/
```

```text
echo "export PATH=~/.local/bin:$PATH" >> ~/.bashrc 
source ~/.bashrc 
echo $PATH
```

```text
cabal clean
cabal update
cabal --version
```

вы должны увидеть что-то вроде этого

> **cardano@localhost**:**~**$  cabal update  
> cabal --version  
> Config file path source is default config file.  
> Config file /home/cardano/.cabal/config not found.  
> Writing default configuration to /home/cardano/.cabal/config  
> Downloading the latest package list from hackage.haskell.org

> **cabal-install version 3.4.0.0  
> compiled using version 3.4.0.0 of the Cabal library**

Добавление флагов для библиотеки libsodium

```text
cabal configure --with-compiler=ghc-8.10.2
```

```text
echo "package cardano-crypto-praos" >>  cabal.project.local
echo "  flags: -external-libsodium-vrf" >>  cabal.project.local
```

```text
build all
```

​

Теперь ВЫ ДОЛЖНЫ ОСТАНОВИТЬ СВОЮ НОДУ перед выполнением команд ниже!

```text
mkdir -p ~/.local/bin/
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-cli-1.27.0/x/cardano-cli/build/cardano-cli/cardano-cli ~/.local/bin/
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-node-1.27.0/x/cardano-node/build/cardano-node/cardano-node ~/.local/bin/
```

Проверьте, правильно ли вы все установили.

```text
cardano-node --version
cardano-cli --version# let's check if we have successfully installed the latst cardano-node and cardano-cli versions.which cardano-node && which cardano-clicardano-node --versioncardano-cli --version
```

> cardano-node --version cardano-node 1.27.0 - linux-x86\_64 - ghc-8.10

> cardano-cli --version cardano-cli 1.27.0 - linux-x86\_64 - ghc-8.10

теперь вы можете копировать бинарники на другие машины, на producer и другие реле, если они у вас есть

```text
tar -czvf archive.tar.gz ~/cnode/db ~/.local/bin/cardano-node ~/.local/bin/cardano-cli
```

Скопируйте файл на сервер, который вы хотите обновить \(вставьте IP другой машины\)

```text
 scp archive.tar.gz cardano@your_remote_server_ip:/home/cardano/
```

если вы хотите подключиться к серверу с помощью ssh \(замените также открытый ключ на имя вашего файла\)

```text
scp -i ~/.ssh/id_rsa archive.tar.gz cardano@your_ip:.
```

теперь вам нужно разархивировать файлы на сервере, куда вы их скопировали

```text
tar -xvf archive.tar.gz 
```

Теперь остановите ноду cardano и выполните следующие команды. Вам нужно удалить старую базу данных, переместить новую базу данных и заменить бинарники.

```text
cd
rm -rf ~/cnode/db 
mv home/cnode/db ~/cnode/db
mv home/.local/bin/* ~/.local/bin 
```

все готово. можно запускать ноду.

