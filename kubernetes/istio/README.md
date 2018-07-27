# Install Istio on AKS

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
$workfolder = "C:\Temp\istio"
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
$workfolder="$workfolder\istio-0.8.0\"
kubectl create -f "$workfolder/install/kubernetes/helm/helm-service-account.yaml"
helm init --service-account tiller
```

```
helm install "$workfolder/install/kubernetes/helm/istio" --name istio --namespace istio-system

#List the Istio Components
kubectl get pods -n istio-system
```