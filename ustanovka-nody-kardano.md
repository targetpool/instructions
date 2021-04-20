# Cardano Node Instalation

Beginnen wir mit dem Herunterladen des Cardano Node-Quellcodes von git \(github\).

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

Jetzt sollten Sie einen neuen Ordner haben - cardano-node mit dem Quellcode des cardano-Knotens. Gehen wir in diesen Ordner und wählen wir die Version aus, die wir installieren \(kompilieren\) wollen.

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
> 1.26.2

\(zum Zeitpunkt dieses Schreibens\) ist die neueste Version von Mainnet Tag 1.26.2, also laden wir sie herunter!

```text
 git checkout tags/1.26.2
```

Lassen Sie uns das mit GHC einrichten.

```text
cabal configure --with-compiler=ghc-8.10.2
```

Ab Version 1.14.x müssen wir die libsodium-Bibliotheken zum Cardano-Knoten hinzufügen, also lassen Sie uns das tun.

```text
echo "package cardano-crypto-praos" >>  cabal.project.local
echo "  flags: -external-libsodium-vrf" >>  cabal.project.local
```

Aus irgendeinem Grund wird meine SSH-Verbindung an dieser Stelle oft unterbrochen, also starten wir TMUX und die Installation. Dies wird ein paar Stunden dauern

```text
tmux
cabal build all
```

Kopieren wir die kompilierten bin-Dateien \(Ausführungsdateien\) in den Ordner, den wir zuvor erstellt haben: .local/bin Jetzt müssen wir den Server herunterfahren und einen Snapshot des Rechners erstellen sudo shutdown -h jetzt

![](.gitbook/assets/image%20%2817%29.png)

```text
mkdir -p ~/.local/bin/
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-cli-1.25.1/x/cardano-cli/build/cardano-cli/cardano-cli ~/.local/bin/
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-node-1.25.1/x/cardano-node/build/cardano-node/cardano-node ~/.local/bin/
```

Installierte Version und Speicherort prüfen

```text
which cardano-node && which cardano-cli
cardano-node --version
cardano-cli --version
```

> cardano-node 1.26.2 - linux-x86\_64 - ghc-8.10   
> git rev 62f38470098fc65e7de5a4b91e21e36ac30799f3

> cardano-cli 1.26.2 - linux-x86\_64 - ghc-8.10   
> git rev 62f38470098fc65e7de5a4b91e21e36ac30799f3

