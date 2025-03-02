# Clone the Prometheus Operator repository
git clone https://github.com/prometheus-operator/kube-prometheus
cd kube-prometheus

# Create the namespace and CRDs, and then wait for them to be availble before creating the remaining resources
kubectl create -f manifests/setup
until kubectl get servicemonitors --all-namespaces ; do date; sleep 1; echo ""; done
kubectl create -f manifests/

# Get created resources
kubectl get all -n monitoring

# Acces to Prometheus dashboard
kubectl --namespace monitoring port-forward svc/prometheus-k8s 9090

# Acces to Grafana dashboard
kubectl --namespace monitoring port-forward svc/grafana 3000

# Acces to Alert Manager dashboard
kubectl --namespace monitoring port-forward svc/alertmanager-main 9093
