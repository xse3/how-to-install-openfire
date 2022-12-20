## Require
1. `sudo apt-get install -y mysql-server`
2. `sudo apt install default-jre`

```
java -version
mysql -u root -p
create database openfire;
```

$ sudo apt-get update
$ sudo apt-get install -y mysql-server

2. Buat Database untuk Jabber/Openfire

$ mysql -u root -p
$ create database openfire; (ganti openfire_db sesuai keinginan)

#Lihat Database yang sudah di buat

$ show databases;

#Keluar dari MySQL

exit

3. Instal Openfire di  VPS (Ubuntu 16.04 x64)

- Buka VPS dengan akses root

$ sudo apt-get update
$ java -version
$ sudo apt-get install default-jre
$ wget -O openfire.deb http://www.igniterealtime.org/downloadServlet?filename=openfire/openfire_4.7.4_all.deb (4.5.2 sesuikan dengan versi terbaru)
$ sudo dpkg --install openfire.deb

4. Konfigurasi Openfire

- Buka ipvps:9090
- Welcome to Setup - English (en) - Continue
- XMPP Domain Name = Isi dengan domain yang sudah di kaitkan dengan VPS (lainnya biarkan sama) Continue
- Standard Database Connection
# Database Driver Presets = MySQL
# JDBC Driver Class = com.mysql.jdbc.Driver
# Database URL = jdbc:mysql://127.0.0.1/openfire?useUnicode=true&characterEncoding=UTF-8&characterSetResults=UTF-8
# Username = Root
# Password = Pass Database MySQL
# Connection Timeout = 365
- Continue
- Default - Continue
- Administrator Account
# Admin Email Address = admin@namadomain.id
# New Password - Confirm Password = Suka-Suka
- Continue
Setup Complete!

# allow port
ufw allow 5222
ufw allow 5223
ufw allow 5229
ufw allow 5262
ufw allow 5263
ufw allow 5269
ufw allow 5270
ufw allow 5275
ufw allow 5276

ufw allow 7070
ufw allow 7443
ufw allow 7777

ufw allow 9091
ufw allow 9090

5222,5223,5229,5262,5263,5269,5270,5275,5276,7070,7443,7777,9090,9091

5. Konfigurasi Keamanan Opendire

Instal Iptables

$ sudo apt-get install iptables-persistent -y

- Mengalihkan Port 9090
$ sudo iptables -t nat -A PREROUTING -p tcp --dport 9090 -j REDIRECT --to-ports 9514

- Simpan perubahan
$ sudo invoke-rc.d netfilter-persistent save
$ sudo iptables-save >/etc/iptables/rules.v4
$ sudo ip6tables-save >/etc/iptables/rules.v6
$ sudo reboot

- Lihat Konfigurasi Iptables
$ iptables -L INPUT -n --line-numbers
$ iptables -L PREROUTING -n --line-numbers

- Lihat Port yang Open
$ netstat -nlpt

- Blok Port
$ iptables -A INPUT -p tcp --dport 9091 -j DROP

- Open Port
$ iptables -A INPUT -p tcp --dport 77 -j ACCEPT
$ iptables -A INPUT -p tcp --dport 2005 -j ACCEPT

-Lihat rule iptables nat
$ iptables -t nat --line-numbers -L INPUT
$ iptables -t nat --line-numbers -L PREROUTING && iptables -L INPUT -n --line-numbers

-Hapus rule iptables nat
$ iptables -t nat -D PREROUTING 6
$ iptables -D INPUT 6
