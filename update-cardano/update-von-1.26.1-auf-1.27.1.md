# Update von 1.26.1 auf 1.27.1

Wenn Sie die Version 1.26.2 verwenden, müssen Sie Ihren Pool auf 1.27.0 aktualisieren. Wenn Sie die Möglichkeit haben, einen Snapshot Ihres Rechners zu erstellen, sollten Sie dies jetzt tun, nur für den Fall. Sie müssen ein Backup Ihrer Binärdateien erstellen

```text
cd .local/bin/​# let's create a folder with the ver
```

jetzt müssen wir die Pakete aktualisieren und upgraden

```text
sudo apt-get update -y
sudo apt-get upgrade -y​
```

nun müssen wir ein Verzeichnis erstellen, in das wir den Code herunterladen

```text
cd ~
mkdir -p source
cd source
```

```text
rm -rf cardano-node
```

Laden wir den Quellcode von Git herunter

```text
git clone https://github.com/input-output-hk/cardano-node.git

cd cardano-node
git fetch --all --recurse-submodules --tags

git checkout tags/1.27.0
```

####  cabal aktualisieren und mit GHC installieren

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

sollten Sie etwa Folgendes sehen

> **cardano@localhost**:**~**$  cabal update  
> cabal --version  
> Config file path source is default config file.  
> Config file /home/cardano/.cabal/config not found.  
> Writing default configuration to /home/cardano/.cabal/config  
> Downloading the latest package list from hackage.haskell.org

> **cabal-install version 3.4.0.0  
> compiled using version 3.4.0.0 of the Cabal library**

Hinzufügen von Flags für die libsodium-Bibliothek

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

JETZT MÜSSEN SIE IHREN NODE STOPPEN, bevor Sie die folgenden Befehle ausführen!

```text
mkdir -p ~/.local/bin/
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-cli-1.27.0/x/cardano-cli/build/cardano-cli/cardano-cli ~/.local/bin/
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-node-1.27.0/x/cardano-node/build/cardano-node/cardano-node ~/.local/bin/
```

Prüfen Sie, ob Sie alles richtig installiert haben.

```text
cardano-node --version
cardano-cli --version# let's check if we have successfully installed the latst cardano-node and cardano-cli versions.which cardano-node && which cardano-clicardano-node --versioncardano-cli --version
```

> cardano-node --version cardano-node 1.27.0 - linux-x86\_64 - ghc-8.10

> cardano-cli --version cardano-cli 1.27.0 - linux-x86\_64 - ghc-8.10

jetzt können Sie das Archiv auf andere Rechner, den Hersteller und andere Relais kopieren, falls Sie solche haben

```text
tar -czvf archive.tar.gz ~/cnode/db ~/.local/bin/cardano-node ~/.local/bin/cardano-cli
```

 Kopieren Sie die Datei auf den Server, den Sie aktualisieren möchten \(geben Sie die IP eines anderen Rechners ein\)

```text
 scp archive.tar.gz cardano@your_remote_server_ip:/home/cardano/
```

wenn Sie eine Verbindung zum Server mit ssh herstellen wollen \(ersetzen Sie auch den öffentlichen Schlüssel durch Ihren Dateinamen\)

```text
scp -i ~/.ssh/id_rsa archive.tar.gz cardano@your_ip:.
```

nun müssen Sie die Dateien auf dem Server entpacken, auf den Sie sie kopiert haben

```text
tar -xvf archive.tar.gz 
```

Halten Sie nun den Cardano-Node an und führen Sie die folgenden Befehle aus. Sie müssen die alte db entfernen, die neue db verschieben und die Binärdateien ersetzen

```text
cd
rm -rf ~/cnode/db 
mv home/cnode/db ~/cnode/db
mv home/.local/bin/* ~/.local/bin 
```

Sie sind fertig. Sie können den Node starten

