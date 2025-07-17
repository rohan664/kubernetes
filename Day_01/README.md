  NodePort service syntax 
```
apiVersion: v1
kind: Service
metadata:
  name: nodeport-demo
  labels:
    env: production
spec:
  type: NodePort
  selector:
    type: frontend
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30005
      protocol: TCP
```

The Actual Use Cases for NodePort Services
NodePort isn't meant for direct external access in production. Instead, it's primarily used for:

- Debugging/Testing

    1. Quick way to expose a service during development

    2. Example: kubectl expose deploy/my-app --type=NodePort

- Internal Cluster Access

  1. Lets other nodes/pods reach a service using any node's IP + static port

  2. Helpful when LoadBalancer isn't available (e.g., bare metal)

- Building Blocks for Ingress/LoadBalancer

  1. Ingress controllers often use NodePorts internally

  2. Example: MetalLB (LoadBalancer for bare metal) uses NodePort underneath

- Special Network Architectures

  1. When you have external load balancers outside Kubernetes that need fixed ports

  2. Example: On-prem setups with F5 BIG-IP load balancers

Following are port mapping present in NodePort service YAML
- port : service node which is map to pod.
- nodePort : port of the node where service is deploy
- targetPort : port of pod

