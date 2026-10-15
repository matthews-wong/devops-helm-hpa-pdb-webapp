# devops-helm-hpa-pdb-webapp

A small Helm chart for a stateless web service that has to stay up through
node drains and traffic spikes. It bundles the pieces that usually get added
one outage at a time:

- `Deployment` with probes, resource requests/limits and a hardened
  `securityContext`
- `HorizontalPodAutoscaler` (autoscaling/v2) with a scale-down stabilization window
- `PodDisruptionBudget` so voluntary evictions never take every replica
- `NetworkPolicy` that only admits traffic from the ingress namespace
- `values-dev.yaml` / `values-prod.yaml` per environment

## Usage

```bash
helm lint charts/webapp -f charts/webapp/values-dev.yaml
helm template webapp charts/webapp -f charts/webapp/values-prod.yaml | kubeconform -strict -summary
```

Image tags are pinned; the schema rejects `latest`.
