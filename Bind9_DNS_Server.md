## Setting up a Bind9 DNS server on a GNU/Linux VM with a test.lan zone

### Prerequisites

* A GNU/Linux VM (Debian or Ubuntu recommended) with a network card configured as "Internal Network".
* Root or sudo access on the VM.

### Procedure

1. **Install Bind9**

```bash
sudo apt update
sudo apt install bind9 bind9utils
```

2. **Configure the `test.lan` zone**

* Create the zone file:

```bash
sudo nano /etc/bind/db.test.lan
```

* Add the following content, replacing the IP addresses with the ones you want:

> ; Zone file for test.lan
> 
> $TTL    604800
> 
> @       IN      SOA     ns.test.lan. admin.test.lan. (
> 
>                     2023090601      ; Serial
> 
>                     604800          ; Refresh
> 
>                     86400           ; Retry
> 
>                     2419200         ; Expire
> 
>                     604800 )        ; Negative Cache TTL
> 
> ; Name records
> 
> @       IN      NS      ns.test.lan.
> 
> ns       IN      A       192.168.56.10   ; IP address of your DNS server
> 
> www      IN      A       192.168.56.11   ; IP address of the web server
> 
> ftp      IN      CNAME   www.test.lan. ; Alias for the web server


3. **Declare the zone in `named.conf.local`**

* Open the configuration file:

```bash
sudo nano /etc/bind/named.conf.local
```

* Add the following lines:

> zone "test.lan" {
> 
>      type master;
> 
>      file "/etc/bind/db.test.lan";
> 
> };

4. **Restart the Bind9 service**

```bash
sudo systemctl restart bind9
```

5. **Tests from the server**

* Check the syntax of the zone file:

```bash
named-checkzone test.lan /etc/bind/db.test.lan
```

The result should be `OK`.

* Test name resolution:

```bash
dig @localhost www.test.lan
dig @localhost ftp.test.lan
```

You should get the corresponding IP addresses.

6. **Tests from a client**

* Configure the IP address of your DNS server (192.168.56.10 in this example) in the network settings of the client.
* Repeat the `dig` commands from the client.

