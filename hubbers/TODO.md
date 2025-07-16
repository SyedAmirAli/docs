### PostgresSQL

```bash
# Run the postgresql web dashboard
cd /usr/pgadmin4 && source venv/bin/activate && python3 -u web/pgAdmin4.py

# Install PostgreSQL and its contrib package
sudo apt install -y postgresql postgresql-contrib

# Enable and start the service
sudo systemctl enable postgresql
sudo systemctl start postgresql

# Check status
sudo systemctl status postgresql

# Check Running Port
sudo ss -tunlp | grep postgresql
```

### RabbitMQ

```bash
# Enable and start RabbitMQ
sudo systemctl enable rabbitmq-server
sudo systemctl start rabbitmq-server

# Check status
sudo systemctl status rabbitmq-server

# Optional: enable RabbitMQ management plugin (UI on port 15672)
sudo rabbitmq-plugins enable rabbitmq_management

# Check Running Port
sudo ss -tunlp | grep rabbitmq-server
sudo ss -tunlp | grep 15672
```

### Redis

```bash
# Enable and start RabbitMQ
sudo systemctl enable redis-server
sudo systemctl start redis-server

# Check status
sudo systemctl status redis-server

# Check Running Port
sudo ss -tunlp | grep redis-server
```
