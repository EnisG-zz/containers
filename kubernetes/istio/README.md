# Istio on AKS

Ref: http://claudioed.tech/2018/07/15/install-istio-on-azure-aks/

## Requirements

Install choco
```
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

Install helm
```
choco install kubernetes-helm
```

## Setup 

Download istio setup files
```
$url = "https://github.com/istio/istio/releases/download/0.8.0/istio-0.8.0-win.zip"
$workfolder = "C:"
$istio = "$workfolder\istio-0.8.0"
$zipfile = "$workfolder\istio.zip"

md $workfolder

Add-Type -AssemblyName System.IO.Compression.FileSystem
Add-Type -AssemblyName System.Net
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12

Invoke-WebRequest -Uri $url -OutFile $zipfile
[System.IO.Compression.ZipFile]::ExtractToDirectory($zipfile, $workfolder)
rm $zipfile

```

Initialize helm (tiller)
```
kubectl create -f "$istio/install/kubernetes/helm/helm-service-account.yaml"
helm init --service-account tiller
```

```
helm install "$istio/install/kubernetes/helm/istio" --name istio --namespace istio-system

#List the Istio Components
kubectl get pods -n istio-system
```

## Run BookStore Sample

https://istio.io/docs/guides/bookinfo/

```
cd $istio
istioctl kube-inject -f samples/bookinfo/kube/bookinfo.yaml | kubectl apply -f -
istioctl create -f samples/bookinfo/routing/bookinfo-gateway.yaml

$INGRESS_HOST=kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0
].ip}'
$INGRESS_PORT=kubectl -n istio-system get service istio-ingressgateway -o jsonpath="{.spec.ports[?(@.name=='http')]
.port}"
$GATEWAY_URL=$INGRESS_HOST+":"+$INGRESS_PORT
curl http://${GATEWAY_URL}/productpage

```