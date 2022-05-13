# All versions of policy co-existing on a single Kubernetes Cluster

## Demo

```bash
# Create a cluster
$ kind create cluster
Creating cluster "kind" ...
 ✓ Ensuring node image (kindest/node:v1.23.4) 🖼
 ✓ Preparing nodes 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
Set kubectl context to "kind-kind"
You can now use your cluster with:

kubectl cluster-info --context kind-kind

Thanks for using kind! 😊

# Install kyverno
$ kubectl apply --wait -k github.com/kyverno/kyverno/config
namespace/kyverno created
customresourcedefinition.apiextensions.k8s.io/clusterpolicies.kyverno.io created
...[etc]...
deployment.apps/kyverno created

# Apply Policy 1.0.0
$ kubectl apply -k "github.com/policy-as-versioned-code/policy/kubernetes/kyverno?ref=1.0.0"
clusterpolicy.kyverno.io/require-department-label-1.0.0 created

# Apply Policy 2.0.0
$ kubectl apply -k "github.com/policy-as-versioned-code/policy/kubernetes/kyverno?ref=2.0.0"
clusterpolicy.kyverno.io/require-department-label-2.0.0 created
clusterpolicy.kyverno.io/require-known-department-label-2.0.0 created

# Apply Policy 2.1.0
$ kubectl apply -k "github.com/policy-as-versioned-code/policy/kubernetes/kyverno?ref=2.1.0"
clusterpolicy.kyverno.io/require-department-label-2.1.0 created
clusterpolicy.kyverno.io/require-known-department-label-2.1.0 created

# Apply Policy 2.1.1
$ kubectl apply -k "github.com/policy-as-versioned-code/policy/kubernetes/kyverno?ref=2.1.1"
clusterpolicy.kyverno.io/require-department-label-2.1.1 created
clusterpolicy.kyverno.io/require-known-department-label-2.1.1 created

# Deploy app1
$ kubectl apply -k github.com/policy-as-versioned-code/app1
deployment.apps/app1 created

# Deploy app2
$ kubectl apply -k github.com/policy-as-versioned-code/app2
deployment.apps/app2 created

# Deploy app3
$ kubectl apply -k github.com/policy-as-versioned-code/app3
deployment.apps/app3 created

# Check all apps are deployed
$ kubectl wait --for=condition=available --timeout=600s \
  deployment/app1 \
  deployment/app2 \
  deployment/app3
deployment.apps/app1 condition met
deployment.apps/app2 condition met
deployment.apps/app3 condition met
```