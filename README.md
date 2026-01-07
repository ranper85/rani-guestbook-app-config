
---

# Guestbook Application – Deployment (GitOps) Repository

This repository contains the **deployment configuration** for the Guestbook project.  
It is used to deploy and manage the application on an OpenShift cluster using
**GitOps with Argo CD**.

The application source code and CI pipelines are managed in a separate repository.

**Application repository:**  
https://github.com/ranper85/rani-guestbook-app

---

## Purpose of This Repository

The purpose of this repository is to manage the **deployment and runtime configuration**
of the Guestbook application.

It is responsible for:
- OpenShift deployment manifests
- Services and Routes
- Persistent storage configuration
- ConfigMaps and application configuration
- Argo CD Application configuration

This repository **does not contain application source code** and does not build
container images.

---

## Repository Structure

```text
.
└── k8s/
    ├── frontend deployment and service
    ├── backend deployment and service
    ├── postgres deployment, service, and PVC
    ├── redis deployment and service
    ├── ConfigMaps
    └── Routes
```
---
## Deployed Components

### Frontend

The frontend is deployed as a containerized application and exposed externally
using an OpenShift Route.
It serves the web interface and forwards API requests to the backend service.

### Backend

The backend is deployed as a separate service and provides a REST API for
guestbook entries.
It communicates with PostgreSQL and Redis inside the cluster.

### PostgreSQL

PostgreSQL is used for persistent storage of guestbook entries.
PersistentVolumeClaims ensure that data is not lost when pods restart.

### Redis

Redis is used as a supporting cache/service for the backend.

---
## GitOps Deployment Workflow

The deployment follows GitOps principles:

1. Changes are made only in this Git repository

2. Argo CD continuously monitors the repository

3. Detected changes are applied automatically

4. The cluster state is kept in sync with Git

Manual changes in the cluster are reverted by Argo CD.

---
## Configuration Highlights

### ConfigMap for Nginx API Proxy

A ConfigMap is used to configure Nginx so that requests to /api/...
are forwarded to the backend service.

### Health Checks

Liveness and readiness probes are configured to improve stability
and availability.

### Persistence

Persistent storage is configured for PostgreSQL to ensure data durability.

---
## Secrets Management

Secrets for PostgreSQL and Redis are created manually using the OpenShift CLI.
Secrets are not stored in Git.

More advanced secret management solutions were not implemented, as this is a lab environment.

---
## Testing and Verification

The following tests were performed:

1. External access via OpenShift Route

2. Data persistence verification

3. Replica scaling using Git commits

4. Automatic deployment after source code changes

All tests confirmed that the GitOps workflow functions as intended.

---
## Conclusion

This repository demonstrates how GitOps and Argo CD can be used to manage
application deployments in a reliable and automated way.
Git acts as the single source of truth, and Argo CD ensures that the cluster
always matches the desired state defined in this repository.
