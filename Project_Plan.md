# Project Plan: Sprint #3 (DevOps with Kubernetes & CI/CD)

## 1. Problem Statement & Solution Overview
Teams often deploy applications manually, leading to downtime, configuration drift, and slow, error-prone releases. The lack of an automated pipeline causes delays in software delivery, while traditional manual hosting does not offer the resilience, scaling, and operational efficiency required by modern applications.

**Solution Overview**: Our solution is a fully automated CI/CD pipeline that builds Docker images of our Node.js app, tests them, and deploys them to a Google Kubernetes Engine (GKE) cluster, enabling zero-downtime rolling updates. Kubernetes ensures scalability and resilience by restarting failed containers, while GitHub Actions CI/CD ensures fast, reliable, and repeatable delivery workflows.

## 2. Scope & Boundaries
To keep the project focused and achievable within the sprint, the scope boundaries are strictly defined.

**In Scope**
- Dockerizing an existing web application (Node.js/Express)
- CI pipeline (Automated npm build + test)
- CD pipeline (Building Docker image & Push to Docker Hub)
- Kubernetes manifests (Deployment.yaml, Service.yaml)
- Cloud-based cluster deployment (Google Kubernetes Engine - GKE)

**Out of Scope**
- Complex microservices architecture
- Service mesh implementations (Istio, Linkerd)
- Advanced monitoring stacks (Prometheus + Grafana)
- Autoscaling based on custom metrics (HPA will not be implemented for the MVP)

## 3. Roles & Responsibilities
Each team member holds distinct ownership to ensure smooth execution.

| Role | Team Member | Key Responsibilities |
| :--- | :--- | :--- |
| **CI/CD Lead** | — | GitHub Actions workflows, build automation, CD deployment testing to registry |
| **Kubernetes Lead** | — | GKE Cluster setup, Kubernetes manifests (Deployments, Services) |
| **Containerization & QA** | — | Dockerfiles, image optimization, local application tests, deployment validation |

## 4. Sprint Timeline (4 Weeks)
The sprint is divided into clear weekly milestones.

| Week | Focus Area | Deliverables / Milestones |
| :--- | :--- | :--- |
| **Week 1** | DevOps Foundations | Develop Node.js app, Dockerize application, container registry setup. |
| **Week 2** | Kubernetes Basics | GKE cluster setup, Deployment & Service manifests created locally. |
| **Week 3** | CI/CD Integration | GitHub Actions CD pipeline, automated Docker Hub publishing. |
| **Week 4** | Stabilization & Demo | Rolling deployments, failure recovery testing, complete documentation, demo prep. |

### Deployment & Testing Plan
- **Testing Strategy**: 
  - CI pipeline automatically validates builds and dependencies upon GitHub push.
  - The container is run locally first, then inside the test K8s cluster.
  - Manual validation of rolling updates is performed by watching Pod statuses during a code change.
  - Failure simulation is conducted by actively deleting a Pod (`kubectl delete pod`) to verify automated redeploy.
- **Deployment Steps**:
  1. Commit code to GitHub `main` branch.
  2. GitHub Actions CI/CD builds the Docker image and runs unit tests.
  3. CI/CD pushes the passed image to Docker Hub.
  4. Deployment is executed by applying the Kubernetes manifests (`kubectl apply`).
  5. Validate the load balancer service endpoint is online and functioning post-deployment.

## 5. MVP (Minimum Viable Product)
Our DevOps MVP securely demonstrates a complete automated delivery flow:
- A completed containerized Node.js application.
- A CI pipeline triggered directly on a GitHub code push.
- The latest built Image is automatically published to Docker Hub.
- The Application is deployed to Kubernetes (GKE) and is publically accessible via a LoadBalancer service endpoint.
- Rolling updates can be performed seamlessly without experiencing any user downtime.

## 6. Functional Requirements
- The CI pipeline MUST build and validate Docker images on every push to `main`.
- The CD pipeline MUST deploy the validated image to the container registry automatically.
- The Kubernetes cluster MUST restart failed application containers using liveness scopes.
- The application MUST return a dynamic message and a `/health` endpoint for validation.
- The application MUST remain responsive via the load balancer during rolling updates.

## 7. Non-Functional Requirements
- **Reliability**: Application recovers from individual node or pod failures automatically.
- **Scalability**: K8s Deployment natively supports multiple application replicas (configured for 2 replicas).
- **Consistency**: The precise image built in CI is identical across all testing and production environments.
- **Automation**: There are zero manual deployment steps between pushing application code and image publication.

## 8. Success Metrics
- 100% automated build-and-push pipeline success rate on healthy code.
- Successful resolution and application reachability on a generated HTTP public IP.
- Zero downtime experienced by users during a CI/CD-triggered rolling K8s update.
- Complete set of documentation created and final demo is technically sound and reproducible.

## 9. Risks & Mitigation
| Risk | Impact | Mitigation |
| :--- | :--- | :--- |
| Kubernetes concepts are complex | Slow progress | Use minimal standard manifests. Avoid advanced K8s features (StatefulSets, PVs). |
| CI/CD pipeline failures (secrets) | Blocked deployment | Test pipeline early; validate Github Secrets for Docker Hub properly upfront. |
| Cloud setup & networking issues | Delayed demo | Validate cloud service quotas, verify GKE cluster access and permissions by Week 1. |
