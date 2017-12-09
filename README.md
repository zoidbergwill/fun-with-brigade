# Fun with Brigade and Kashti

## Quickstart

Make sure you have `helm` installed first, then initialise it in your
cluster:
```
helm init
```

Deploy brigade to your cluster:
```
helm repo add brigade https://azure.github.io/brigade
helm install -n brigade brigade/brigade
```

Create your project's brigade helm chart:
```
helm inspect values brigade/brigade-project > myvalues.yaml
```

Once you're done there, deploy its Helm chart:
```
helm install --name test-project brigade/brigade-project -f myvalues.yaml
```

Install the brigade CLI:
```
git clone https://github.com/Azure/brigade $(go env GOPATH)/src/github.com/Azure/brigade
cd $(go env GOPATH)/src/github.com/Azure/brigade
make bootstrap brig
```

See if your project installed successfully:
```
${GOPATH}/src/github.com/Azure/brigade/bin/brig project list
```

Test your Brigade build config javascript file:
```
cd ../brigade-test-repo
${GOPATH}/src/github.com/Azure/brigade/bin/brig run -f brigade.js zoidbergwill/brigade-test-repo
```

Install Kashti
```
helm install -n kashti ./charts/kashti --set service.type=NodePort --set brigade.apiServer=http://localhost:7745
```

Open tunnels to Kashti and Brigade so we can take a look at Kashti's
dashboard and it can connect to Brigade:
```
kubectl port-forward $(kubectl get po | grep brigade-brigade-api | awk '{ print $1 }') 7745
kubectl port-forward $(kubectl get po | grep kashti | awk '{ print $1 }') 8080:80
```
