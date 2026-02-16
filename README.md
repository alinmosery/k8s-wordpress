פרויקט פריסת WordPress ומערכת ניטור ב-Kubernetes
תיאור הפרויקט
הפרויקט מציג פריסה אוטומטית של אפליקציית WordPress מבוססת בסיס נתונים MySQL על גבי Kubernetes. המערכת כוללת ניהול משאבים באמצעות Helm Chart והטמעת מערכת ניטור מלאה מבוססת Prometheus ו-Grafana.

ארכיטקטורת המערכת
Application Layer: WordPress Deployment המחובר לשירות LoadBalancer/NodePort.

Persistence Layer: MySQL Deployment עם שימוש ב-Persistent Volume Claim לשמירת נתונים.

Infrastructure: שימוש ב-Minikube על גבי שרת AWS EC2.

Monitoring Stack: איסוף מטריקות באמצעות Prometheus והצגתן בדאשבורד Grafana.

דרישות מקדימות
שרת לינוקס (EC2) עם Minikube מותקן.

כלי ניהול החבילות Helm.

גישה ל-Kubectl.

הוראות הרצה
הפעלת הקלאסטר:

Bash
minikube start
פריסת האפליקציה באמצעות Helm:
עבורי לתיקיית ה-Chart והריצי:

Bash
helm install wordpress-release ./my-wordpress-chart
התקנת מערכת הניטור:

Bash
helm install monitoring-stack prometheus-community/kube-prometheus-stack -n monitoring --create-namespace
אימות והתחברות
וידוא שהפודים רצים: kubectl get pods -A.

גישה לניטור: ביצוע Port Forward לפורט 3000 וחיבור דרך הדפדפן.

בדיקת תקינות: בגרפאנה הוגדר פאנל Stat המציג את זמינות הקונטיינר (Running/Down) על בסיס מטריקת kube_pod_container_status_running.

הגדרות Done
האפליקציה נגישה ומחוברת לבסיס הנתונים.

קיים ניטור פעיל בגרפאנה עבור הקונטיינרים.

כל המשאבים מנוהלים דרך Helm ומאוחסנים ב-Git.
