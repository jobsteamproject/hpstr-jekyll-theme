---
layout: post
title: Installasi domain name system (DNS) pada raspberry pi
description: "Domain name system (DNS) adalah sistem penamaan domain yang mentranslasikan domain ke IP address dan sebaliknya"
tags: [linux, raspberry pi, DNS]
---

## Installasi dns pada raspberry pi

### Apa itu domain name system (DNS) ?

[Domain Name System][] adalah sistem penamaan domain yang mentranslasikan domain ke IP address dan sebaliknya.

### Install paket yang diperlukan
 
Sebelum melakukan penginstallan dns, download paket yang diperlukan terlebih dahulu:

{% highlight bash %}
$ sudo apt-get install bind9
{% endhighlight %}

### Pengaturan IP address

setelah selesai melakukan penginstallan paket, sekarang lakukan pengaturan alamat IP:

{% highlight bash %}
$ sudo nano /etc/network/interfaces
{% endhighlight %}

{% highlight bash %}
auto eth0
iface eth0 inet static
address 192.168.1.1
netmask 255.255.252.0
{% endhighlight %}

atau juga bisa dengan cara sebagai berikut:

{% highlight bash %}
$ sudo ifconfig eth0 192.168.1.1/24 up
{% endhighlight %}

### Pemasangan DNS

untuk melakukan pemasangan DNS, masuk ke dalam folder `/etc/bind`

{% highlight bash %}
$ cd /etc/bind/
{% endhighlight %}

setelah itu lakukan pengeditan pada `named.conf.local`

{% highlight bash %}
$ sudo nano named.conf.local
{% endhighlight %}

tambahkan konfigurasi dibawah ini pada `named.conf.local`

{% highlight bash %}
    zone "jobsteamproject.com"{
            type master;
            file "/etc/bind/db.jobsteam";
            };
    zone "192.in-addr.arpa"{
            type master;
            file "/etc/bind/db.192";
            };
{% endhighlight %}

lakukan penyalinan 'file forward' dan `file reverse` yang ada pada folder `/etc/bind/` dengan cara sebagai berikut:

#### file forward
{% highlight bash %}
$ sudo cp db.local db.jobsteamproject
{% endhighlight %}

#### file reverse
{% highlight bash %}
$ sudo cp db.127 db.192
{% endhighlight %}

selanjutnya edit file `forward` `db.jobsteamproject`

{% highlight bash %}
$ sudo nano db.jobsteamproject
{% endhighlight %}

sesuaikan dengan baris dibawah ini:

{% highlight bash %}

$TTL                    604800
@       IN              SOA             jobsteamproject.com. root.jobsteamproject.com. (
                        2               ; Serial
                        604800          ; Refresh
                        86400           ; Retry
                        2419200         ; Expire
                        604800 )        ; Negative Cache TTL

@        IN     NS      jobsteamproject.com.
@        IN     A       192.168.1.1   //--> ini merupakan ip raspberry kita
www      IN     A       192.168.1.1
    
{% endhighlight %}    

selanjutnya edit file `reverse` `db.192`

{% highlight bash %}

$TTL                    604800
@       IN              SOA             jobsteamproject.com. root.jobsteamproject.com. (
                        2               ; Serial
                        604800          ; Refresh
                        86400           ; Retry
                        2419200         ; Expire
                        604800 )        ; Negative Cache TTL

@        IN     NS      jobsteamproject.com.
1.1.168  IN     PTR     www.jobsteamproject.com.
    
{% endhighlight %}

kemudian edit file `resolv.conf`

{% highlight bash %}
$ nano /etc/resolv.conf
{% endhighlight %}

ubah seperti dibawah ini:

{% highlight bash %}
search jobsteamproject.com
nameserver 192.168.1.1
{% endhighlight %}

jika sudah konfigurasi semua, lakukan `restart` pada bind9

{% highlight bash %}
$ sudo /etc/init.d/bind9 restart
{% endhighlight %}

lakukan pengecekan DNS dengan `nslookup`

{% highlight bash %}
$ sudo nslookup jobsteamproject.com
$ sudo nslookup 192.168.1.1
{% endhighlight %}

Selesai, semoga bermamfaat.
