# 🔍 Backup Monitoring Stack

- **Prometheus** for metrics collection
- **Grafana** for dashboards
- **urbackup-exporter** for exposing UrBackup stats
- **node_exporter** for system metrics (Free space on server is essential to monitor as a key metric)

## Setup Firewall
### By default all services (Grafana, Prometheus, exporters) are publicly exposed if deployed on a VPS with a public IP.

```bash
sudo iptables -A DOCKER-USER ! -s <your_ip> -p tcp --dport 55414 -j DROP
sudo iptables -A DOCKER-USER ! -s <your_ip> -p tcp --dport 9554 -j DROP
sudo iptables -A DOCKER-USER ! -s <your_ip> -p tcp --dport 9090 -j DROP
sudo iptables -A DOCKER-USER ! -s <your_ip> -p tcp --dport 3000 -j DROP
```

## We need to Allow Prometheus connect to node_exporter which is working in "host" network mode
```bash
sudo itpables -A INPUT -p tcp --dport 9100 -s 172.18.0.0/16 -j ACCEPT -m comment --comment Allow_docker_containers_to_node_exporter
```

## Configuration

All configuration is done with environment variables.

- `URBACKUP_SERVER_URL`: UrBackup server URL including host, port and API endpoint. Example: `http://urbackup-server:55414/x`
- `URBACKUP_SERVER_USERNAME`: (Optional) Username to login in the server. Only required if authorization is enabled. The default is `admin`.
- `URBACKUP_SERVER_PASSWORD`: (Optional) Password to login in the server. Only required if authorization is enabled. The default is `1234`.
- `EXPORT_CLIENT_BACKUPS`: (Optional) Export detailed metrics for each client. This option can generate a lot of metrics if there many configured clients. The default is `true`.
- `LISTEN_PORT`: (Optional) The address the exporter should listen on. The default is `9554`.
- `LISTEN_ADDRESS`: (Optional) The address the exporter should listen on. The default is
   to listen on all addresses.
- `LOG_LEVEL`: (Optional) Log level of the traces. The default is `INFO`.

## Grafana dashboard

There is a reference Grafana dashboard in [grafana/grafana_dashboard.json](./grafana/grafana_dashboard.json).

## Credits

This project is based on [ngosang/urbackup-exporter](https://github.com/ngosang/urbackup-exporter), licensed under the MIT License.