# Установка ноды кардано

Давайте начнем с загрузки исходного кода Cardano Node из git \(github\).

```text
cd ~/git
git clone https://github.com/input-output-hk/cardano-node.git
```

> cardano@localhost:~$  **git clone https://github.com/input-output-hk/cardano-node.git**  
> Cloning into 'cardano-node'...  
> remote: Enumerating objects: 18, done.  
> remote: Counting objects: 100% \(18/18\), done.  
> remote: Compressing objects: 100% \(16/16\), done.  
> remote: Total 17996 \(delta 3\), reused 4 \(delta 1\), pack-reused 17978  
> Receiving objects: 100% \(17996/17996\), 5.66 MiB \| 1.62 MiB/s, done.  
> Resolving deltas: 100% \(11955/11955\), done.

Теперь у вас должна быть новая папка - cardano-node с исходным кодом ноды cardano, давайте перейдем в эту папку и выберем, какую версию мы хотим установить \(скомпилировать\).

```text
cd cardano-node
git fetch --all --recurse-submodules --tags
```



> cardano@localhost:~/cardano-node$ **cd** **cardano-node**  
> cardano@localhost:~$ **git fetch --all --tags && git tag**   
> Fetching origin  
> .......  
> 1.18.0  
> 1.18.1  
> 1.19.1  
> .......  
> 1.25.1

\(на момент написания данной инструкции\) последняя версия Mainnet Tag - 1.26.1, так что давайте ее скачаем!

```text
 git checkout tags/1.26.1
```

Давайте установим с помощью GHC.

```text
cabal configure --with-compiler=ghc-8.10.2
```

Начиная с версии 1.14.x, нам нужно добавить библиотеки libsodium на ноду Cardano, так что давайте это сделаем.

```text
echo "package cardano-crypto-praos" >>  cabal.project.local
echo "  flags: -external-libsodium-vrf" >>  cabal.project.local
```

По каким-то причинам мое SSH-соединение часто прерывается на этом этапе, поэтому давайте запустим TMUX, и там мы начнем установку. Это займет несколько часов

```text
tmux
cabal build all
```

давайте скопируем скомпилированные файлы bin \(executive\) в папку, которую мы ранее создали: .local/bin

#### Теперь нам нужно выключить сервер и сделать снэпшот машины

sudo shutdown -h now

![](.gitbook/assets/image%20%2817%29.png)

```text
mkdir -p ~/.local/bin/
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-cli-1.25.1/x/cardano-cli/build/cardano-cli/cardano-cli ~/.local/bin/
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-node-1.25.1/x/cardano-node/build/cardano-node/cardano-node ~/.local/bin/
```

Проверим установленную версию и местоположение

```text
which cardano-node && which cardano-cli
cardano-node --version
cardano-cli --version
```

> cardano-node 1.26.1 - linux-x86\_64 - ghc-8.10   
> git rev 62f38470098fc65e7de5a4b91e21e36ac30799f3

> cardano-cli 1.26.1 - linux-x86\_64 - ghc-8.10   
> git rev 62f38470098fc65e7de5a4b91e21e36ac30799f3

