# Fail2Ban

```text
sudo apt-get install fail2ban
```

```text
sudo nano /etc/ssh/sshd_config
```

![](../.gitbook/assets/image%20%28107%29.png)

```text
sudo ufw allow proto tcp from any to any port 1234
```

```text
sudo ufw enable
```

```text
sudo systemctl restart sshd
```

```text
 sudo nano /etc/fail2ban/jail.conf
```

![](../.gitbook/assets/image%20%28106%29.png)

```text
sudo systemctl restart fail2ban
```



