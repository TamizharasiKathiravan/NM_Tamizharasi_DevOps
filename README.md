# Kubernetes RBAC and Network Policies Implementation

## Deployment Instructions

1. Apply the namespace:
   ```bash
   kubectl apply -f rbac/namespace.yaml
   ```

2. Apply RBAC configurations:
   ```bash
   kubectl apply -f rbac/serviceaccount.yaml
   kubectl apply -f rbac/role.yaml
   kubectl apply -f rbac/rolebinding.yaml
   ```

3. Deploy the test pod:
   ```bash
   kubectl apply -f rbac/test-pod.yaml
   ```

4. Apply Network Policies:
   ```bash
   kubectl apply -f network-policies/default-deny-all.yaml
   kubectl apply -f network-policies/allow-internal-traffic.yaml
   kubectl apply -f network-policies/allow-web-traffic.yaml
   kubectl apply -f network-policies/allow-dns.yaml
   ```

## Description

### RBAC
- **Namespace**: `rbac-demo` is created to isolate RBAC configurations.
- **ServiceAccount**: `demo-sa` is assigned limited permissions.
- **Role**: Grants read-only access to pods and pod logs.
- **RoleBinding**: Binds the `demo-sa` to the defined `Role`.
- **Test Pod**: Runs under the `demo-sa` for testing access control.

### Network Policies
- **default-deny-all.yaml**: Denies all ingress and egress by default.
- **allow-internal-traffic.yaml**: Allows traffic within the namespace between all pods.
- **allow-web-traffic.yaml**: Allows ingress traffic to pods with label `role: web` on ports 80 and 443.
- **allow-dns.yaml**: Allows egress DNS queries (UDP port 53) to `kube-dns` in `kube-system` namespace.

## Notes

- Ensure your Kubernetes cluster has a CNI plugin that supports NetworkPolicies (e.g., Calico, Cilium, Weave).
- Use `kubectl describe` and `kubectl logs` for troubleshooting.
- Verify policies using `kubectl exec` and network connectivity tools like `curl`, `wget`, or `nslookup` inside test pods.
