# Ingress Configuration and Best Practices for Strava-Like App on GKE

## Overview

This document outlines the recommended ingress configurations and best practices for deploying a Strava-like application on Google Kubernetes Engine (GKE). The ingress setup ensures secure, efficient, and scalable routing of HTTP/HTTPS traffic to the app's microservices.

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
  name: stravalike-ingress
  annotations:
    kubernetes.io/ingress.class: "gce" # Use GCE Ingress Controller for GKE
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
    nginx.ingress.kubernetes.io/proxy-read-timeout: "60"
    nginx.ingress.kubernetes.io/proxy-body-size: "64m"
spec:
  tls:
  - hosts:
    - stravalike.example.com
    secretName: stravalike-tls # TLS Secret for HTTPS
  rules:
  - host: stravalike.example.com
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
