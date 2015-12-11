# nsupdate
PHP nsupdate script to update Dynamic DNS records.

To guide how to set and use refer to http://www.2dd.it/articoli/debian-create-your-own-dynamic-dns-server-provider/

Requirements:

1. Have a DNS server to serve your network. You can use this guide : http://www.2dd.it/articoli/debian-8-install-powerdns-server-with-sqlite-backend/
2. Fist you need to install a webserver with PHP support. To install it on your machine, follow this guide: http://www.2dd.it/articoli/debian-8-install-light-webserver-with-php-support/
3. Add the authentification support to the web server http://www.2dd.it/articoli/lighttpd-install-authentification-module/




Test the System


To test the system, open e browser webpage and type:

http://10.0.0.10/dns/nsupdate.php?ns=127.0.0.1&domain=test1.example.com&newip=10.0.0.13
to check if it is really changed in a terminal you can type:

dig test1.example.com @127.0.0.1

and if the output is something like this and the IP match , the Dynamic DNS is working properly!

dig test1.example.com @127.0.0.1
; <<>> DiG 9.9.5-9+deb8u3-Debian <<>> test1.example.com @127.0.0.1
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 38647
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1680
;; QUESTION SECTION:
;test1.example.com.              IN      A

;; ANSWER SECTION:
test1.example.com.       10      IN      A       10.0.0.13

;; Query time: 5 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Thu Dec 10 23:16:06 GMT 2015
;; MSG SIZE  rcvd: 61





Set the clients


You have different method to update the DNS server from the client. I will suggest some of them

Use the wget command like:

wget -O -   --no-http-keep-alive  --http-user="user" --http-passwd="password" "http://10.0.0.1/dns/nsupdate.php?ns=127.0.0.1&domain=test1.example.com&newip=10.0.0.13"
using with the bash functionaly like the hostname:

wget -O -   --no-http-keep-alive  --http-user="user" --http-passwd="password" "http://10.0.0.1/dns/nsupdate.php?ns=127.0.0.1&domain=$(hostname -f)"
use can use the previous command with crontab

sudo crontab -e 

\3 * * * *   wget -O -   --no-http-keep-alive  --http-user="user" --http-passwd="password" "http://10.0.0.1/dns/nsupdate.php?ns=127.0.0.1&domain=$(hostname -f)" >> /tmp/dnsupdate_$(hostname -f).log

