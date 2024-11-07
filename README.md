# Set up a DNS server on Linux

To make a working DNS resolver based on Linux first install bind9 with a command `sudo apt install bind9`. 

<p align="center">
<img src="https://github.com/user-attachments/assets/dfc225fe-a4d8-4685-90e7-ec8f9a14cc6d">
</p>

Once installed you will need to modify bind9 config files specifically for your purposes starting with a named.conf file which in my case located at the path `/etc/bind/named.conf`. First make sure your have two conditions `allow-query { any; };` and `listen-on-v6 { any; };` set as that.

<p align="center">
<img src="https://github.com/user-attachments/assets/c1e89df0-c213-43f9-aa21-fe32ac38a694">
</p>

Make changes to the file named.conf.default-zones in the same directory adding following: 
```
zone "example.com" {
  type master;
  file "etc/bind/db.example.com";
};

zone "myzone.com" {
  type master;
  file "etc/bind/db.myzone.com";
};
```

Your zones will contain all information about resolved domains. Type "master" or primary type means it will be renewed every time after a dns query was sent and it will be fully operating, recording and making resets in accordance to user configurations or default settings in difference to secondary zones which are widely a copies of primary zones.

<p align="center">
<img src="https://github.com/user-attachments/assets/cc635945-92e9-471a-80a1-e5d139267a4b">
</p>

Create a file for every zone you assigned. Use a following code:
```
$TTL 3600
@  IN  SOA  ns1.*zone domain name*  hostmaster.*zone domain name*  (
      2024042101
      900
      1800
      604800
      5400
      )
    IN  NS  ns1.*zone domain name*
    IN  NS  ns2.*zone domain name*
$ORIGIN *zone domain name*
ns1  IN  A  *your IP address*
ns2  IN  A  *your IP address*
```


<p align="center">
<img src="https://github.com/user-attachments/assets/cce59120-555e-4074-8657-731f1ce3c9d6">
<img src="https://github.com/user-attachments/assets/34290e2b-2815-49b1-a87f-b7628e964700">
</p>

You can check your configurations files for any errors made in those. Use a command <br>
`sudo named-checkconf /etc/bind/named.conf` to check named.conf file and <br>
`sudo named-checkzone *path to your file*` to check any of your zone files. <br>
If no errors detected there will be no visible result printed or it will simply respond "OK".
<br>

As you done making your changes in configurations restart bind9 by running `systemctl restart bind9` and check its status with `systemctl status bind9`. 

<p align="center">
<img src="https://github.com/user-attachments/assets/760b5385-d72f-4185-82a6-be001caf1acd">
</p>

When started you can check its work by sending dns queries handly through CLI using `dig *domain name*` command. 

<p align="center">
<img src="https://github.com/user-attachments/assets/0938f1ca-04a1-4c7b-8576-e6e653a6d3db">
<img src="https://github.com/user-attachments/assets/86ebfefb-2cbb-4ddc-aa98-4f21a3147607">
</p>

First time when send DNS query with google.com IP request my PC receives a respond in 1105 ms

<p align="center">
<img src="https://github.com/user-attachments/assets/4b9d9a0c-623b-413e-addd-8b956458389e">
</p>

Next time this query takes 0 ms to respond as it is saved in bind9 cache. 

<p align="center">
<img src="https://github.com/user-attachments/assets/5d9a2665-0e27-4272-befa-3b782e6bf62e">
</p>

To make your router use your new DNS resolver, put its IP address into router's DNS server configurations. Any single device could be setted to use your DNS server as well by its settings. You may also try to edit forwarders in named.conf file and test your query latency time. In my case default settings works the fastest so I left it unchanged. 

## References:
[build your own DNS server on Linux](https://opensource.com/article/17/4/build-your-own-name-server)
[bind9 official documentation](https://bind9.readthedocs.io/en/v9.18.31/)
