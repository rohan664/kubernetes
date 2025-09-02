# How pods are schedule on nodes

There are server methods to schedule pods on nodes:

1. Selector (base on labels).
2. Tainst and Tolerations.
3. Affinity and Anti-affinity rule.

for affinity and anti-affinity rule : Read Blog https://medium.com/@danielaaronw/k8s-pod-anti-affinity-dd2667a20c5f

### Tainst and Tolerations
Node affinity is a property of Pods that attracts them to a set of nodes (either as a preference or a hard requirement). Taints are the opposite -- they allow a node to repel a set of pods.

Tolerations are applied to pods. Tolerations allow the scheduler to schedule pods with matching taints. Tolerations allow scheduling but don't guarantee scheduling: the scheduler also evaluates other parameters as part of its function.

Taints and tolerations work together to ensure that pods are not scheduled onto inappropriate nodes. One or more taints are applied to a node; this marks that the node should not accept any pods that do not tolerate the taints.

