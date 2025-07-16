# 🐧 Ubuntu Server Setup Guide: PostgreSQL, RabbitMQ, and Redis

This guide walks you through installing and running **PostgreSQL**, **RabbitMQ**, and **Redis** servers on an Ubuntu (20.04, 22.04, or 24.04) system. It also covers running **pgAdmin** as a local web dashboard for PostgreSQL.

---

## ✅ Prerequisites

Run the following to ensure your system has required tools:

```bash
sudo apt update && sudo apt install curl gnupg apt-transport-https -y
```

---

## 🐘 PostgreSQL Setup

### 1. Install PostgreSQL

```bash
sudo apt install -y postgresql postgresql-contrib
```

### 2. Enable & Start PostgreSQL Service

```bash
sudo systemctl enable postgresql
sudo systemctl start postgresql
```

### 3. Check PostgreSQL Service Status

```bash
sudo systemctl status postgresql
```

### 4. Check Listening Port

```bash
sudo ss -tunlp | grep postgres
```

> Default PostgreSQL port is **5432**.

---

## 🌐 pgAdmin Web Dashboard

### 1. Run pgAdmin Manually (No Apache Required)

If already installed under `/usr/pgadmin4`:

```bash
cd /usr/pgadmin4
source venv/bin/activate
python3 -u web/pgAdmin4.py
```

Access: [http://localhost:5050](http://localhost:5050)

> For persistent local data/config, add a `config_local.py` file.

---

## 🐇 RabbitMQ Setup

### 1. Add RabbitMQ Repositories and Keys

```bash
curl -1sLf "https://keys.openpgp.org/vks/v1/by-fingerprint/0A9AF2115F4687BD29803A206B73A36E6026DFCA" | \
  sudo gpg --dearmor | sudo tee /usr/share/keyrings/com.rabbitmq.team.gpg > /dev/null

sudo tee /etc/apt/sources.list.d/rabbitmq.list <<EOF
## Modern Erlang/OTP releases
deb [arch=amd64 signed-by=/usr/share/keyrings/com.rabbitmq.team.gpg] https://deb1.rabbitmq.com/rabbitmq-erlang/ubuntu noble main
deb [arch=amd64 signed-by=/usr/share/keyrings/com.rabbitmq.team.gpg] https://deb2.rabbitmq.com/rabbitmq-erlang/ubuntu noble main

## Latest RabbitMQ releases
deb [arch=amd64 signed-by=/usr/share/keyrings/com.rabbitmq.team.gpg] https://deb1.rabbitmq.com/rabbitmq-server/ubuntu noble main
deb [arch=amd64 signed-by=/usr/share/keyrings/com.rabbitmq.team.gpg] https://deb2.rabbitmq.com/rabbitmq-server/ubuntu noble main
EOF

sudo apt update
```

### 2. Install Erlang & RabbitMQ

```bash
sudo apt install -y erlang-base \
    erlang-asn1 erlang-crypto erlang-eldap erlang-ftp erlang-inets \
    erlang-mnesia erlang-os-mon erlang-parsetools erlang-public-key \
    erlang-runtime-tools erlang-snmp erlang-ssl erlang-syntax-tools \
    erlang-tftp erlang-tools erlang-xmerl

sudo apt install rabbitmq-server -y --fix-missing
```

### 3. Enable & Start RabbitMQ

```bash
sudo systemctl enable rabbitmq-server
sudo systemctl start rabbitmq-server
```

### 4. Enable Management UI

```bash
sudo rabbitmq-plugins enable rabbitmq_management
```

Access: [http://localhost:15672](http://localhost:15672)

> Create user if needed:

```bash
sudo rabbitmqctl add_user admin admin123
sudo rabbitmqctl set_user_tags admin administrator
sudo rabbitmqctl set_permissions -p / admin ".*" ".*" ".*"
```

### 5. Check RabbitMQ Ports

```bash
sudo ss -tunlp | grep 5672    # AMQP port
sudo ss -tunlp | grep 15672   # Web UI port
```

---

## 🔁 Redis Setup

### 1. Install Redis Server

```bash
sudo apt install redis-server -y
```

### 2. Enable & Start Redis

```bash
sudo systemctl enable redis-server
sudo systemctl start redis-server
```

### 3. Check Redis Status

```bash
sudo systemctl status redis-server
```

### 4. Check Redis Port

```bash
sudo ss -tunlp | grep redis
```

> Default Redis port is **6379**.

---

## ✅ Summary Table

| Service    | Default Port | Dashboard / CLI                                  | Start Command                          |
| ---------- | ------------ | ------------------------------------------------ | -------------------------------------- |
| PostgreSQL | 5432         | pgAdmin / psql                                   | `sudo systemctl start postgresql`      |
| RabbitMQ   | 5672 (AMQP)  | [http://localhost:15672](http://localhost:15672) | `sudo systemctl start rabbitmq-server` |
| Redis      | 6379         | redis-cli                                        | `sudo systemctl start redis-server`    |

---

Let me know if you'd like to make this portable as a shell script, or configure Docker alternatives.
