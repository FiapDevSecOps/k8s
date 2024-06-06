# Para um Pod
kubectl run my-pod --image=nginx --dry-run=client -o yaml

# Para um Service
kubectl create service clusterip my-svc --tcp=5678:8080 --dry-run=client -o yaml

# Para um Deployment
kubectl create deployment my-dep --image=nginx --dry-run=client -o yaml

# Para um Namespace
kubectl create namespace my-namespace --dry-run=client -o yaml

# Para um Secret
kubectl create secret generic my-secret --from-literal=key1=value1 --dry-run=client -o yaml

# Para um ConfigMap
kubectl create configmap my-config --from-literal=key1=value1 --dry-run=client -o yaml
