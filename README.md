# Monitoring-setup-for-PostgreSQL-MySQL-and-System-Metrics-using-Prometheus-for-metrics-with-Grafana

**1. For this monitoring setup below the ports should be opened.**

- Node Exporter (9100)
- PostgreSQL Exporter (9187)
- MySQL Exporter (9104)
- Prometheus (9090)
- Grafana (3000)

**2. Install the Prometheus, Mysql, Postgres and Grafana**

![image](./Screenshot/7.png)

**3. Once Installed the prometheus then install the exporter for Node, Mysql and Postgres. Then create a system service file at this path /etc/systemd/system/**

- node_exporter.service
- postgres_exporter.service
- mysql_exporter.service

![image](./Screenshot/1.png)

**4. Then inside the prometheus.yml add the above exporters with their port.**

![image](./Screenshot/8.png)

**5. Once configured the services file we need to restart the services using below commands.**

      sudo systemctl daemon-reexec
      sudo systemctl daemon-reload
      sudo systemctl enable prometheus
      sudo systemctl start prometheus

![image](./Screenshot/2.png)

**6. For postgres exporter, we need to create a DB User**

      CREATE USER exporter WITH PASSWORD 'exporter123';
      GRANT CONNECT ON DATABASE postgres TO exporter;
      GRANT pg_monitor TO exporter;

Then create a environment file

      sudo nano /etc/default/postgres_exporter
      
      DATA_SOURCE_NAME="postgresql://exporter:exporter123@localhost:5432/postgres?sslmode=disable"

**7. For Mysql exporter, we need to create a DB user**

      CREATE USER 'mysqlExporter'@'localhost' IDENTIFIED BY 'MysqlEx123';
      GRANT PROCESS, REPLICATION CLIENT, SELECT ON *.* TO 'mysqlExporter'@'localhost';
      FLUSH PRIVILEGES;


For mysql give the user credentials at this location
      
      sudo nano /etc/.mysqld_exporter.cnf

      user=mysqlExporter
      password=MysqlEx123

**8. Restart the all services at last once. Then install the Grafana**

      sudo apt update
      sudo apt install -y grafana
      sudo systemctl enable grafana-server
      sudo systemctl start grafana-server

**9. Let's connect Grafana to Prometheus. Under the data source choose the prometheus to add it**

**10. Finally, Add the Dashboards for Node, Mysql and Postgres**

- System Metrics
Dashboard ID: 1860 (Node Exporter Full)

- PostgreSQL Metrics
Dashboard ID: 9628

- MySQL Metrics
Dashboard ID: 7362

![image](./Screenshot/3.png)

![image](./Screenshot/4.png)

![image](./Screenshot/5.png)

![image](./Screenshot/6.png)
