# 🚀 Zabbix 7.2 Installation Guide (Ubuntu 24.04)

This guide provides step-by-step instructions to install and configure **Zabbix 7.2** on **Ubuntu 24.04**.

---


## 📅 1. Install Zabbix Repository

```bash
wget https://repo.zabbix.com/zabbix/7.2/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.2+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_7.2+ubuntu24.04_all.deb
apt update
```

---

## ⚙️ 2. Install Zabbix Server, Frontend, and Agent

```bash
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent
```

---

## 🗄️ 3. Create Initial Database

Make sure your database server is running. Then run the following on your **database host**:

```bash
mysql -uroot -p
```

Inside MySQL:

```sql
create database zabbix character set utf8mb4 collate utf8mb4_bin;
create user zabbix@localhost identified by 'password';
grant all privileges on zabbix.* to zabbix@localhost;
set global log_bin_trust_function_creators = 1;
quit;
```

Then, import the initial schema and data on your **Zabbix server**:

```bash
zcat /usr/share/zabbix/sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
```

Finally, disable the `log_bin_trust_function_creators` option:

```bash
mysql -uroot -p
```

Inside MySQL:

```sql
set global log_bin_trust_function_creators = 0;
quit;
```

---

## 💡 4. Configure Zabbix Server

Edit the Zabbix server config file:

```bash
nano /etc/zabbix/zabbix_server.conf
```

Set the following line with your database password:

```
DBPassword=password
```

---

## 🔄 5. Start Zabbix Services

Start and enable Zabbix server, agent, and Apache:

```bash
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
```

---

## 🌐 6. Access Zabbix Web Interface

Open your browser and go to:

```
http://<your-server-ip>/zabbix
```

---

## ✅ Done!

Zabbix is now ready for configuration via the web UI. Enjoy monitoring! 🎉

---

> Created by [Mohammad Talebi](https://linkedin.com/in/mtlbd) – DevOps Engineer 👨‍💻

