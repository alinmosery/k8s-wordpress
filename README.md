פרויקט פריסת WordPress ומערכת ניטור ב-Kubernetes
תיאור הפרויקט
פרויקט זה מבצע פריסה אוטומטית של אפליקציית WordPress מבוססת בסיס נתונים MySQL על גבי קלאסטר Kubernetes. הפרויקט כולל ניהול משאבים באמצעות Helm Chart ומערכת ניטור מלאה (Monitoring) מבוססת Prometheus ו-Grafana למעקב אחר ביצועי הקונטיינרים.

ארכיטקטורה
Application Layer: WordPress Deployment.

Database Layer: MySQL Deployment עם Persistent Volume Claim (PVC) לשמירת נתונים.

Monitoring Stack: Prometheus לאיסוף מטריקות ו-Grafana להצגת דאשבורדים.

Package Management: שימוש ב-Helm לניהול הגדרות הפריסה.

דרישות מקדימות
קלאסטר Kubernetes פעיל.

Helm מותקן ומקושר לקלאסטר.

Kube-state-metrics מותקן (חלק מ-kube-prom-stack).

הוראות הרצה
התקנת ה-Chart:
מעבר לתיקיית הפרויקט והרצת הפקודה:

Bash
helm install wordpress-app ./my-wordpress-chart
התקנת מערכת הניטור:
הוספת ה-Repository והתקנת ה-Stack:

Bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install monitoring-stack prometheus-community/kube-prometheus-stack -n monitoring --create-namespace
אימות פריסה
בדיקת סטטוס פודים: kubectl get pods.

וידוא גישה לאתר: בדיקת ה-Service של WordPress וגישה דרך הדפדפן.

וידוא ניטור: כניסה ל-Grafana Explore ובדיקת המטריקה kube_pod_container_status_running.

הגדרות ניטור
במערכת ה-Grafana הוגדר דאשבורד ייעודי המציג את זמינות הקונטיינרים (Uptime). הפאנל מוגדר עם Value Mappings המציג סטטוס Running עבור ערך 1 וסטטוס Down עבור ערך 0.
