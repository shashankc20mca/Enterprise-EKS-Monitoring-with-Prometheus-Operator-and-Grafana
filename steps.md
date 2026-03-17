## Monitoring Setup

For monitoring, pull and run the required containers for:

- Prometheus
- Alertmanager
- Grafana
- Blackbox Exporter

### Official Repositories

- [prom/prometheus](https://hub.docker.com/r/prom/prometheus)
- [grafana/grafana](https://hub.docker.com/r/grafana/grafana)
- [prom/blackbox-exporter](https://hub.docker.com/r/prom/blackbox-exporter)
- [prom/alertmanager](https://hub.docker.com/r/prom/alertmanager)

---

## 1. Pull the Required Docker Images

Run the following commands to pull the monitoring images:

```bash
docker pull prom/prometheus
docker pull prom/alertmanager
docker pull prom/blackbox-exporter
docker pull grafana/grafana
```

---

## 2. Run the Prometheus Container

Before running the Prometheus container, make sure the following files are present in your current directory:

- [alert_rules.yml](https://github.com/shashankc20mca/Enterprise-EKS-Monitoring-with-Prometheus-Grafana-Node-Exporter-Blackbox-Exporter-and-Alertmanage/blob/main/prometheus/alert_rules.yml)
- [prometheus_node_discovery.yml](https://github.com/shashankc20mca/Enterprise-EKS-Monitoring-with-Prometheus-Grafana-Node-Exporter-Blackbox-Exporter-and-Alertmanage/blob/main/prometheus/prometheus_node_discovery.yml)

Run the Prometheus container using:

```bash
docker run -d \
  --name prometheus_node_discovery \
  -p 9090:9090 \
  -v $(pwd)/prometheus_node_discovery.yml:/etc/prometheus/prometheus.yml \
  -v $(pwd)/alert_rules.yml:/etc/prometheus/alert_rules.yml \
  prom/prometheus
```

### Verify the Prometheus Container

```bash
docker ps
docker logs prometheus_node_discovery
```

Prometheus UI can be accessed at:

```text
http://<host-ip>:9090
```

---

## 3. Run the Alertmanager Container

Before running the Alertmanager container, make sure the following file is present in your current directory:

- [alertmanager.yml](https://github.com/shashankc20mca/Enterprise-EKS-Monitoring-with-Prometheus-Grafana-Node-Exporter-Blackbox-Exporter-and-Alertmanage/blob/main/alertmanager/alertmanager.yml)


Run the Alertmanager container using:

```bash
docker run -d \
  --name alertmanager \
  -p 9093:9093 \
  -v $(pwd)/alertmanager.yml:/etc/alertmanager/alertmanager.yml \
  prom/alertmanager
```

### Verify the Alertmanager Container

```bash
docker ps
docker logs alertmanager
```

Alertmanager UI can be accessed at:

```text
http://<host-ip>:9093
```

---

## 4. Run the Blackbox Exporter Container

Run the Blackbox Exporter container using:

```bash
docker run -d \
  --name blackbox-exporter \
  -p 9115:9115 \
  prom/blackbox-exporter
```

### Verify the Blackbox Exporter Container

```bash
docker ps
docker logs blackbox-exporter
```

Blackbox Exporter UI can be accessed at:

```text
http://<host-ip>:9115
```

---

## 5. Run the Grafana Container

Run the Grafana container using:

```bash
docker run -d \
  --name grafana \
  -p 3000:3000 \
  grafana/grafana
```

### Verify the Grafana Container

```bash
docker ps
docker logs grafana
```

Grafana UI can be accessed at:

```text
http://<host-ip>:3000
```

---

## 6. Check All Running Monitoring Containers

Run:

```bash
docker ps
```

You should see running containers for:

- Prometheus
- Alertmanager
- Blackbox Exporter
- Grafana

---

## 7. Useful Docker Commands

### Stop All Monitoring Containers

```bash
docker stop prometheus_node_discovery
docker stop alertmanager
docker stop blackbox-exporter
docker stop grafana
```

### Start All Monitoring Containers Again

```bash
docker start prometheus_node_discovery
docker start alertmanager
docker start blackbox-exporter
docker start grafana
```

### Remove All Monitoring Containers

```bash
docker rm -f prometheus_node_discovery
docker rm -f alertmanager
docker rm -f blackbox-exporter
docker rm -f grafana
```

---

## 8. Ports Used

| Service | Port |
|---|---|
| Prometheus | 9090 |
| Alertmanager | 9093 |
| Blackbox Exporter | 9115 |
| Grafana | 3000 |

---

## 9. Final Note

Make sure the required ports are opened in the bastion host security group so that the UIs can be accessed from the browser in the format:

```text
<bastion-ip>:port
```

Ports to open:

- Prometheus: `9090`
- Alertmanager: `9093`
- Blackbox Exporter: `9115`
- Grafana: `3000`
