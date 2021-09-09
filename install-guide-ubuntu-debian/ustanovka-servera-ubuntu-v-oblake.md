# Ubuntu server cloud installation

For tis tutorial I will be using Digital Ocean as our server hosting service

![](../.gitbook/assets/image%20%283%29.png)

Lets pick our Ubuntu version and a payment plan. On cheaper payment plans your node will not run, I have tried. \)\)\) Unfortunately after the upgrade to 1.26.1 the 20$ option is not working either. You now need 8 gigs of RAM

![](../.gitbook/assets/image%20%284%29.png)

    
  
Here you can pick a datacenter where your servers will be hosted.

![](../.gitbook/assets/server%20%281%29.jpg)

Next we will need to connect to our servers, you dont want to be using passwords since they are not secure. For all of our servers we will be using SSH keys. In order to generate SSH keys you will have to use putty key gen. [https://the.earth.li/~sgtatham/putty/latest/w64/puttygen.exe](https://the.earth.li/~sgtatham/putty/latest/w64/puttygen.exe)

Run putty keygen on your machine and move your mouse randomly till your SSH key is generated.

![](../.gitbook/assets/image%20%2812%29.png)

Copy your generated key and set up a secure password for it.

![](../.gitbook/assets/image%20%287%29.png)

Now press New SSH key and copy the key that you have just generated un keygen generator

![](../.gitbook/assets/image%20%2813%29.png)

Now you want to name your server for example relay1 and press Generate Droplet

![](../.gitbook/assets/image%20%2814%29.png)

to connect to your new server we will be using PUTTY client

[https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe](https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe)

Here you want to specify the location where you have stored your SSH key

![](../.gitbook/assets/image%20%2818%29.png)

Specify your public IPin PUTTY and eneter the password from your SSH key

![](../.gitbook/assets/image%20%2820%29.png)

now we want to create a new user Cardano

```text
sudo adduser cardano
```

Adding user `cardano' ... Adding new group`cardano' \(1000\) ... Adding new user `cardano' (1000) with group`cardano' ... Creating home directory `/home/cardano' ... Copying files from`/etc/skel' ... New password: Retype new password: passwd: password updated successfully Changing the user information for cardano Enter the new value, or press ENTER for the default Full Name \[\]: cardano Room Number \[\]: Work Phone \[\]: Home Phone \[\]: Other \[\]: Is the information correct? \[Y/n\] y  
  
You want to give your Cardano user admin rights

```text
sudo usermod -aG sudo cardano
```

Now we are giving a new user the SSH key that we have created

Now we want to create a directory where your SSH key will be stored

```text
mkdir /home/cardano/.ssh
```

Now we want to create a file where we will copy the SSH key. Copy the contents of the user’s public key into this is a plain text file where you can paste one public key per line.

```text
nano /home/cardano/.ssh/authorized_keys
```

lets give user rights to use the SSH key

```text
chown -R cardano:cardano /home/cardano/.ssh
chmod 600 /home/cardano/.ssh/authorized_keys
```

Next we will need to create a SWAP file 10G

```text
sudo fallocate -l 10G /swapfile
```

Now you are giving rights to the SWAP file

```text
sudo chmod 600 /swapfile
```

And you mark SWAP file as active

```text
sudo mkswap /swapfile
sudo swapon /swapfile
```

Now you want to make the swap file permanent

```text
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

