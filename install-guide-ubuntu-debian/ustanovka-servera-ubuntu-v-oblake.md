# Установка сервера Ubuntu в облаке

Для данного руководства я буду использовать "Digital Ocean" для хостинга наших Linux-серверов.

Создадим новый проект в "D.O."

![](../.gitbook/assets/image%20%283%29.png)

Давайте выберем нашу версию Ubuntu и тарифный план. На более дешевых версиях Cardano не устанавливается \(я проверил :\) \).

![](../.gitbook/assets/image%20%284%29.png)

    
  
Выберете  датацентр. где будут распологаться ваши серверы

![](../.gitbook/assets/server%20%281%29.jpg)

Далее вы хотите создать вход в систему, используя SSH ключ, не используйте пароли. Для генерации SSH-ключа можно использовать PUTTY. [https://the.earth.li/~sgtatham/putty/latest/w64/puttygen.exe](https://the.earth.li/~sgtatham/putty/latest/w64/puttygen.exe)

Открываем PUTTY KEY GEN нажимаем на кнопку Generate  т двигаем мышкой пока не сгенерится ключ.

![](../.gitbook/assets/image%20%2812%29.png)

 Копируем сгенерированый ключ и устанавливаем для него надёжный пароль.

![](../.gitbook/assets/image%20%287%29.png)

#### Нажимаем на кнопку  New SSH key и копируем ключ который сгенерировали в Putty

![](../.gitbook/assets/image%20%2813%29.png)

#### Нажимаем  Generate Droplet

![](../.gitbook/assets/image%20%2814%29.png)

для подключения к вашему новому серверу мы будем использовать PUTTY Client

[https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe](https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe)

