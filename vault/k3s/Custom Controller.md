---
tags:
  - k3s
---
These are effectively control loops in [[Kubernetes]] thye have the task of watching kubernetes objects such as Pods, Deployments or [[Custom Resource Definition|CRDs]]. When it sees that something has changed it tries to react and move the system back to the desired state.

> [!warning]
> It should be noted that a controller is not necessarily an [[Custom Operators|operators]], all operators are controllers but not all controllers are operators.

Primarily a custom controller contains logic that defines what should happen when a [[Custom Resource Definition|CRD]] is created, updated or deleted. They are often built using the Kubernetes SDK.

Internally they operate similar to this:
```psuedo
loop {
	// Get all Custom Resources
	// Compare their spec to the actual current state
	// Take actions ( eg. create pods, update configs, etc) to match the spec.
}
```

## Example
If you had a CRD called Database, the controller might:
- See that a new database CRD was created
- Automatically spin up a PostgresQL instance or call an external service to provision it.
- Monitor it and then clean it up when the Custom Resource is deleted.

## Real Life Example
- cert-manager: Watches Certificate CRs and manages TLS certs for you.
- ArgoCD: watches Application CRs and syncs Git Repositories to Kubernetes.
- Prometheus Operator: watches ServiceMonitor CRs and sets up Prometheus scaping accordingly.
