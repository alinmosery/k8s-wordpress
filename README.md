#  WordPress Deployment on Kubernetes with Monitoring

פרויקט זה מציג הגירה של אפליקציית WordPress מפורמט Docker-Compose לסביבת Kubernetes מנוהלת. הפרויקט כולל פריסה מאובטחת, שמירת נתונים קבועה וניטור ביצועים בזמן אמת.

---

###  Architecture & Stack
- **Cloud Infrastructure:** AWS EC2 (T3.Medium hosting Minikube).
- **Orchestration:** Kubernetes managed with Helm Charts.
- **Container Registry:** Amazon ECR for custom images.
- **Application:** WordPress (Deployment with 2 Replicas).
- **Database:** MySQL (StatefulSet for data persistence).
- **Monitoring:** Prometheus & Grafana stack.
- **Traffic Routing:** NGINX Ingress Controller.

---

###  Installation & Setup (Quick Start)

**1. Install WordPress & MySQL:**
```bash
helm install wordpress-release ./my-wordpress-chart
