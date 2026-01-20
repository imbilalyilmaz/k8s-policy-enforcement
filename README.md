# Kubernetes Policy Enforcement (DevSecOps)

This project implements a **"Policy as Code"** approach to automate security and operational best practices in a Kubernetes environment using **Kyverno**.

## 🎯 Objectives
1. **Prevent Supply Chain Attacks:** Disallow the use of the `latest` image tag.
2. **Ensure Consistency:** Enforce `imagePullPolicy: Always`.
3. **Hardening Security:** Block containers running in `privileged` (root) mode.
4. **Namespace Isolation:** Restrict access to the `ecommerce` namespace to authorized workloads only.

## 💡 Why It Matters?
In a production Kubernetes environment, default configurations are often too permissive. This project demonstrates a **"Secure by Default"** mindset:

* **Supply Chain Security:** Blocking `:latest` tags ensures deployments are immutable and reproducible. We know exactly what code is running.
* **Least Privilege Principle:** Disallowing `privileged` containers prevents potential attackers from gaining root access to the host node.
* **Multi-Tenancy Isolation:** strict namespace policies ensure that different teams or applications (like `ecommerce`) cannot accidentally interfere with each other.
* **Shift-Left Security:** By integrating **Trivy** in the CI pipeline and **Kyverno** in the cluster, we catch vulnerabilities *before* they reach production.

## 🛠 Tech Stack
- **Kubernetes (K3s)**: Orchestration platform.
- **Kyverno**: Kubernetes Native Policy Management.
- **Trivy**: Security scanner for CI/CD integration.

## 🚀 Usage

### 1. Apply Policies
Deploy the policies to your cluster:
```bash
kubectl apply -f policies/
```

### 2. Verify Enforcement
Attempting to deploy a non-compliant pod will result in an admission error:
```bash
kubectl apply -f tests/bad-deployment.yaml
# Output: Error from server: admission webhook "validate.kyverno.svc-fail" denied the request...
```
To deploy a valid workload:
```bash
kubectl apply -f tests/good-deployment.yaml
```

## 🔄 CI/CD Integration
This repository includes a GitHub Actions workflow (`.github/workflows/policy-check.yaml`) that scans infrastructure code for vulnerabilities using Trivy on every push.

---------------------------------------------------------------------------------------------

*This project was developed to demonstrate DevSecOps best practices on a local K3s cluster.*




