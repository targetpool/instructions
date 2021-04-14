# Installieren eines Ubuntu-Servers in der Cloud

Für dieses Tutorial werde ich "Digital Ocean" verwenden, um unsere Linux-Server zu hosten. Legen wir ein neues Projekt in "D.O." an.

![](../.gitbook/assets/image%20%283%29.png)

Wählen wir unsere Ubuntu-Version und den Datentarif aus. Auf billigeren Versionen lässt sich Cardano nicht installieren \(ich habe es überprüft :\) \)

![](../.gitbook/assets/image%20%284%29.png)

    
  
Wählen Sie das Rechenzentrum, in dem sich Ihre Server befinden werden

![](../.gitbook/assets/server%20%281%29.jpg)

Als nächstes müssen Sie ein Login mit einem SSH-Schlüssel erstellen, verwenden Sie keine Passwörter. Sie können PUTTY verwenden, um den SSH-Schlüssel zu erzeugen. [https://the.earth.li/~sgtatham/putty/latest/w64/puttygen.exe](https://the.earth.li/~sgtatham/putty/latest/w64/puttygen.exe)

Öffnen Sie PUTTY KEY GEN, drücken Sie die Taste Generate und bewegen Sie die Maus, um den Schlüssel zu erzeugen.

![](../.gitbook/assets/image%20%2812%29.png)

Kopieren Sie den generierten Schlüssel und setzen Sie ein sicheres Passwort dafür.

![](../.gitbook/assets/image%20%287%29.png)

Klicken Sie auf die Schaltfläche Neuer SSH-Schlüssel und kopieren Sie den von Ihnen erzeugten Schlüssel in Putty

![](../.gitbook/assets/image%20%2813%29.png)

Benennen Sie den Server Relay1 Klicken Sie auf Droplet generieren

![](../.gitbook/assets/image%20%2814%29.png)

um eine Verbindung zu Ihrem neuen Server herzustellen, verwenden wir den PUTTY-Client [https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe](https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe) Geben Sie den Pfad zu Ihrem Schlüssel an

![](../.gitbook/assets/image%20%2818%29.png)

Verbinden Sie sich über Putty mit der öffentlichen IP und geben Sie das SSH-Passwort ein

![](../.gitbook/assets/image%20%2820%29.png)

Anlegen eines Cardano-Benutzers

```text
sudo adduser cardano
```

Geben Sie dem Benutzer Administratorrechte

```text
sudo usermod -aG sudo cardano
```

SSH-Schlüssel für neuen Benutzer verbinden Erstellen Sie das Verzeichnis, in dem der Schlüssel gespeichert werden soll

```text
mkdir /home/example_user/.ssh
```

Erstellen Sie eine Datei und kopieren Sie unseren SSH-Schlüssel dorthin, speichern und beenden Sie

```text
mkdir /home/example_user/.ssh
```

Erlauben Sie einem Benutzer, den SSH-Schlüssel zu verwenden

```text
chown -R cardano:cardano /home/cardano/.ssh
chmod 600 /home/cardano/.ssh/authorized_keys
```

Erstellen einer 10G-SWAP datai

```text
sudo fallocate -l 10G /swapfile
```

Erteilen von Auslagerungsdateiberechtigungen

```text
sudo chmod 600 /swapfile
```

SWAP-Datei markieren und aktivieren

```text
sudo mkswap /swapfile
sudo swapon /swapfile

```



```text
sudo swapon /swapfile
```

SWAP-Datei dauerhaft machen

```text
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

