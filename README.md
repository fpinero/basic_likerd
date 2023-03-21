# basic_likerd
demostración básica del service mesh likerd

* Install the CLI
```
curl --proto '=https' --tlsv1.2 -sSfL https://run.linkerd.io/install | sh
```
* (Optional) Add the linkerd CLI to your path with:
```
export PATH=$PATH:/Users/fpinero/.linkerd2/bin
```
* Validate your Kubernetes cluster
```
linkerd check --pre
```
* Install Linkerd onto your cluster
```
linkerd install --crds | kubectl apply -f -
```
````
linkerd install | kubectl apply -f -
````
* Wait until everything is up and running (Status check results are √) executing this command:
```
linkerd check
```
* Install the demo app
````
curl --proto '=https' --tlsv1.2 -sSfL https://run.linkerd.io/emojivoto.yml \
  | kubectl apply -f -
````
This command installs Emojivoto onto your cluster, but Linkerd hasn’t been activated on it yet—we’ll need to “mesh” the application before Linkerd can work its magic.

Before we mesh it, let’s take a look at Emojivoto in its natural state. We’ll do this by forwarding traffic to its web-svc service so that we can point our browser to it. Forward web-svc locally to port 8080 by running:
`````
kubectl -n emojivoto port-forward svc/web-svc 8080:80
`````
Now visit http://localhost:8080. Voila! You should see Emojivoto in all its glory.
<br />
With Emoji installed and running, we’re ready to mesh it—that is, to add Linkerd’s data plane proxies to it. We can do this on a live application without downtime, thanks to Kubernetes’s rolling deploys. Mesh your Emojivoto application by running:
````
kubectl get -n emojivoto deploy -o yaml \
  | linkerd inject - \
  | kubectl apply -f -
````
* Verify that everything is working the way it should on the data plane side. Check your data plane with:
```
linkerd -n emojivoto check --proxy
```
# Explore Linkerd!
* Let’s install the viz extension, which will install an on-cluster metric stack and dashboard. To install the viz extension, run:
```
linkerd viz install | kubectl apply -f - 
```
* Wait again till everything is up and running (Status check results are √) executing this command:
````
linkerd check
````
With the control plane and extensions installed and running, we’re now ready to explore Linkerd! Access the dashboard with:
````
linkerd viz dashboard &
````
The browser will pop up showing linkerd dashboard 

