# Ingress Configuration and Best Practices for Trackme.ai App on GKE

## Overview

This document outlines the recommended ingress configurations and best practices for deploying this application on Google Kubernetes Engine (GKE). The ingress setup ensures secure, efficient, and scalable routing of HTTP/HTTPS traffic to the app's microservices.

---

## Core Components

- **Ingress Resource**: Defines routing rules for HTTP/HTTPS traffic.
- **Ingress Controller**: Implements the ingress resource and manages the flow of traffic (e.g., GKE-native HTTP(S) Load Balancer).
- **TLS Certificates**: Secures communication using HTTPS.
- **Annotations**: Customizes ingress behavior for specific use cases.

---

## Ingress Rules for Microservices

The following ingress configuration routes traffic to the application's microservices based on the URL paths.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: trackme.ai-ingress
  annotations:
    kubernetes.io/ingress.class: "gce" # Use GCE Ingress Controller for GKE
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
    nginx.ingress.kubernetes.io/proxy-read-timeout: "60"
    nginx.ingress.kubernetes.io/proxy-body-size: "64m"
spec:
  tls:
  - hosts:
    - trackme.ai.example.com
    secretName: trackme.ai-tls # TLS Secret for HTTPS
  rules:
  - host: trackme.ai.example.com
    http:
      paths:
      - path: /auth
        pathType: Prefix
        backend:
          service:
            name: auth-service
            port:
              number: 80
      - path: /activity
        pathType: Prefix
        backend:
          service:
            name: activity-service
            port:
              number: 80
      - path: /feed
        pathType: Prefix
        backend:
          service:
            name: feed-service
            port:
              number: 80
      - path: /notifications
        pathType: Prefix
        backend:
            name: notification-service
            port:
              number: 80

```
## Best Practices
1. Use HTTPS for Secure Communication
Obtain TLS certificates using Cert-Manager or Let's Encrypt.
Define the tls section in the ingress resource to enforce HTTPS.
2. Leverage Annotations for Custom Behavior
Use annotations to customize ingress behavior, such as enabling SSL redirection and setting timeouts.
3. Enable Autoscaling for Traffic Spikes
Configure Horizontal Pod Autoscalers (HPA) for backend services to handle sudden traffic surges.
4. Distribute Traffic Across Zones
Deploy services in a regional cluster to ensure high availability.

