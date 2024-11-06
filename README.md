# Set up a DNS server on Linux

To make a working DNS resolver based on Linux first install bind9 with a command `sudo apt install bind9`. 

<p align="center">
<img src="https://github.com/user-attachments/assets/dfc225fe-a4d8-4685-90e7-ec8f9a14cc6d">
</p>

Once installed you will need to modify bind9 config files specifically for your purposes starting with a named.conf file which in my case located at the path `/etc/bind/named.conf`. First make sure your have two conditions `allow-query { any; };` and `listen-on-v6 { any; };` set as that.

<p align="center">
<img src="https://github.com/user-attachments/assets/c1e89df0-c213-43f9-aa21-fe32ac38a694">
</p>
