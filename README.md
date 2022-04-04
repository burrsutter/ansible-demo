# ansible-demo

Deploy StackRox and create sales demos on k8s/OpenShift with Ansible

To use:

1. Base64 encode your kubeconfig (must have only one context -- this is an Ansible limitation) 
```
base64 -in $KUBECONFIG -okubeconfig-base64.txt
```
1. Optional: docker config.json with read access to gcr.io/rox-se
```
base64 -in ~/.docker/config.json -odockerconfig-base64.txt
```  
2. ADMIN_PASSWORD via 
```
kubectl -n stackrox get secret central-htpasswd -o go-template='{{index .data "password" | base64decode}}'
```
3. CENTRAL_ADDR via 
```
kubectl -n stackrox get route central -o jsonpath="{.status.ingress[0].host}"
```
5. Copy `docker-compose.yml` and `sample.env` from the repo.  Rename `sample.env` to `config.env` and put the proper values for each of the variables in there.  For example:

```
KUBECONFIG_BASE64=TG9yZW0gaXBzdW0gZG9sb3Igc2l0IGFtZXQsIGNvbnNlY3RldHVyIGFkaXBpc2NpbmcgZWxpdC4gVml2YW11cyBmYWNpbGlzaXMgZWxlaWZlbmQgZWxlbWVudHVtLiBBbGlxdWFtIHVsbGFtY29ycGVyIHJpc3VzIGxvcmVtLCBuZWMgYXVjdG9yLgo=
DOCKERCONFIG_BASE64=<optinonal>
CENTRAL_PORT=443
ADMIN_PASSWORD=ThisIsAnUnusuallyStrongPassphraseThatYou'llEndUpTypoing
CENTRAL_ADDR=
ORCHESTRATOR=openshift
IMAGE_PULL_USER=<optinonal>
IMAGE_PULL_PASSWORD=<optinonal>
```


Add the appropriate values to `config.env`.)


(optional:  If `CENTRAL_ADDR` is supplied, the playbook will skip installing Central and the cluster bundle.)
(optional:  If you want to pull images from `stackrox.io` directly, omit IMAGE_REGISTRY and provide credentials for `stackrox.io`.)

6. Invoke the `docker-compose.yml` with `docker-compose run ansible-demo-build`

