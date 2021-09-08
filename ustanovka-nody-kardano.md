# Cardano Node Installation

Let's start by downloading the Cardano Node source code from git \(github\).

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

Now you should have a new folder - cardano-node with the source code of the cardano node, let's go to this folder and choose which version we want to install \(compile\).

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
> 1.29.0

\(as of this writing\) the latest version of Mainnet Tag is 1.29.0, so let's download it!

```text
git checkout tags/1.29.0
```

Lets install using GHC.

```text
cabal clean
cabal update
cabal configure --with-compiler=ghc-8.10.4
```

Starting with version 1.14.x, we need to add libsodium libraries to the Cardano node, so let's do that.

```text
echo "package cardano-crypto-praos" >>  cabal.project.local
echo "  flags: -external-libsodium-vrf" >>  cabal.project.local
```

For some reason my SSH connection is often interrupted at this point, so in order to avoid this let's run TMUX and there we will start the installation. This will take a few hours

```text
tmux
cabal build all
```

let's copy the compiled bin \(executive\) files into the folder we created earlier: .local/bin

Now we need to shut down the server and take a snapshot of the machine

```text
sudo shutdown -h now
```

![](.gitbook/assets/image%20%2817%29.png)

```text
mkdir -p ~/.local/bin/
cp -p ~/git/cardano-node/dist-newstyle/build/x86_64-linux/ghc-8.10.4/cardano-cli-1.29.0/x/cardano-cli/build/cardano-cli/cardano-cli ~/.local/bin/
cp -p ~/git/cardano-node/dist-newstyle/build/x86_64-linux/ghc-8.10.4/cardano-node-1.29.0/x/cardano-node/build/cardano-node/cardano-node ~/.local/bin/
echo PATH="$PATH:$HOME/.local/bin/" >> $HOME/.bashrc
source ~/.bashrc
```

Check the installed version and location 

```text
cardano-node --version
cardano-cli --version
```

> cardano-node 1.29.0 - linux-x86\_64 - ghc-8.10 git rev 8fe46140a52810b6ca456be01d652ca08fe730bf

> cardano-cli 1.29.0 - linux-x86\_64 - ghc-8.10 git rev 8fe46140a52810b6ca456be01d652ca08fe730bf



