פרויקט DevOps: פריסת WordPress על Kubernetes עם ניטור מלא 🚀פרויקט זה מציג הגירה של אפליקציית WordPress מפורמט Docker-Compose לסביבת Kubernetes (Minikube) על גבי AWS EC2. המערכת כוללת שרידות נתונים גבוהה, ניטור בזמן אמת וניהול באמצעות Helm.🏗️ טכנולוגיות וארכיטקטורהרכיבטכנולוגיהתיאורתשתיתAWS EC2 (T3.Medium)שרת האירוח של המערכת.ניהול קלאסטרMinikube (Kubernetes)הרצת הקונטיינרים והשירותים.ניהול חבילותHelm Chartsניהול פריסת האפליקציה והניטור.אחסון אימג'יםAmazon ECRאחסון מאובטח של ה-Docker Images.בסיס נתוניםMySQL StatefulSetהבטחת זהות קבועה ושמירת נתונים (Persistence).ניטורPrometheus & Grafanaאיסוף מטריקות וויזואליזציה של בריאות המערכת.⚙️ הוראות הרצה מהירה (Copy-Paste)לביצוע התקנה נקייה של הפרויקט, הרץ את הפקודות הבאות בטרמינל:1. התקנת האפליקציה (WordPress & MySQL)Bash# התקנת ה-Chart המקומי של האפליקציה
helm install wordpress-release ./my-wordpress-chart
2. התקנת מערכת הניטור (Prometheus & Grafana)Bash# הוספת המאגר של Prometheus
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

# התקנת הסטאק ב-Namespace ייעודי
helm install monitoring-stack prometheus-community/kube-prometheus-stack -n monitoring --create-namespace
🖥️ גישה לשירותים (Access)מכיוון שהפרויקט רץ בסביבת Minikube מרוחקת, הגישה מתבצעת באמצעות Port Forwarding:גישה לאתר WordPressBashkubectl port-forward --address 0.0.0.0 svc/wordpress 8080:80
כניסה בדפדפן: http://<Public-IP>:8080גישה ללוח הבקרה GrafanaBashkubectl port-forward --address 0.0.0.0 svc/prometheus-grafana 3000:80 -n monitoring
כניסה בדפדפן: http://<Public-IP>:3000📊 אסטרטגיית ניטורבנינו דאשבורד ייעודי ב-Grafana תחת השם App Uptime המנטר את זמינות הקונטיינרים ב-Namespace של האפליקציה.השאילתה המרכזית (PromQL):קטע קודkube_pod_container_status_running{namespace="default"}
שאילתה זו מסננת רעשי מערכת ומציגה חיווי ויזואלי מהיר (Stat Panel) על בריאות הוורדפרס ובסיס הנתונים בלבד.
