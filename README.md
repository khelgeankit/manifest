apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app-deployment
  namespace: default  # Change to your desired namespace
  labels:
    app: my-app
spec:
  replicas: 3  # Number of desired replicas
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app-container
        image: my-docker-image:latest  # Replace with your Docker image and tag
        ports:
        - containerPort: 80  # Replace with your application’s port
        env:
        - name: ENVIRONMENT
          value: "production"  # Replace with your environment variable
        # Optionally, you can define resource requests and limits
        resources:
          requests:
            memory: "64Mi"
            cpu: "250m"
          limits:
            memory: "128Mi"
            cpu: "500m"
        # Optionally, define liveness and readiness probes
        livenessProbe:
          httpGet:
            path: /healthz
            port: 80
          initialDelaySeconds: 30
          timeoutSeconds: 5
        readinessProbe:
          httpGet:
            path: /readiness
            port: 80
          initialDelaySeconds: 10
          timeoutSeconds: 5
      # Optionally, define volumes and volume mounts
      volumes:
      - name: config-volume
        configMap:
          name: my-app-config  # Replace with your ConfigMap name
      volumeMounts:
      - name: config-volume
        mountPath: /etc/config
