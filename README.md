## Configuración

### Clonar el repositorio de K8s Operators de Prometheus, Grafana y AlertManager
git clone https://github.com/prometheus-operator/kube-prometheus
cd kube-prometheus

### Crear el namespace "monitoring" y los CRDs (Custom Resource Definition)
### Create the namespace and CRDs, and then wait for them to be availble before creating the remaining resources
- kubectl create -f manifests/setup
- kubectl create -f manifests/

### Configurar la Política de Seguridad de Pods (PSP) en el nivel privileged
kubectl label --overwrite ns monitoring pod-security.kubernetes.io/enforce=privileged

### Get de todos los componentes del namespace "monitoring"
kubectl get all -n monitoring

### Forwardear los puertos de Prometheus para acceder al dashboard via web
kubectl --namespace monitoring port-forward svc/prometheus-k8s 9090

### Forwardear los puertos de Grafana para acceder al dashboard via web
kubectl --namespace monitoring port-forward svc/grafana 3000

=============

## Acceso desde Internet

### Forwardear los puertos de AlertManager para acceder al dashboard via web
kubectl --namespace monitoring port-forward svc/alertmanager-main 9093

### Para acceder desde Internet, hacer SSH tunneling hacia el sistema
ssh -L 0.0.0.0:3000:localhost:3000 root@talosctl.matanga.it

### Configurar portforwarding en el router donde se encuentre el NGINX
### Configurar el NGINX para que forwardee hacia el equipo que tiene el SSH tunneling
