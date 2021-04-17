# Upgrade von Version 1.25.1 auf 1.26.1

#### Upgrade von Version 1.25.1 auf 1.26.1

Wenn Sie die Version 1.25.1 verwenden, müssen Sie Ihren Pool aktualisieren. Bitte verwenden Sie die folgenden Anweisungen, um Ihren Pool zu aktualisieren. müssen Sie eine Sicherungskopie Ihrer Binärdateien erstellen.

```text
cd .local/bin/​# let's create a folder with the version numbermkdir -p $(cardano-node version | grep -oP '(?<=cardano-node )[0-9\.]+')​# copying files to the created foldercp cardano-node $(cardano-node version | grep -oP '(?<=cardano-node )[0-9\.]+')/cp cardano-cli $(cardano-node version | grep -oP '(?<=cardano-node )[0-9\.]+')/
```

Jetzt müssen wir die Pakete aktualisieren und upgraden

```text
sudo apt-get update -y
sudo apt-get upgrade -y​
```

Nun müssen wir ein Verzeichnis erstellen, in das wir den Code laden.

```text
cd ~
mkdir -p source
cd source
```

```text
rm -rf cardano-node
```

Lassen Sie uns den Quellcode von GIT herunterladen

```text
git clone https://github.com/input-output-hk/cardano-node.git

cd cardano-node
git fetch --all --recurse-submodules --tags

git checkout tags/1.26.1
```

####  Cabal aktualisieren und mit GHC installieren

```text
cabal clean
cabal update

cabal configure --with-compiler=ghc-8.10.2
```

Hinzufügen von Flags für die Libsodium-Bibliothek

```text
echo "package cardano-crypto-praos" >>  cabal.project.local
echo "  flags: -external-libsodium-vrf" >>  cabal.project.local
```

```text
build all
```

​

  vor dem Ausführen der folgenden Befehle MÜSSEN SIE IHREN NODE STOPPEN!

```text
mkdir -p ~/.local/bin/
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-cli-1.26.1/x/cardano-cli/build/cardano-cli/cardano-cli ~/.local/bin/
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-node-1.26.1/x/cardano-node/build/cardano-node/cardano-node ~/.local/bin/
```

Stellen Sie sicher, dass Sie alles korrekt installiert haben.

```text
cardano-node --version
cardano-cli --version# let's check if we have successfully installed the latst cardano-node and cardano-cli versions.which cardano-node && which cardano-clicardano-node --versioncardano-cli --version
```

> cardano-node --version cardano-node 1.26.1 - linux-x86\_64 - ghc-8.10

> cardano-cli --version cardano-cli 1.26.1 - linux-x86\_64 - ghc-8.10

können Sie nun das Archiv auf andere Maschinen, auf den Hersteller und auf andere Relais kopieren, wenn Sie diese haben.

```text
tar -czvf archive.tar.gz ~/cnode/db ~/.local/bin/cardano-node ~/.local/bin/cardano-cli
```

Kopieren Sie die Datei auf den Server, den Sie aktualisieren möchten \(fügen Sie die IP eines anderen Rechners ein\).

```text
 scp archive.tar.gz cardano@your_remote_server_ip:/home/cardano/
```

wenn Sie sich über ssh mit dem Server verbinden wollen \(ersetzen Sie auch den öffentlichen Schlüssel durch Ihren Dateinamen\)

```text
scp -i ~/.ssh/id_rsa archive.tar.gz cardano@your_ip:.
```

Nun müssen Sie die Dateien auf dem Server, die Sie kopiert haben, dekomprimieren.

```text
tar -xvf archive.tar.gz 
```

Halten Sie nun den Cardano Node an und führen Sie die folgenden Befehle aus. Sie müssen die alte db löschen, die neue db verschieben und die Binärdateien ersetzen.

```text
cd
rm -rf ~/cnode/db 
mv home/cnode/db ~/cnode/db
mv home/.local/bin/* ~/.local/bin 
```

Wir sind hier fertig. Sie können Node starten.

​

