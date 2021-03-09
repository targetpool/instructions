# Установка сервера Ubuntu в облаке

Для данного руководства я буду использовать "Digital Ocean" для хостинга наших Linux-серверов.

Создадим новый проект в "D.O."

![](../.gitbook/assets/image%20%283%29.png)

Давайте выберем нашу версию Ubuntu и тарифный план. На более дешевых версиях Cardano не устанавливается \(я проверил :\) \).

![](../.gitbook/assets/image%20%284%29.png)

    
  
Выберете  датацентр. где будут распологаться ваши серверы

![](../.gitbook/assets/server%20%281%29.jpg)

Далее нужно создать вход в систему, используя SSH ключ, не используйте пароли. Для генерации SSH-ключа можно использовать PUTTY. [https://the.earth.li/~sgtatham/putty/latest/w64/puttygen.exe](https://the.earth.li/~sgtatham/putty/latest/w64/puttygen.exe)

Открываем PUTTY KEY GEN нажимаем на кнопку Generate  и двигаем мышкой пока не сгенерится ключ.

![](../.gitbook/assets/image%20%2812%29.png)

 Копируем сгенерированый ключ и устанавливаем для него надёжный пароль.

![](../.gitbook/assets/image%20%287%29.png)

Нажимаем на кнопку  New SSH key и копируем ключ который сгенерировали в Putty

![](../.gitbook/assets/image%20%2813%29.png)

Даем имя серверу Relay1 Нажимаем  Generate Droplet

![](../.gitbook/assets/image%20%2814%29.png)

для подключения к вашему новому серверу мы будем использовать PUTTY Client

[https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe](https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe)

Указываем путь к вашему ключу

![](../.gitbook/assets/image%20%2818%29.png)

Подключаемся к Public IP через Putty и вводим пароль от SSH

![](../.gitbook/assets/image%20%2820%29.png)

Создаем пользователя Cardano

```text
sudo adduser cardano
```

Даем пользователю админ права

```text
sudo usermod -aG sudo cardano
```

Подключаем SSH Ключи для нового пользователя

Создаем директорию где будет храниться ключ 

```text
mkdir /home/example_user/.ssh
```

Создаем файл и копируем туда наш SSH ключ, сохраняем и выходим

```text
mkdir /home/example_user/.ssh
```

Даем пользователю права на использование SSH ключа

```text
chown -R cardano:cardano /home/cardano/.ssh
chmod 600 /home/cardano/.ssh/authorized_keys
```

Создаем swap file 10G

```text
sudo fallocate -l 10G /swapfile
```

Даем права на swap file 

```text
sudo chmod 600 /swapfile
```

Маркиркируем и активируем SWAP file

```text
sudo mkswap /swapfile
sudo swapon /swapfile

```

Делаем бэкап

```text
sudo swapon /swapfile
```

Делаем swap file постоянным

```text
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

