# CKA Practice Exam 1 - Solutions

## Task 1: Pod Troubleshooting

```bash
# Check pod status
kubectl get pod web-app -n production
kubectl describe pod web-app -n production

# Check logs
kubectl logs web-app -n production --previous

# Check events
kubectl get events -n production --sort-by=.lastTimestamp

# Common fixes:
# 1. Fix image name if ImagePullBackOff
# 2. Fix command if crashing
# 3. Increase resource limits if OOMKilled
# 4. Fix volume mounts if issues

# Example fix (if command issue):
kubectl edit pod web-app -n production
# Or delete and recreate with correct config
```

---

## Task 2: Create Deployment

```bash
kubectl create deployment api-server --image=nginx:1.21 --replicas=3

# Add labels
kubectl label deployment api-server app=api tier=backend

# Add resource limits
kubectl set resources deployment api-server \
  --requests=cpu=100m,memory=128Mi \
  --limits=cpu=200m,memory=256Mi

# Or use YAML:
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-server
spec:
  replicas: 3
  selector:
    matchLabels:
      app: api
      tier: backend
  template:
    metadata:
      labels:
        app: api
        tier: backend
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 200m
            memory: 256Mi
```

---

## Task 3: Service Configuration

```bash
kubectl create service clusterip api-service --tcp=80:8080

# Update selector
kubectl patch svc api-service -p '{"spec":{"selector":{"app":"api","tier":"backend"}}}'

# Verify endpoints
kubectl get endpoints api-service
```

Or YAML:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: api-service
spec:
  type: ClusterIP
  selector:
    app: api
    tier: backend
  ports:
  - port: 80
    targetPort: 8080
    protocol: TCP
```

---

## Task 4: ConfigMap and Pod

```bash
# Create ConfigMap
kubectl create configmap app-config \
  --from-literal=database_url=postgresql://db:5432/mydb \
  --from-literal=log_level=info \
  --from-literal=max_connections=100

# Create pod
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: config-pod
spec:
  containers:
  - name: app
    image: busybox:1.35
    command: ['sh', '-c', 'cat /etc/config/* && sleep 3600']
    volumeMounts:
    - name: config-volume
      mountPath: /etc/config
  volumes:
  - name: config-volume
    configMap:
      name: app-config
```

---

## Task 5: Secret Management

```bash
# Create secret
kubectl create secret generic db-credentials \
  --from-literal=username=admin \
  --from-literal=password=secret123

# Create pod
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: db-client
spec:
  containers:
  - name: app
    image: busybox:1.35
    command: ['sh', '-c', 'env && sleep 3600']
    env:
    - name: DB_USER
      valueFrom:
        secretKeyRef:
          name: db-credentials
          key: username
    - name: DB_PASS
      valueFrom:
        secretKeyRef:
          name: db-credentials
          key: password
```

---

## Task 6: PersistentVolume and PVC

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-manual
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: manual
  hostPath:
    path: /mnt/data
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: data-pvc
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
```

```bash
# Verify binding
kubectl get pv pv-manual
kubectl get pvc data-pvc
```

---

## Task 7: Deployment Update

```bash
# Update image
kubectl set image deployment/api-server nginx=nginx:1.22

# Scale
kubectl scale deployment api-server --replicas=5

# Monitor
kubectl rollout status deployment/api-server

# If issues, rollback
kubectl rollout undo deployment/api-server
```

---

## Task 8: RBAC Configuration

```bash
# Create namespace
kubectl create namespace monitoring

# Create ServiceAccount
kubectl create serviceaccount monitor-sa -n monitoring

# Create Role and RoleBinding
```

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: monitoring
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch"]
- apiGroups: [""]
  resources: ["services"]
  verbs: ["get", "list"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: monitor-binding
  namespace: monitoring
subjects:
- kind: ServiceAccount
  name: monitor-sa
  namespace: monitoring
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

```bash
# Verify permissions
kubectl auth can-i get pods --as=system:serviceaccount:monitoring:monitor-sa -n monitoring
```

---

## Task 9: NetworkPolicy

```yaml
# Default deny all
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all
  namespace: production
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
---
# Frontend policy
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: frontend-policy
  namespace: production
spec:
  podSelector:
    matchLabels:
      app: frontend
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - ports:
    - protocol: TCP
      port: 80
  egress:
  - to:
    - podSelector:
        matchLabels:
          app: backend
    ports:
    - protocol: TCP
      port: 8080
---
# Backend policy
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: backend-policy
  namespace: production
spec:
  podSelector:
    matchLabels:
      app: backend
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
    ports:
    - protocol: TCP
      port: 8080
  egress:
  - to:
    - podSelector:
        matchLabels:
          app: database
    ports:
    - protocol: TCP
      port: 3306
---
# Database policy
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: database-policy
  namespace: production
spec:
  podSelector:
    matchLabels:
      app: database
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: backend
    ports:
    - protocol: TCP
      port: 3306
```

---

## Task 10: Ingress Configuration

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web-ingress
spec:
  ingressClassName: nginx
  rules:
  - host: app.example.com
    http:
      paths:
      - path: /api
        pathType: Prefix
        backend:
          service:
            name: api-service
            port:
              number: 80
      - path: /web
        pathType: Prefix
        backend:
          service:
            name: web-service
            port:
              number: 80
      - path: /
        pathType: Prefix
        backend:
          service:
            name: api-service
            port:
              number: 80
```

---

## Task 11: StatefulSet

```yaml
apiVersion: v1
kind: Service
metadata:
  name: db-headless
spec:
  clusterIP: None
  ports:
  - port: 3306
    targetPort: 3306
  selector:
    app: database
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: db-statefulset
spec:
  serviceName: db-headless
  replicas: 3
  selector:
    matchLabels:
      app: database
  template:
    metadata:
      labels:
        app: database
    spec:
      containers:
      - name: mysql
        image: mysql:8.0
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: "rootpass"
        - name: MYSQL_DATABASE
          value: "mydb"
  volumeClaimTemplates:
  - metadata:
      name: data
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: fast-ssd
      resources:
        requests:
          storage: 10Gi
```

---

## Task 12: Job and CronJob

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: data-backup
spec:
  activeDeadlineSeconds: 300
  backoffLimit: 3
  template:
    spec:
      containers:
      - name: backup
        image: busybox:1.35
        command: ['sh', '-c', 'echo "Backup completed" && date']
      restartPolicy: Never
---
apiVersion: batch/v1
kind: CronJob
metadata:
  name: daily-backup
spec:
  schedule: "0 2 * * *"
  successfulJobsHistoryLimit: 3
  failedJobsHistoryLimit: 1
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: backup
            image: busybox:1.35
            command: ['sh', '-c', 'echo "Backup completed" && date']
          restartPolicy: OnFailure
```

---

## Task 13: DaemonSet

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: log-collector
spec:
  selector:
    matchLabels:
      app: log-collector
  template:
    metadata:
      labels:
        app: log-collector
    spec:
      tolerations:
      - key: node-role.kubernetes.io/master
        operator: Exists
        effect: NoSchedule
      - key: node-role.kubernetes.io/control-plane
        operator: Exists
        effect: NoSchedule
      containers:
      - name: fluentd
        image: busybox:1.35
        command: ['sh', '-c', 'echo "Collecting logs" && sleep 3600']
```

---

## Task 14: ResourceQuota

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
  namespace: production
spec:
  hard:
    requests.cpu: "10"
    requests.memory: 20Gi
    limits.cpu: "20"
    limits.memory: 40Gi
    pods: "50"
    persistentvolumeclaims: "10"
```

---

## Task 15: Node Maintenance

```bash
# Cordon
kubectl cordon worker-node-1

# Drain
kubectl drain worker-node-1 --ignore-daemonsets --delete-emptydir-data

# Verify pods rescheduled
kubectl get pods --all-namespaces -o wide | grep -v worker-node-1

# Maintenance (document):
# - Update packages
# - Reboot if needed
# - Hardware maintenance

# Uncordon
kubectl uncordon worker-node-1

# Verify
kubectl get nodes
```

---

## Task 16: Troubleshooting Service

```bash
# Check service
kubectl get svc web-svc -o yaml
kubectl describe svc web-svc

# Check endpoints
kubectl get endpoints web-svc

# Check pod labels
kubectl get pods --show-labels

# Fix selector if mismatch
kubectl patch svc web-svc -p '{"spec":{"selector":{"app":"web"}}}'

# Verify
kubectl get endpoints web-svc
```

---

## Task 17: Pod Security Context

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secure-app
spec:
  securityContext:
    runAsNonRoot: true
    runAsUser: 1000
    seccompProfile:
      type: RuntimeDefault
  containers:
  - name: app
    image: nginx:1.21
    securityContext:
      readOnlyRootFilesystem: true
      allowPrivilegeEscalation: false
      runAsNonRoot: true
      runAsUser: 1000
      capabilities:
        drop:
        - ALL
        add:
        - NET_BIND_SERVICE
    volumeMounts:
    - name: tmp
      mountPath: /tmp
  volumes:
  - name: tmp
    emptyDir: {}
```

---

**End of Solutions**

