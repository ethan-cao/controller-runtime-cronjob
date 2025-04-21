# controller-runtime-cronjob

this is a project following https://book.kubebuilder.io/cronjob-tutorial/cronjob-tutorial

```
# create API domain
kubebuilder init --domain tutorial.kubebuilder.io --repo tutorial.kubebuilder.io/controller-runtime-cronjob

# create API
kubebuilder create api --group batch --version v1 --kind CronJob

# scaffold defaulting and validating webhooks for CRD CronJob 
kubebuilder create webhook --group batch --version v1 --kind CronJob --defaulting --programmatic-validation


# Generate new manifests: WebhookConfiguration, ClusterRole and CustomResourceDefinition objects.
make manifests

```
