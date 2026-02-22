# WordPress Deployment on Kubernetes with Monitoring

פרויקט זה מציג הגירה של אפליקציית WordPress מפורמט Docker-Compose לסביבת Kubernetes מנוהלת. הפרויקט כולל פריסה מאובטחת, שמירת נתונים קבועה וניטור ביצועים בזמן אמת.

---

### Architecture & Stack
- **Cloud Infrastructure:** AWS EC2 (T3.Medium hosting Minikube).
- **Orchestration:** Kubernetes managed with Helm Charts.
- **Container Registry:** Amazon ECR for custom images.
- **Application:** WordPress (Deployment with 2 Replicas).
- **Database:** MySQL (StatefulSet for data persistence).
- **Monitoring:** Prometheus & Grafana stack.
- **Traffic Routing:** NGINX Ingress Controller.

---

### Installation & Setup (Quick Start)

**1. Install WordPress & MySQL:**
```bash
helm install wordpress-release ./my-wordpress-chart
```

**2. Add Prometheus Community Repo:**
```bash
helm repo add prometheus-community [https://prometheus-community.github.io/helm-charts](https://prometheus-community.github.io/helm-charts)
helm repo update
```

**3. Install Monitoring Stack:**
```bash
helm install monitoring-stack prometheus-community/kube-prometheus-stack -n monitoring --create-namespace
```

---

### Accessing the Services

כדי לגשת לשירותים מסביבת ה-EC2 המרוחקת לסביבה המקומית, השתמשתי ב-Port Forwarding:

**Access WordPress Site (Port 8080):**
```bash
kubectl port-forward --address 0.0.0.0 svc/wordpress 8080:80
```
> Visit: http://<Public-IP>:8080

**Access Grafana Dashboard (Port 3000):**
```bash
kubectl port-forward --address 0.0.0.0 svc/prometheus-grafana 3000:80 -n monitoring
```
> Visit: http://<Public-IP>:3000

---

### Monitoring Strategy

הגדרנו דאשבורד ייעודי בגרפאנה (App Uptime) המנטר את מצב הריצה של הקונטיינרים ב-Namespace של האפליקציה.

**PromQL Query used:**
```promql
kube_pod_container_status_running{namespace="default"}
```

שאילתה זו מאפשרת לנו לוודא שהאפליקציה (WordPress) ומסד הנתונים (MySQL) למעלה, תוך סינון רעשי רקע של הקלאסטר וקבלת אינדיקציה ויזואלית חדה.

---

### Definition of Done (Verified)
- [x] Application is reachable and displaying "Hello World".
- [x] Database is running as a StatefulSet with bound PVC.
- [x] Grafana panels are active and showing "Running" status.
- [x] Full Git repository with Helm structure is ready for submission.
