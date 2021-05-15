# Обновление с версии 1.26.1 до 1.26.2

If you are running version 1.26.1 you need to upgrade your pool. Please use the instructions below to upgrade your pool.​

you will have to create a backup of your binaries

```text
cd .local/bin/​# let's create a folder with the version numbermkdir -p $(cardano-node version | grep -oP '(?<=cardano-node )[0-9\.]+')​# copying files to the created foldercp cardano-node $(cardano-node version | grep -oP '(?<=cardano-node )[0-9\.]+')/cp cardano-cli $(cardano-node version | grep -oP '(?<=cardano-node )[0-9\.]+')/
```

now we need to update and upgrade the packages

```text
sudo apt-get update -y
sudo apt-get upgrade -y​
```

now we need to create a directory where we will download the code

```text
cd ~
mkdir -p source
cd source
```

```text
rm -rf cardano-node
```

#### lets download the source from git

```text
git clone https://github.com/input-output-hk/cardano-node.git

cd cardano-node
git fetch --all --recurse-submodules --tags

git checkout tags/1.26.2
```

####  update cabal and install using GHC

```text
cabal clean
cabal update

cabal configure --with-compiler=ghc-8.10.2
```

#### Adding flags for the libsodium library

```text
echo "package cardano-crypto-praos" >>  cabal.project.local
echo "  flags: -external-libsodium-vrf" >>  cabal.project.local
```

```text
build all
```

​

NOW YOU HAVE TO STOP YOUR NODE before running commands below!

```text
mkdir -p ~/.local/bin/
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-cli-1.26.2/x/cardano-cli/build/cardano-cli/cardano-cli ~/.local/bin/
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-node-1.26.2/x/cardano-node/build/cardano-node/cardano-node ~/.local/bin/
```

#### Check if you have installed everything correctly.

```text
cardano-node --version
cardano-cli --version# let's check if we have successfully installed the latst cardano-node and cardano-cli versions.which cardano-node && which cardano-clicardano-node --versioncardano-cli --version
```

> cardano-node --version cardano-node 1.26.2 - linux-x86\_64 - ghc-8.10

> cardano-cli --version cardano-cli 1.26.2 - linux-x86\_64 - ghc-8.10

now you can copy the archive to other machines, to producer and other relays if you have them

```text
tar -czvf archive.tar.gz ~/cnode/db ~/.local/bin/cardano-node ~/.local/bin/cardano-cli
```

 Copy the file to the server that you want to upgrade \(insert IP of another machine\)

```text
 scp archive.tar.gz cardano@your_remote_server_ip:/home/cardano/
```

if you want to connect to server with ssh \(replace also the public key with your filename\)

```text
scp -i ~/.ssh/id_rsa archive.tar.gz cardano@your_ip:.
```

now you need to unzip the files on the server where you copied them to

```text
tar -xvf archive.tar.gz 
```

Now stop the cardano node and run the following commands. You need to remove the old db, move the new db and replace binaries

```text
cd
rm -rf ~/cnode/db 
mv home/cnode/db ~/cnode/db
mv home/.local/bin/* ~/.local/bin 
```

**you are all done. you can start the node**

