# Installieren Cabal, GHC, Libsodium

Verbinden Sie sich mit dem Server mit dem Cardano-Benutzer, root - verwenden Sie keine weiteren

![](.gitbook/assets/image%20%2819%29.png)

Um cardano erfolgreich installieren \(kompilieren\) zu können, müssen wir sicherstellen, dass wir alle notwendigen Updates haben! Um sie herunterzuladen und zu installieren, geben Sie die folgenden Befehle ein:

```
sudo apt-get update -y
sudo apt-get upgrade -y
```

```bash
sudo apt-get install -y curl python3 build-essential pkg-config libffi-dev libgmp-dev libssl-dev libtinfo-dev systemd libsystemd-dev libsodium-dev zlib1g-dev yarn make g++ jq libncursesw5 libtool autoconf git tmux htop nload
export LD_LIBRARY_PATH="/usr/local/lib:$LD_LIBRARY_PATH"
```

Kabale Installation

 Wir werden Cardano mit Cabal installieren. Wir werden die empfohlene Version 3.4.0.0 verwenden und sie in unserem lokalen bin-Ordner \(.local/bin\) installieren.

```bash
wget https://downloads.haskell.org/~cabal/cabal-install-3.4.0.0/cabal-install-3.4.0.0-aarch64-ubuntu-18.04.tar.xz
tar -xf cabal-install-3.4.0.0-aarch64-ubuntu-18.04.tar.xz
rm cabal-install-3.4.0.0-aarch64-ubuntu-18.04.tar.xz cabal.sig
mkdir -p ~/.local/bin
mv cabal ~/.local/bin/
```

Jetzt sollten Sie cabal im Ordner ~/.local/bin/ installiert haben, jetzt müssen wir nur noch sicherstellen, dass das System die cabal bin\(executive\)-Datei auch finden kann. Fügen wir diese Informationen zu unserer Benutzerprofildatei \(.bashrc\) hinzu, laden sie neu \(mit dem Befehl source\) und prüfen, ob unser Ordner ~/.local/bin/ aufgelistet ist.

```bash
echo "export PATH=~/.local/bin:$PATH" >> ~/.bashrc 
source ~/.bashrc 
echo $PATH
```

Prüfen wir nun, ob die neueste Version des Cabal-Pakets installiert ist.

```bash
cabal update
cabal --version
```

sollten Sie etwas Ähnliches sehen:

> **cardano@localhost**:**~**$  cabal update  
> cabal --version  
> Config file path source is default config file.  
> Config file /home/cardano/.cabal/config not found.  
> Writing default configuration to /home/cardano/.cabal/config  
> Downloading the latest package list from hackage.haskell.org

> **cabal-install version 3.4.0.0  
> compiled using version 3.4.0.0 of the Cabal library**

## GHC Installation

Gehen wir zu den nächsten Schritten über - Installation von GHC - dem Haskell-Code-Compiler \(Cardano, geschrieben in Haskell \)

```bash
mkdir -p ~/git
cd ~/git

wget https://downloads.haskell.org/ghc/8.10.2/ghc-8.10.2-x86_64-deb9-linux.tar.xz
tar -xf ghc-8.10.2-x86_64-deb9-linux.tar.xz
rm ghc-8.10.2-x86_64-deb9-linux.tar.xz
cd ghc-8.10.2
./configure
sudo make install
```

Lassen Sie uns sicherstellen, dass alles richtig eingestellt ist:

```bash
ghc --version
```

sollten Sie etwas Ähnliches sehen:

> cardano@localhost:~$ **ghc --version**  
> The Glorious Glasgow Haskell Compilation System, version 8.10.2

## Libsodium Installation

Erstellen Sie einen Git-Ordner, in dem Sie den Quellcode der libsodium-Bibliothek kompilieren:

```bash
cd ~/git
```

Laden wir die libsodium-Bibliothek herunter und installieren sie \(wir benötigen einen bestimmten Zweig der Bibliothek, folgen Sie also der Anleitung\).

```bash
git clone https://github.com/input-output-hk/libsodium
cd libsodium
git checkout 66f017f1
./autogen.sh
./configure
make
sudo make install
```

Fügen wir den folgenden Pfad zu unserer .bashrc-Datei hinzu

```bash
echo "export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH" >> ~/.bashrc
echo "export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH"     >> ~/.bashrc
source ~/.bashrc
```

