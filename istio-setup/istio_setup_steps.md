# Istio-Learning

Istio Setup

- Make sure you can connect using kubectl

installation:

- download isito with the following comand this will download latest isio on a folder
  curl -L https://istio.io/downloadIstio | sh -
- copy the istio folder to ~/opt/
  - create the directory with the following command
    mkdir -p ~/opt
- mv istio-1.14.3/ ~/opt/
- add isito bin folder in .bashrc path
  $HOME/.local/bin:$HOME/bin:$HOME/opt/istio-1.14.3/bin:$PATH

- Check the different isito profile for different settings

- simple installation command:
  - istioctl install --set profile=demo
  - or switch profile by simply running command
  - istioctl install --set profile=default
  - kubectl get po -n istio-system
  - to show all the experimental command
    istioctl x
  - to uninstall istio
    istioctl x uninstall --purge
  - kubectl delete namespace istio-system

Installation through yaml file

- istioctl profile dump demo > raw_demo_settings.yaml
- istioctl profile dump default > raw_default_settings.yaml

Change
enable egress Gateway
egressGateways: - enabled: true

- istioctl install -f tuned_default_settings.yaml
- istioctl manifest generate -f tuned_default_settings.yaml

Deploy using k8s yaml file:

add the following in the generated kubernetes yaml file:
istioctl manifest generate -f tuned_default_settings.yaml > plain_istio_kubernetes.yaml

```
apiVersion: v1
kind: Namespace
metadata:
  name: decofe-uat
```

kubectl apply -f plain_istio_kubernetes.yaml
kubectl delete -f plain_istio_kubernetes.yaml

to add the addons (kiali, graphana, premetheus, jaeger etc) go to

- cd ~/opt/istio-1.14.3/samples/addons
- kubectl apply -f .
- kubectl port-forward svc/kiali 20001:20001 -n istio-system

to enable istio-proxy on a namespace.

- kubectl get ns default --show-labels
- kubectl label namespace default istio-injection=enabled
