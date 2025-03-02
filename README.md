## Infraestructura

### Sistemas desplegados:
Localmente en Proxmox VE 8.2.2 desplegado:
- k8s controlplane - TALOS OS - VM
- k8s worker0 - TALOS OS - VM
- k8s worker1 - TALOS OS - VM
- k8s worker2 - TALOS OS - VM
- talosctl - kubectl command line, Debian 12 - VM

### Detalles de HW:
- 16 x AMD Ryzen 9 6900HX with Radeon Graphics (1 Socket)
- 64 Gb DDR5 
- 1 Tb SSD

## Configuración

### Clonar el repositorio de K8s Operators de Prometheus, Grafana y AlertManager
- git clone https://github.com/prometheus-operator/kube-prometheus
- cd kube-prometheus

### Crear el namespace "monitoring" y los CRDs (Custom Resource Definition)
- kubectl create -f manifests/setup
- kubectl create -f manifests/

### Configurar la Política de Seguridad de Pods (PSP) en el nivel privileged
- kubectl label --overwrite ns monitoring pod-security.kubernetes.io/enforce=privileged

### Get de todos los componentes del namespace "monitoring"
- kubectl get all -n monitoring

### Forwardear los puertos de Prometheus para acceder al dashboard via web
- kubectl --namespace monitoring port-forward svc/prometheus-k8s 9090

### Forwardear los puertos de Grafana para acceder al dashboard via web
- kubectl --namespace monitoring port-forward svc/grafana 3000

### Forwardear los puertos de AlertManager para acceder al dashboard via web
- kubectl --namespace monitoring port-forward svc/alertmanager-main 9093

=============

## Acceso desde Internet

### Hacer SSH tunneling desde otro host hacia el talosctl
- ssh -L 0.0.0.0:3000:localhost:3000 root@talosctl.matanga.it

### Configurar portforwarding en el router local para que apunte al equipo en el que se encuentre el NGINX

### Configurar el NGINX para que forwardee hacia el equipo que tiene el SSH tunneling

### Configurar el DDNS en el router local

### Configurar CloudFlare con el CNAME adecuado para apuntar el DDNS
