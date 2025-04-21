# controller-runtime-cronjob

this is a project following https://book.kubebuilder.io/cronjob-tutorial/cronjob-tutorial

```

# create API domain
kubebuilder init --domain tutorial.kubebuilder.io --repo tutorial.kubebuilder.io/controller-runtime-cronjob

# create API
kubebuilder create api --group batch --version v1 --kind CronJob

# Generate WebhookConfiguration, ClusterRole and CustomResourceDefinition objects.
make generate

```
