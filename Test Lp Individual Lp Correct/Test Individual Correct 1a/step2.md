---
title: Introduction to Istio and installation process

---
<!--Installation of Istio in the cluster-->

As we have seen in the theories that Istio helps in network communication for the cluster mainly for microservice architectures. Before moving to the service mesh terms, let's first learn how to install Istio.

Managing the services manually becomes a challenge as all of these non-business services require extra management. But Istio makes it centralized and covers all the mess in the mesh.! We implement this Istio service as a sidecar proxy that is independent of the business application.

Istio has a separate control plane, and we only have to install it in the master node. Rest sidecar injection will be done automatically.

Let's install Istio in the master node:

*This command will download the latest stable version. Currently, the latest version is 1.13.2.* 

Run the below command to download the latest release:

{{ execute }}
```
curl -L https://istio.io/downloadIstio | sh -
```
{{ /execute }}

Switch to the Istio download directory:

{{ execute }}
```
cd istio-1.13.2
```
{{ /execute }}

Add `istioctl` client path to the path:

{{ execute }}
```
export PATH=$PWD/bin:$PATH
```
{{ /execute }}

Now let's install Istio and the services:

{{ execute }}
```
istioctl install --set profile=demo -y
```
{{ /execute }}

Now label the namespace so that the control plane can perform Istio injection in the application and detect it via this label:

{{ execute }}
```
kubectl label namespace default istio-injection=enabled
```
{{ /execute }}

Till here, we've completed the download and installation of the Istio v1.13.2 and labeled the namespace for further automated processes. We will see the application deployment part and networking setup in the next step.