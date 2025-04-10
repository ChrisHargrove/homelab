---
tags:
  - k3s
---
A custom resource definition is a way to extend the [[Kubernetes]] API. Like created a custom type of Pod, Deployment or Service.

>[!tip] A.K.A - CRDs

This defines a new api resource that can be used as part of the kubectl command so if you had a custom type called "thingy" you could call
```
kubectl get thingy
```

And it would return all objects of that custom type.

## Why use them
- It allows for the definition and storing of app domain specific information in kubernetes.
- Uses the Kubernetes' reconciliation pattern.
- Allows for the automation of complex infrastructure or app logic in a declarative way.

## Example
```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
	name: thingy.example.com
spec:
	group: example.com
	versions:
		- name: v1
	scope: Namespaced
	names:
		plural: thingies
		singular: thingy
		kind: Thingy
```
