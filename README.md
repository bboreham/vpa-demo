# vpa-demo
Testing code used in preparation for KubeCon EU 2025 talk

# Running the kind cluster

1. `kind create cluster --config kind-config.yaml`
1. `kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.7.2/components.yaml`
1. `kubectl patch -n kube-system deployment metrics-server --type=json   -p '[{"op":"add","path":"/spec/template/spec/containers/0/args/-","value":"--kubelet-insecure-tls"}]'`

# Deploying the Grafana stack

[File not included at present since it includes a secret key. TBD.]

```
helm repo add grafana https://grafana.github.io/helm-charts &&
  helm repo update &&
  helm upgrade --install --version ^2 --atomic --timeout 300s grafana-k8s-monitoring grafana/k8s-monitoring \
    --namespace "default" --create-namespace --values grafana-monitoring.yaml
```

# Installing the default VPA

```
git clone https://github.com/kubernetes/autoscaler.git
cd autoscaler/vertical-pod-autoscaler/
./hack/vpa-up.sh
```

# Creating a demo VPA

`kubectl apply -f vpa.yaml`

# Deploying a CPU heavy workload

1. Deploy the daemonset: `kubectl apply -f deploy-random-logger.yaml`
