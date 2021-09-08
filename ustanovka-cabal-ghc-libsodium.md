# Установка Cabal, GHC, Libsodium

Подключаемся к серверу с пользователем Cardano, root - больше не используем

![](.gitbook/assets/image%20%2819%29.png)

Чтобы успешно установить \(скомпилировать\) кардано, мы должны быть уверены, что у нас есть все необходимые обновления! Чтобы скачать и установить их, введите следующие команды:

```
sudo apt-get update -y
sudo apt-get upgrade -y
```

```bash
sudo apt-get install -y curl python3 build-essential pkg-config libffi-dev libgmp-dev libssl-dev libtinfo-dev systemd libsystemd-dev libsodium-dev zlib1g-dev yarn make g++ jq libncursesw5 libtool autoconf git tmux htop nload
export LD_LIBRARY_PATH="/usr/local/lib:$LD_LIBRARY_PATH"
```

## Установка  Cabal

Мы будем устанавливать Cardano с помощью Cabal. Мы используем рекомендованную версию 3.4.0.0 и установим ее в нашу локальную папку bin \(.local/bin\).

```bash
wget  https://downloads.haskell.org/~cabal/cabal-install-3.4.0.0/cabal-install-3.4.0.0-x86_64-ubuntu-16.04.tar.xz
tar -xf cabal-install-3.4.0.0-x86_64-ubuntu-16.04.tar.xz
rm cabal-install-3.4.0.0-x86_64-ubuntu-16.04.tar.xz
mkdir -p ~/.local/bin
mv cabal ~/.local/bin/
```

Теперь у вас должна быть установлена программа cabal в папке ~/.local/bin/, теперь мы просто должны убедиться, что система также может найти файл cabal bin\(executive\). Давайте добавим эту информацию в наш файл пользовательского профиля \(.bashrc\), перезагрузим его \(используя команду исходного кода\), и проверим, видим ли мы в списке нашу папку ~/.local/bin/.

```bash
echo "export PATH=~/.local/bin:$PATH" >> ~/.bashrc 
source ~/.bashrc 
echo $PATH
```

Теперь давайте проверим, установлена ли последняя версия пакета Cabal.

```bash
cabal update
cabal --version
```

вы должны увидеть что-то вроде этого:

> **cardano@localhost**:**~**$  cabal update  
> cabal --version  
> Config file path source is default config file.  
> Config file /home/cardano/.cabal/config not found.  
> Writing default configuration to /home/cardano/.cabal/config  
> Downloading the latest package list from hackage.haskell.org

> **cabal-install version 3.4.0.0  
> compiled using version 3.4.0.0 of the Cabal library**

## Установка  GHC

Перейдем к следующим шагам - установка GHC - компилятора кода Haskell \(Cardano, написан на Haskell \)

```bash
wget https://downloads.haskell.org/ghc/8.10.4/ghc-8.10.4-x86_64-deb9-linux.tar.xz
tar -xf ghc-8.10.4-x86_64-deb9-linux.tar.xz
rm ghc-8.10.4-x86_64-deb9-linux.tar.xz
cd ghc-8.10.4
./configure
sudo make install
```

давайте убедимся, что всё установлено правильно**:**

```bash
ghc --version
```

вы должны увидеть что-то вроде этого:

{% hint style="info" %}
> На этом этапе у меня произошел сбой, и после выполнения вышеуказанной команды все еще показывалась старая версия. Это было исправлено простым выходом из системы и входом в систему.
{% endhint %}

> cardano@localhost:~$ ghc --version Славная система компиляции Хаскеля из Глазго, версия 8.10.4.

## Установка  Libsodium

Создадим папку git, в которой будем компилировать исходный код библиотеки libsodium:

```bash
mkdir -p ~/git
cd ~/git
```

Скачаем и установим библиотеку libsodium \(нам нужна конкретная ветка библиотеки, поэтому следуйте указаниям руководства\).

```bash
git clone https://github.com/input-output-hk/libsodium
cd libsodium
git checkout 66f017f1
./autogen.sh
./configure
make
sudo make install
```

Давайте добавим следующий путь к нашему файлу .bashrc

```bash
echo "export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH" >> ~/.bashrc
echo "export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH"     >> ~/.bashrc
source ~/.bashrc
```

