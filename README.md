# פרויקט DevOps: פריסת WordPress על Kubernetes עם ניטור מלא

פרויקט זה מציג פריסה (Deployment) מלאה של אפליקציית WordPress המחוברת לבסיס נתונים MySQL, על גבי קלאסטר Kubernetes.
המערכת מדמה סביבת ייצור (Production) וכוללת ניהול גרסאות, שמירת נתונים קבועה (Persistence), ומערכת ניטור בזמן אמת.

**פיצ'רים מרכזיים:**
* **High Availability:** אפליקציית WordPress רצה כ-Deployment עם יכולת לגידול (Scalability).
* **Data Persistence:** בסיס הנתונים MySQL מוגדר כ-**StatefulSet** עם **PVC** כדי להבטיח שהמידע יישמר גם במקרה של נפילת פוד או אתחול.
* **Container Registry:** שימוש ב-**AWS ECR** לאחסון ומשיכת ה-Images.
* **Observability:** מערכת ניטור מלאה הכוללת **Prometheus** לאיסוף מטריקות ו-**Grafana** להצגת דאשבורדים.
* **Package Management:** שימוש ב-**Helm** לניהול ההתקנות.

---

### ארכיטקטורה וטכנולוגיות
* **תשתית ענן:** AWS EC2 (T3.Medium).
* **אורקסטרציה:** Minikube (Kubernetes).
* **בסיס נתונים:** MySQL 8.0 (StatefulSet).
* **ניטור:** Kube-Prometheus-Stack (Community Helm Chart).
* **Ingress:** Nginx Ingress Controller.

---

###  הוראות התקנה (Installation)

#### 1. דרישות קדם
* קלאסטר Kubernetes פעיל (Minikube).
* כלי CLI מותקנים: `kubectl`, `helm`.
* גישה ל-AWS ECR (עבור משיכת האימג'ים).

#### 2. התקנת האפליקציה (WordPress + MySQL)
ההתקנה מתבצעת באמצעות Helm Chart ייעודי שנכתב לפרויקט:
```bash
helm install wordpress-release ./my-wordpress-chart
התקנת מערכת הניטור
התקנת ה-Stack של פרומיתאוס וגרפאנה:

Bash
helm repo add prometheus-community [https://prometheus-community.github.io/helm-charts](https://prometheus-community.github.io/helm-charts)
helm install monitoring-stack prometheus-community/kube-prometheus-stack -n monitoring
גישה למערכת (Access)
מכיוון שהפרויקט רץ על סביבת Minikube בתוך שרת מרוחק (EC2), הגישה מתבצעת בצורה מאובטחת באמצעות Port Forwarding:

1. גישה לאתר (WordPress):

Bash
kubectl port-forward svc/wordpress 8080:80
הכתובת בדפדפן: http://<Server-IP>:8080

2. גישה ללוח הבקרה (Grafana):

Bash
kubectl port-forward svc/prometheus-grafana 3000:80 -n monitoring
הכתובת בדפדפן: http://<Server-IP>:3000

 ניטור (Monitoring Strategy)
הוגדר דאשבורד ייעודי ב-Grafana לניטור בריאות האפליקציה.

המטריקה: kube_pod_container_status_running{namespace="default"}

המטרה: סינון רעשי רקע (System Pods) והתמקדות בסטטוס ה-Uptime של קונטיינר הוורדפרס וה-DB בלבד.

ויזואליזציה: פאנל Stat המציג חיווי ברור (ירוק/אדום) לזמינות השירותים.
