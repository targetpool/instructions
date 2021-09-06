# Install Cabal, GHC, Libsodium



Now we need to connect to the server with our newly created user  Cardano, root - we will no longer be using

![](.gitbook/assets/image%20%2819%29.png)

Now you want to download and install all of the updates

```
sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get install libdw-dev
```

```bash
sudo apt-get install -y curl python3 build-essential pkg-config libffi-dev libgmp-dev libssl-dev libtinfo-dev systemd libsystemd-dev libsodium-dev zlib1g-dev yarn make g++ jq libncursesw5 libtool autoconf git tmux htop nload
export LD_LIBRARY_PATH="/usr/local/lib:$LD_LIBRARY_PATH"
```

## Установка  Cabal

e will install Cardano with Cabal. We will use the recommended version 3.4.0.0 and install it in our local bin folder \(.local/bin\).

```bash
wget  https://downloads.haskell.org/~cabal/cabal-install-latest/cabal-install-3.4.0.0-x86_64-ubuntu-16.04.tar.xz
tar -xf cabal-install-3.4.0.0-x86_64-ubuntu-16.04.tar.xz
rm cabal-install-3.4.0.0-x86_64-ubuntu-16.04.tar.xz
mkdir -p ~/.local/bin
mv cabal ~/.local/bin/
```

Now you should have cabal installed in the ~/.local/bin/ folder, now we just have to make sure the system can find the cabal bin\(executive\) file too. Let's add this information to our user profile file \(.bashrc\), reload it \(using the source code command\), and check if we see our ~/.local/bin/ folder listed.

```bash
echo "export PATH=~/.local/bin:$PATH" >> ~/.bashrc 
source ~/.bashrc 
echo $PATH
```

Now let's check if the latest version of the Cabal package is installed.

```bash
cabal update
cabal --version
```

you should see something like this

> **cardano@localhost**:**~**$  cabal update  
> cabal --version  
> Config file path source is default config file.  
> Config file /home/cardano/.cabal/config not found.  
> Writing default configuration to /home/cardano/.cabal/config  
> Downloading the latest package list from hackage.haskell.org

> **cabal-install version 3.4.0.0  
> compiled using version 3.4.0.0 of the Cabal library**

## Installing  GHC

Let's move on to the next steps - installing GHC - the Haskell code compiler \(Cardano is written in Haskell \)

```bash
mkdir -p ~/git
cd ~/git
wget https://downloads.haskell.org/ghc/8.10.4/ghc-8.10.4-x86_64-deb9-linux-dwarf.tar.xz
tar -xf ghc-8.10.4-x86_64-deb9-linux-dwarf.tar.xz
rm ghc-8.10.4-x86_64-deb9-linux-dwarf.tar.xz
cd ghc-8.10.4
./configure
sudo make install
```

lets make sure that everything is installed correctly**:**

```bash
ghc --version
```

you should see something like this:

> cardano@localhost:~$ **ghc --version**  
> The Glorious Glasgow Haskell Compilation System, version 8.10.4

## Installing  Libsodium

Create folder git, in which we will compile libsodium library:

```bash
mkdir -p ~/git
cd ~/git
```

Download and install the libsodium library \(we need a specific branch of the library, so follow the manual\).

```bash
git clone https://github.com/input-output-hk/libsodium
cd libsodium
git checkout 66f017f1
./autogen.sh
./configure
make
sudo make install
```

Now we need to specify the following path to our .bashrc file

```bash
echo "export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH" >> ~/.bashrc
echo "export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH"     >> ~/.bashrc
source ~/.bashrc
```

