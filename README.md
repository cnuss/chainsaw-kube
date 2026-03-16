# Kubernetes KUTTL Conformance Tests

Declarative end-to-end tests for core Kubernetes functionality using [KUTTL](https://kuttl.dev/) v0.25.

## What This Tests

| Test | Area | Description |
|------|------|-------------|
| `pod-lifecycle` | Pods | Create, run, and delete a pod |
| `deployment-lifecycle` | Deployments | Create, scale, and rolling-update a deployment |
| `service-clusterip` | Services | ClusterIP service with endpoint verification |
| `service-nodeport` | Services | NodePort service with endpoint verification |
| `configmap-mount` | ConfigMaps | Mount a ConfigMap in a pod and verify data |
| `secret-mount` | Secrets | Mount a Secret in a pod and verify data |
| `namespace-lifecycle` | Namespaces | Create and delete a namespace |
| `job-completion` | Jobs | Run a Job to completion |
| `cronjob-scheduling` | CronJobs | Create a CronJob and verify it spawns Jobs |
| `daemonset` | DaemonSets | Verify pods scheduled on all nodes |
| `statefulset` | StatefulSets | Ordered pod creation and stable identity |
| `persistent-volume-claim` | PVCs | Create a PVC and bind it to a pod |
| `rbac` | RBAC | ServiceAccount, Role, RoleBinding access verification |
| `network-policy` | NetworkPolicies | Create and verify network policy resources |
| `resource-quota` | ResourceQuotas | Quota enforcement on a namespace |
| `limit-range` | LimitRanges | Default resource limits applied to pods |
| `horizontal-pod-autoscaler` | HPA | HPA targeting a deployment |
| `ingress` | Ingress | Ingress resource creation and acceptance |

## Prerequisites

- `kubectl` configured with cluster access
- [kuttl](https://kuttl.dev/) v0.25+ installed
- A running Kubernetes cluster (Kind, k3s, minikube, etc.)

## Running Tests

Run all tests with an ephemeral Kind cluster:

```bash
kubectl kuttl test --config tests/kuttl-test.yaml --start-kind
```

Run all tests against an existing cluster:

```bash
kubectl kuttl test --config tests/kuttl-test.yaml
```

Run a single test:

```bash
kubectl kuttl test --config tests/kuttl-test.yaml --test pod-lifecycle
```

## Design Principles

- **Self-contained**: Each test uses KUTTL's automatic namespace management. No cross-test dependencies.
- **Lightweight images**: Tests use `busybox` and `nginx` to minimize pull times.
- **Subset assertions**: Only fields that matter are asserted. No over-specification.
- **Realistic timeouts**: Defaults to 60s suite-wide; individual steps override where needed.
- **Kind-compatible**: Every test passes on a standard Kind cluster with no special configuration.
