# controller-runtime-cronjob
This is a project following https://book.kubebuilder.io/cronjob-tutorial/cronjob-tutorial

## Overview on CronJob CRD

This CronJob CRD (Custom Resource Definition) extends Kubernetes to support scheduled jobs with cron-like syntax. It allows you to:

- Schedule jobs to run periodically on a cron schedule (e.g. "*/1 * * * *" for every minute)
- Configure job concurrency policies (Allow, Forbid, Replace)
- Set starting deadlines and history limits
- Define the job template including container specs, just like a regular Kubernetes Job

Key features:
- Cron scheduling using standard cron syntax
- Automatic job creation and cleanup
- Configurable concurrency handling
- Full job template customization
- Built-in defaulting and validation via webhooks



## bootstrap
```
# create API domain
kubebuilder init --domain tutorial.kubebuilder.io --repo tutorial.kubebuilder.io/controller-runtime-cronjob

# create API (CRD)
kubebuilder create api --group batch --version v1 --kind CronJob

# scaffold default and validating webhooks for CRD CronJob 
kubebuilder create webhook --group batch --version v1 --kind CronJob --defaulting --programmatic-validation
```


## CRD
```
# Generate new manifests: WebhookConfiguration, ClusterRole and CustomResourceDefinition objects.
make manifests

# install CRD / manifests to cluster
make install

# disable webhook
# If you want to run the webhooks locally, you’ll have to generate certificates for serving the webhooks, and place them in the right directory (/tmp/k8s-webhook-server/serving-certs/tls.{crt,key}, by default).
# If you’re not running a local API server, you’ll also need to figure out how to proxy traffic from the remote cluster to your local webhook server. For this reason, we generally recommend disabling webhooks when doing your local code-run-test cycle, as we do below.

export ENABLE_WEBHOOKS=false

# start controller in cluster, without image
make run

# build the image using the default container tool (Docker) with the tag specified in the IMG variable (default is controller:latest)
make docker-build

# build and load image to Kind cluster
kind load docker-image <your-image-name>:tag --name <your-kind-cluster-name>
kind load docker-image controller:latest --name my-cluster

# avoid pull image in config/manager/manager.yaml, since image is already loaded
add-> imagePullPolicy: Never

# install CRD in cluster
kubectl apply -f config/crd/bases/batch.tutorial.kubebuilder.io_cronjobs.yaml

# add permission
kubectl apply -f config/rbac/

# run controller-manager
# this manager pod is not working, as certificates is not added
# todo: follow https://book.kubebuilder.io/cronjob-tutorial/cert-manager
kubectl apply -f config/manager/manager.yaml 

```

## CR
```
# add the example CR
kubectl create -f config/samples/batch_v1_cronjob.yaml

# check CR
kubectl get cronjob

```
