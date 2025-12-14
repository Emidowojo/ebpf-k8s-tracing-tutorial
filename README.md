# eBPF Kubernetes Tracing Tutorial

Learn how to debug Kubernetes applications using eBPF tracing when traditional logs and metrics fail you.

## 🚀 Quick Start

### Prerequisites

- Kubernetes 1.19+ cluster (local or cloud)
- kubectl installed and configured
- Cluster admin permissions
- Linux kernel 5.10+

### 1. Install Inspektor Gadget
```bash
# Install kubectl gadget plugin
kubectl krew install gadget

# Deploy to cluster
kubectl gadget deploy

# Verify
kubectl get pods -n gadget
```

> **Don't have krew?** [Install it here](https://krew.sigs.k8s.io/docs/user-guide/setup/install/)

### 2. Deploy Demo Application
```bash
kubectl create namespace demo-app
kubectl apply -f manifests/demo-app.yaml

# Wait for pods
kubectl wait --for=condition=ready pod -l app=frontend -n demo-app --timeout=300s
kubectl wait --for=condition=ready pod -l app=productcatalog -n demo-app --timeout=300s
```

### 3. Access the Application

**Local cluster:**
```bash
kubectl port-forward -n demo-app service/frontend 8080:80
```
Then visit: http://localhost:8080

**Cloud cluster:**
```bash
kubectl get service frontend -n demo-app
# Use the EXTERNAL-IP
```

### 4. Start Tracing
```bash
# Trace request loops
kubectl gadget traceloop -n demo-app
```

## 📚 What's Included

### Manifests

- **`demo-app.yaml`** - Main demo application (frontend + product catalog)
- **`demo-app-with-bug.yaml`** - Version with intentional bugs for debugging practice
- **`demo-app-slow-backend.yaml`** - Simulates slow backend for performance debugging

### Examples

Sample trace outputs showing:
- Healthy request flow
- Slow endpoint patterns
- Failed connection scenarios

## 🔍 Common Use Cases

### Find Slow Endpoints
```bash
kubectl gadget traceloop -n demo-app --podname frontend
curl http://localhost:8080/products
```

### Debug Connection Failures
```bash
kubectl gadget trace_tcp -n demo-app
# Observe connection patterns
```

### Map Service Dependencies
```bash
kubectl gadget trace_tcp -n demo-app
```

### Monitor Process Execution
```bash
kubectl gadget trace_exec -n demo-app
```

### Trace DNS Queries
```bash
kubectl gadget trace_dns -n demo-app
```

## 🧹 Cleanup

```bash
kubectl delete namespace demo-app
kubectl gadget undeploy
```

## 📝 License

MIT License - see [LICENSE](LICENSE)

## 🔗 Resources

- [Inspektor Gadget Documentation](https://www.inspektor-gadget.io/docs/)
- [eBPF.io](https://ebpf.io)
- [CNCF Observability Resources](https://www.cncf.io/blog/)

## 👤 Author

**Emidowojo Opaluwa**  
- LinkedIn: [Emidowojo Opaluwa](https://www.linkedin.com/in/emidowojo/)
- Twitter: [@Emidowojo](https://twitter.com/Emidowojo)

---

⭐ If this tutorial helped you, please star the repo!
