# Kubernetes + Istio Security Lab

## Overview
This project demonstrates layered security in Kubernetes using:
- NetworkPolicy (L3/L4)
- Istio AuthorizationPolicy (L7)

## Architecture
- 3 namespaces: ns1, ns2, ns3
- pod-a (client) in ns1
- pod-b (nginx server) in ns2
- pod-c (client) in ns3

## Key Concepts

### 1. Namespaces
Namespaces provide logical isolation, but do NOT block network traffic.

### 2. Default Behavior
Pods in different namespaces can communicate by default via Service DNS.

### 3. NetworkPolicy
- Enforces L3/L4 traffic rules
- Default deny model
- Allows only ns1 to access pod-b on port 80

Result:
- ns1 → allowed
- ns3 → blocked (timeout)

### 4. Istio AuthorizationPolicy
- Enforces service-to-service authorization
- Based on workload identity

Result:
- ns1 → allowed
- ns3 → blocked (HTTP error)

### 5. Kustomize
- Base configuration shared across environments
- Overlays used for different security scenarios:
  - networkpolicy/
  - istio/

## How to run

### NetworkPolicy demo
kubectl apply -k overlays/networkpolicy

### Istio demo
kubectl apply -k overlays/istio
