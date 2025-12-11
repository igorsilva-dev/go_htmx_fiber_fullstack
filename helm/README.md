# Minimal Helm Chart for Go + Fiber + HTMX

This is a minimal Helm chart with only essential components:
- **Deployment** - Application deployment with security context
- **HPA** - Horizontal Pod Autoscaler for auto-scaling
- **PDB** - Pod Disruption Budget for high availability
- **ConfigMap** - Application configuration

## Installation

### Basic Installation

```bash
helm install my-app ./helm/go-htmx-fiber
```

### With Custom Image Tag

```bash
helm install my-app ./helm/go-htmx-fiber \
  --set image.tag=0.0.2
```

### With Custom Values

```bash
helm install my-app ./helm/go-htmx-fiber \
  --set replicaCount=3 \
  --set autoscaling.enabled=true \
  --set autoscaling.maxReplicas=10
```

## Configuration

### Key Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `replicaCount` | 2 | Number of replicas |
| `image.tag` | latest | Docker image tag |
| `image.pullPolicy` | IfNotPresent | Image pull policy |
| `resources.requests.cpu` | 250m | CPU request |
| `resources.requests.memory` | 128Mi | Memory request |
| `resources.limits.cpu` | 500m | CPU limit |
| `resources.limits.memory` | 256Mi | Memory limit |

### Autoscaling

```bash
helm install my-app ./helm/go-htmx-fiber \
  --set autoscaling.enabled=true \
  --set autoscaling.minReplicas=2 \
  --set autoscaling.maxReplicas=5 \
  --set autoscaling.targetCPUUtilizationPercentage=80
```

### Pod Disruption Budget

By default, the PDB ensures at least 1 pod is available. To disable:

```bash
helm install my-app ./helm/go-htmx-fiber \
  --set podDisruptionBudget.enabled=false
```

### ConfigMap

Add environment variables via ConfigMap:

```bash
helm install my-app ./helm/go-htmx-fiber \
  --set configMap.data.LOG_LEVEL=info \
  --set configMap.data.APP_ENV=production
```

Or in values file:

```yaml
configMap:
  enabled: true
  data:
    LOG_LEVEL: "info"
    APP_ENV: "production"
```

## Upgrade

```bash
helm upgrade my-app ./helm/go-htmx-fiber \
  --set image.tag=0.0.3
```

## Uninstall

```bash
helm uninstall my-app
```

## Verify Installation

```bash
# Check deployment
kubectl describe deployment my-app-go-htmx-fiber

# Check pods
kubectl get pods -l app.kubernetes.io/name=go-htmx-fiber

# Check logs
kubectl logs -l app.kubernetes.io/name=go-htmx-fiber
```
