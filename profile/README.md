# Vita Mojo

Here is Vita Mojo's organisation map

## Table of Contents

- [Infrastructure](#infrastructure)
  - [Kubernetes](#kubernetes)
  - [Observability](#observability)
  - [Databases](#databases)
    - [Redis](#redis)
- [Services](#services)
  - [Backend Services](#backend-services)
  - [Frontend Services](#frontend-services)
  - [Point of Sale](#point-of-sale)
- [CI/CD](#cicd)

## Infrastructure

### Kubernetes

Kubernetes cluster is managed by Deckhouse. There are several ways to access and manage the cluster:

- [Kubernetes Dashboard (Web UI)](https://dashboard.kube.vitamojo.net/)

- [kubeconfig (CLI access)](https://kubeconfig.kube.vitamojo.net)
  Generate and download your kubeconfig file to use with `kubectl` command-line tool.

- [Deckhouse Console (Management interface)](https://console.kube.vitamojo.net/system/management)

- [Deckhouse CLI](https://tools.kube.vitamojo.net/)

**Kubernetes Infrastructure Repository:** [k8s-infra](https://github.com/vita-mojo/k8s-infra)

This repository contains Helm charts and Kubernetes manifests for infrastructure components:

- **backup-restic** - Backup solutions using Restic
- **deckhouse** - Deckhouse module configurations and cluster resources
- **istio** - Istio service mesh configuration
- **knative** - Knative serving configuration
- **knative-operator** - Knative operator deployment
- **madison-nginx-proxy** - Nginx proxy configuration
- **non-managed-resources** - Resources not managed by Helm
- **redis** - Redis Failover cluster deployments
- **redis-operator** - Redis Operator deployment
- **signadot** - Signadot sandbox environment configuration
- **user-authz** - User authorization configuration

### Observability
- [Grafana](https://grafana.kube.vitamojo.net)
- [Prometheus](https://grafana.kube.vitamojo.net/prometheus/graph)
- [Kiali](https://istio.kube.vitamojo.net/console/overview?duration=21600&refresh=60000) (Istio observability dashboard)

### Databases

#### Redis

**Namespace**: `infra-redis-main`

Redis cluster is managed by [Redis Operator](https://github.com/spotahome/redis-operator) using RedisFailover Custom Resource (CR). The operator automatically manages Redis instances through Redis Sentinel.

**Repositories:**
- [Redis Operator](https://github.com/vita-mojo/k8s-infra/tree/main/redis-operator)
- [Redis Failover](https://github.com/vita-mojo/k8s-infra/tree/main/redis)

**Cluster Components:**
- **RFR (Redis Failover Redis)** - Redis itself, a group of instances (master + replicas) with persistent storage
- **RFS (Redis Failover Sentinel)** - Redis Sentinel, which selects the master and performs automatic failover. Sentinel-aware clients can work directly through Sentinel
- **Sentinel Proxy** - proxy for connecting classic Redis clients that don't support Sentinel directly

**Connection Endpoints:**

- **Redis Sentinel Proxy** (for regular Redis clients that **don't support** Sentinel):
  - Internal:
  ```
  redis-sentinel-proxy.infra-redis-main:6379
  ```
  - External LoadBalancer:
  ```
  a4906934f213b4f45938ec1eddb060c5-d57d3c67ce52df3d.elb.eu-west-1.amazonaws.com:6379
  ```
- **Redis Sentinel** (for **sentinel-aware** clients):
  ```
  rfs-redis.infra-redis-main:26379
  ```

To connect you should use [credentials](https://github.com/vita-mojo/k8s-infra/blob/main/redis/.helm/secret-values-main.yaml), which are encrypted with `werf secrets`

## Services

### Backend Services
- [catalog-service](https://github.com/vita-mojo/catalog-service)
- [loyalty-service](https://github.com/vita-mojo/loyalty-service)
- [media-service](https://github.com/vita-mojo/media-service)
- [order-service](https://github.com/vita-mojo/order-service)
- [payment-service](https://github.com/vita-mojo/payment-service)
- [tenant-service](https://github.com/vita-mojo/tenant-service)
- [user-auth-service](https://github.com/vita-mojo/user-auth-service)

### Frontend Services
- [collection-tv-v3](https://github.com/vita-mojo/collection-tv-v3)
- [management-dashboard-v2](https://github.com/vita-mojo/management-dashboard-v2)
- [online-store](https://github.com/vita-mojo/online-store)
- [tenants-management-ui](https://github.com/vita-mojo/tenants-management-ui)

### Point of Sale
- [pos2](https://github.com/vita-mojo/pos2)

## CI/CD

CI/CD pipelines and workflows for the organization are managed through GitHub Actions.

**Repositories:**
- [github-workflows](https://github.com/vita-mojo/github-workflows) - Shared GitHub Actions workflows and reusable pipeline definitions
