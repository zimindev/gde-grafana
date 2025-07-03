## 📊 **Grafana Setup & Configuration Guide**  

### ✅ **What is Grafana?**  
**Grafana** is an open-source **dashboard and visualization platform** that works with:  
✔ **Prometheus, Loki, InfluxDB, Elasticsearch, MySQL** (30+ data sources)  
✔ **Time-series, logs, and application metrics**  
✔ **Custom dashboards with panels, graphs, and alerts**  

Ideal for **monitoring servers, apps, and business metrics in real-time**.  

---

## 🛠️ **Step 1: Install Grafana**  

### **Linux (Debian/Ubuntu)**  
```bash
sudo apt-get install -y apt-transport-https
sudo apt-get install -y software-properties-common wget
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
sudo apt-get update
sudo apt-get install grafana
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

### **Docker**  
```bash
docker run -d -p 3000:3000 --name=grafana grafana/grafana-oss
```

### **Kubernetes (Helm)**  
```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm install grafana grafana/grafana
```

### **macOS (Homebrew)**  
```bash
brew install grafana
brew services start grafana
```

---

## 🌐 **Step 2: Access Grafana**  
1. Open in browser:  
   ```
   http://localhost:3000
   ```
2. **Default login**:  
   - Username: `admin`  
   - Password: `admin` (change on first login)  

---

## ⚙️ **Step 3: Add Data Sources**  

### **Prometheus**  
1. Go to **Configuration → Data Sources → Add data source**  
2. Select **Prometheus**  
3. Enter URL: `http://localhost:9090` (adjust if Prometheus runs elsewhere)  
4. Click **Save & Test**  

### **Other Supported Sources**  
- **Loki** (for logs): `http://localhost:3100`  
- **InfluxDB**: `http://localhost:8086`  
- **MySQL/PostgreSQL**: Enter DB credentials  

---

## 📈 **Step 4: Import Dashboards**  

### **Option 1: Import from Grafana.com**  
1. Find a dashboard (e.g., [Node Exporter Full](https://grafana.com/grafana/dashboards/1860))  
2. Copy its ID (`1860`)  
3. In Grafana: **Create → Import → Paste ID**  

### **Option 2: Manual JSON Upload**  
1. Download a dashboard JSON (e.g., from [Grafana Dashboards](https://grafana.com/grafana/dashboards/))  
2. Go to **Import → Upload JSON file**  

### **Popular Dashboards**  
| ID    | Name                          | Data Source     |  
|-------|-------------------------------|----------------|  
| `1860` | Node Exporter Full            | Prometheus     |  
| `315`  | Docker Monitoring             | Prometheus     |  
| `7362` | MySQL Overview                | MySQL          |  

---

## 🖌️ **Step 5: Create a Custom Dashboard**  

1. Click **Create → Dashboard**  
2. **Add a Panel** → Choose visualization (Graph, Stat, Table, etc.)  
3. **Write a Query** (e.g., PromQL for Prometheus):  
   ```promql
   sum(rate(node_cpu_seconds_total{mode="idle"}[5m])) by (instance)
   ```
4. Customize:  
   - **Panel title**  
   - **Axes/units**  
   - **Thresholds/colors**  

5. **Save** (Ctrl+S) with a name (e.g., `CPU Usage`).  

---

## 🔔 **Step 6: Set Up Alerts**  

1. Open a panel → **Alert → Create Alert**  
2. Set **conditions** (e.g., `CPU > 80% for 5m`)  
3. Configure **notifications**:  
   - **Email**: Add SMTP settings in `grafana.ini`  
   - **Slack/Discord**: Use [Alertmanager](https://grafana.com/docs/grafana/latest/alerting/))  
4. Test with **Preview Alerts**.  

---

## 🔧 **Step 7: Advanced Configuration**  

### **Customize `grafana.ini`** (Location: `/etc/grafana/grafana.ini`)  
```ini
[smtp]
enabled = true
host = smtp.gmail.com:587
user = your-email@gmail.com
password = your-password
```

### **Enable LDAP/Auth Proxy**  
```ini
[auth.ldap]
enabled = true
config_file = /etc/grafana/ldap.toml
```

### **Persist Data (Docker)**  
```bash
docker run -d \
  -v grafana-storage:/var/lib/grafana \
  -p 3000:3000 \
  grafana/grafana-oss
```

---

## 📚 **Cheat Sheet**  

| Task                      | Command/Path                     |  
|--------------------------|----------------------------------|  
| Restart Grafana          | `sudo systemctl restart grafana-server` |  
| Reset admin password     | `grafana-cli admin reset-admin-password newpassword` |  
| Logs location            | `/var/log/grafana/grafana.log`   |  
| Plugins directory        | `/var/lib/grafana/plugins`       |  

---

## 🚀 **Pro Tips**  
- **Variables**: Create dropdown filters (e.g., `$server`) in dashboard settings.  
- **Annotations**: Mark events (deployments/incidents) on graphs.  
- **Templating**: Use `$__interval` for dynamic time ranges.  
- **Plugins**: Install [plugins](https://grafana.com/grafana/plugins/) for **world maps, diagrams, etc**.  

---

Now you have a **powerful monitoring dashboard**! 🎉  
Visualize metrics, set alerts, and share insights with your team.  

For production, consider:  
- **Grafana Cloud** (hosted option)  
- **Authentication** (OAuth/LDAP)  
- **Backups** (regularly export dashboards as JSON).
