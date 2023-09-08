Run on Linux laptop
# Install NGINX Ingress Controler #
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.1/deploy/static/provider/cloud/deploy.yaml
kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=120s
```

# Deploy simple nginx #
```
kubectl create deployment demo --image=httpd --port=80
kubectl expose deployment demo
```

# Create ingress for a domain on port 80 #
```
kubectl create ingress demo-localhost --class=nginx \
  --rule="demo.localdev.me/*=demo:80"
```

# Deploy simple nginx #
```
kubectl port-forward --namespace=ingress-nginx service/ingress-nginx-controller 8080:80
```
# Check deployment (expect: It works!) #
```
curl --resolve demo.localdev.me:8080:127.0.0.1 http://demo.localdev.me:8080
```
