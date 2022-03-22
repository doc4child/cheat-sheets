# Kubernetes Cheat-Sheet

## microk8s
https://microk8s.io/docs/getting-started

Install microk8s during ubuntu server install
or
`sudo snap install microk8s --classic --channel=1.21`

sudo usermod -a -G microk8s $USER

sudo chown -f -R $USER ~/.kube

Restart group for changes to take effect.

su - $USER

Create alias

alias kubectl='microk8s kubectl'

install cert-manager
https://cert-manager.io/docs/installation/


Prerec

Following addons needed per
https://stackoverflow.com/questions/64393873/connection-refused-when-using-cert-manager-to-get-a-letsencrypt-certificate-for
microk8s enable dns ingress

microk8s enable dns ingress dashboard storage helm


Check latest version for installation

kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.7.1/cert-manager.yaml

check pods

kubectl -n cert-manager get pods

Create files microk8s-staging-issuer.yaml and microk8s-prod-issuer.yaml


microk8s-staging-issuer.yaml

```colsole
   apiVersion: cert-manager.io/v1
   kind: ClusterIssuer
   metadata:
     name: letsencrypt-staging
   spec:
     acme:
       # The ACME server URL
       server: https://acme-staging-v02.api.letsencrypt.org/directory
       # Email address used for ACME registration
       email: doc4child@gmail.com
       # Name of a secret used to store the ACME account private key
       privateKeySecretRef:
         name: letsencrypt-staging
       # Enable the HTTP-01 challenge provider
       solvers:
       - http01:
           ingress:
             class:  public


```

microk8s-prod-issuer.yaml
```console
   apiVersion: cert-manager.io/v1
   kind: ClusterIssuer
   metadata:
     name: letsencrypt-prod
   spec:
     acme:
       # The ACME server URL
       server: https://acme-v02.api.letsencrypt.org/directory
       # Email address used for ACME registration
       email: doc4child@gmail.com
       # Name of a secret used to store the ACME account private key
       privateKeySecretRef:
         name: letsencrypt-prod
       # Enable the HTTP-01 challenge provider
       solvers:
       - http01:
           ingress:
             class: public
```
kubectl apply -f microk8s-staging-issuer.yaml -f microk8s-prod-issuer.yaml

kubectl describe clusterissuer <ClusterIssuer name>

## install helm

sudo snap install helm --classic

## Networking
Connect containers using Kubernetes internal DNS system:
`<service-name>.<namespace>.svc.cluster.local`

Troubleshoot Networking with a netshoot toolkit Container:
`kubectl run tmp-shell --rm -i --tty --image nicolaka/netshoot -- /bin/bash`

## Containers
Restart Deployments (Stops and Restarts all Pods):
`kubectl scale deploy <deployment> --replicas=0`
`kubectl scale deploy <deployment> --replicas=1`

Executing Commands on Pods:
`kubectl exec -it <PODNAME> -- <COMMAND>`
`kubectl exec -it generic-pod -- /bin/bash` 

## Config and Cluster Management
COMMAND | DESCRIPTION
---|---
`kubectl cluster-info` | Display endpoint information about the master and services in the cluster
`kubectl config view` |Get the configuration of the cluster
## Resource Management
COMMAND | DESCRIPTION
---|---
`kubectl get all --all-namespaces` | List all resources in the entire Cluster
`kubectl delete <RESOURCE> <RESOURCENAME> --grace-period=0 --force` | Try to force the deletion of the resource

### List of kubectl Short Names
Short Name | Long Name
---|---
`csr`|`certificatesigningrequests`
`cs`|`componentstatuses`
`cm`|`configmaps`
`ds`|`daemonsets`
`deploy`|`deployments`
`ep`|`endpoints`
`ev`|`events`
`hpa`|`horizontalpodautoscalers`
`ing`|`ingresses`
`limits`|`limitranges`
`ns`|`namespaces`
`no`|`nodes`
`pvc`|`persistentvolumeclaims`
`pv`|`persistentvolumes`
`po`|`pods`
`pdb`|`poddisruptionbudgets`
`psp`|`podsecuritypolicies`
`rs`|`replicasets`
`rc`|`replicationcontrollers`
`quota`|`resourcequotas`
`sa`|`serviceaccounts`
`svc`|`services`
## Logs and Troubleshooting
### Logs


### MySQL 
`kubectl run -it --rm --image=mysql:5.7 --restart=Never mysql-client -- mysql -u USERNAME -h HOSTNAME -p`
