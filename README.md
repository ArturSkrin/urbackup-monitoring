# 🔍 Backup Monitoring Stack

- **Prometheus** for metrics collection
- **Grafana** for dashboards
- **urbackup-exporter** for exposing UrBackup stats
- (Optional) **node_exporter** for system metrics

Setup Firewall

By default, all services (Grafana, Prometheus, exporters) are publicly exposed if deployed on a VPS with a public IP. Add these rules to restrict access:

sudo iptables -A DOCKER-USER ! -s <your_ip> -p tcp --dport 55414 -j DROP
sudo iptables -A DOCKER-USER ! -s <your_ip> -p tcp --dport 9554 -j DROP
sudo iptables -A DOCKER-USER ! -s <your_ip> -p tcp --dport 9100 -j DROP
sudo iptables -A DOCKER-USER ! -s <your_ip> -p tcp --dport 9090 -j DROP
sudo iptables -A DOCKER-USER ! -s <your_ip> -p tcp --dport 3000 -j DROP

Configuration

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