---
title: Installing the microservice application

---
<!--Installing the microservice application-->

Let's install a sample microservice application in the cluster. We are not focusing much on building a microservice application as the out main goal is to implement Istio operations. Hence, a default demo application is taken in the example. The application is a `booking` app that contains basic information and a catalog for the stored books.

Let's directly apply the YAML file from the default demo application directory:

{{execute}}
```
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
```
{{/execute}}

Now the application will start and all the pods will get in the ready state then the control plane will inject the sidecar proxy in the application pods. This may take some time to get up and running. Grab a cup of coffee till then.

Run the below command after several minutes to verify the application pod status:

{{execute}}
```
kubectl get pods
```
{{/execute}}

You'll see all pods are initialized, up and running. Now let's apply the networking capabilities given by Istio. We'll implement an Istio ingress gateway that helps in routing the traffic to the deployed mesh architecture.

Run the below command for implementing ingress gateway from the sample application:

{{execute}}
```
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
```
{{/execute}}

Now run the built-in istio command to analyze the network setup and know about the errors if any:

{{execute}}
```
istioctl analyze
```
{{/execute}}

In our current example, the analysis part should approve the validation check. Now we've created the gateway, let's add the ingress ports to expose the application. We will use the `nodePort` service type in this example.

{{execute}}
```
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')
export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].nodePort}')
```
{{/execute}}

Now run the below command to allow the traffic from the created ingress gateway:

{{execute}}
```
export INGRESS_HOST=$(kubectl get po -l istio=ingressgateway -n istio-system -o jsonpath='{.items[0].status.hostIP}')
```
{{/execute}}

And now expose the gateway URL which is nothing but just a combination of IP and bound port:

{{execute}}
```
export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
```
{{/execute}}

Kubernetes API is at the core of Kubernetes. It exposes a HTTP API server that let you manage your cluster without the need to use a CLI or a UI.
![duck](/samplereorder.png "Testing Images")
Basically, the API helps administrators and developers manager their Kubernetes resources like namespaces, services, pods, events, logs and config maps.
![assets](/assets1.png "Testing Images")
Even if most Kubernetes operations can be done using the kubectl CLI, learning the API is the best way for complex use cases and when you want to create an application that interacts with Kubernetes.
![tree](/tree.png "Testing Images")s