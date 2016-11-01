---
layout: post
title: Installasi web server NGINX pada raspberry pi
description: "NGINX adalah sebuah sebuah layanan service server berbasis open source"
tags: [linux, raspberry pi, server, web server]
---

## Installasi web server pada raspberry pi

### Apa itu NGINX (Engine X)?

NGINX adalah sebuah service `HTTP` atau `PROXY` dengan kode sumber terbuka yang bisa juga berfungsi sebagai proxy `IMAP/POP3`.

### Install paket yang diperlukan

download paket pendukung dari `NGINX`

{% highlight bash %}
$ sudo apt-get install nginx
{% endhighlight %}

{% highlight bash %}
$ sudo apt-get install php5-fpm
{% endhighlight %}


### Configurasi awal pada NGINX

setelah selesai melakukan penginstallan paket, sekarang lakukan configurasi awal pada `/etc/nginx/site-available/default` :

{% highlight bash %}
$ sudo nano /etc/nginx/site-available/default
{% endhighlight %}

`uncomment` baris dibawah ini:

{% highlight bash %}
port 80
location /doc/ 
deny all;
location ~ \.php$ { 
fastcgi_split_path_info
fastcgi_pass unix /var/run/php5-fpm-shock; 
include fastcgi_params;
{% endhighlight %}


### Configurasi awal pada php

lakukan configurasi pada `php5-fpm`  `nano /etc/php5/fpm/php.ini` 

{% highlight bash %}
$ sudo ifconfig eth0 192.168.1.1/24 up
{% endhighlight %}

cari file `cgi.fix_pathinfo=1` dan ubah menjadi `cgi.fix_pathinfo=0`

{% highlight bash %}
$ sudo nano /etc/php5/fpm/php.ini
{% endhighlight %}

Restart NGINX dan php dengan perintah:

{% highlight bash %}
$ sudo /etc/init.d/nginx restart
$ sudo /etc/init.d/php5-fpm restart
{% endhighlight %}

### Percobaan pada browser

buka browser ketik `localhost` atau `IP anda`
Selesai, semoga bermamfaat.
