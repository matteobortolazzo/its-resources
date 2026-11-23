# ITS - Modern DevOps Resources

## DevOps

DevOps is not a tool, it's a mindset. It's about breaking the silos between development and operations.

Here are a few concepts of DevOps. Use official documentation for implementation details and check examples against the versions you run.

A DevOps team should not be a team that does everything, but a team that helps other teams to do everything.

### Learning priorities

Start with a small application and learn to build, deploy, observe, and recover it. Introduce infrastructure complexity when you can explain the requirement it satisfies.

- Trace a request through DNS, TLS, a proxy, application code, and a database.
- Understand processes, permissions, ports, memory limits, and persistent storage well enough to diagnose a failed service.
- Define reliability expectations, collect logs/metrics/traces, and investigate a slow or failing dependency.
- Automate repeatable checks and deployment; demonstrate rollback and backup restoration.
- Use AI to help write configuration and investigate failures. Review its changes and verify behavior in a disposable environment before applying them to shared systems.

These feedback mechanisms also support an [agent development harness](./AI_ML.md#what-is-an-agent-harness). The same build, test, and diagnostic commands should be usable by both people and agents.

### Git

Git is a **distributed** version control system. It's the most used version control system in the world.

#### Concepts

- Branching
- Merge, Fast Forward, Rebase
- Squash
- Cherry-pick
- Stash
- Tag
- Amend (change the last commit)
- Bisect (find the commit that broke the code)
- [Hooks](https://git-scm.com/docs/githooks) (pre-commit, pre-push, post-commit; server-side examples include post-receive)

- Pull request
- Code review
- Pair programming
- Fork

Use diffs to review generated changes and keep commits small enough to explain. Worktrees provide separate working directories for independent tasks; agree on integration ownership when several people or agents work in parallel.

#### Services
- **GitHub**
- **Azure Repos**
- **GitLab**
- **BitBucket**

### CI/CD

CI/CD is the practice of automating the integration and deployment of code to production.

- **CI**: Continuous Integration
- **CD**: Continuous Delivery (keep releases ready to deploy) or Continuous Deployment (automatically deploy changes that pass the required checks)

#### Pipelines 

Usually in CI/CD we use pipeline. Pipeline are a series of steps that are executed in order (or in parallel) after a trigger.

They can be used to:
- Build the code
- Lint the code 
- Test the code (TDD, BDD)
  - Unit tests
  - Mutation Tests
  - Integration Tests
  - End-to-End tests
  - Smoke Tests
  - Performance Tests
  - Regression Tests
  - Code coverage (a signal of what ran, not proof of correct behavior)
- Check vulnerabilities
- Check code quality
- Check outdated dependencies
- Version the code (SemVer, standard version)
- Tag the code
- Generate the documentation
- Package the code (Nuget, NPM, Docker, etc)
- Deploy the code
- Notify the team
- Anything else you can think of

Choose tests around risks and requirements. Include authorization, invalid inputs, dependency failures, and recovery where relevant. Check that a known defect causes the corresponding check to fail, especially when AI generated both the implementation and tests. A skipped or still-running check is not a passing check.

#### Tools

- **GitHub Actions**
- **Azure Pipelines**
- **Jenkins**

### Deployment Techniques

APIs/UIs:
- **Blue-Green Deployment**: Deploy a new version of the code in a new environment*, and switch the traffic when it's ready.
  - It can be a new server, a new container, a new region, etc.
  - It can be done with DNS, Load Balancer, or a Proxy.
- **Canary Deployment**: Deploy a new version of the code to a subset of users, and increase the traffic when it's ready.
- **A/B Testing**: Deploy a new version of the code to a subset of users, and compare the results with the old version.
- **Rolling Deployment**: Deploy a new version of the code to a subset of servers, and increase the traffic when it's ready.
- **Feature Flags**: Deploy a new version of the code to all users, but enable the new features only for a subset of users.
- **Dark Launch**: Deploy a new version of the code to all users, but don't enable the new features yet.

Packages:
- **Nightly Build**: Deploy a new version of the code every nighty
  - Not all test can be done on every commit. Nightly builds are useful to run long tests.

Chaos Engineering:
- Controlled experiments introduce failures to test the system's resilience against a stated hypothesis.
- Introduces latency, errors, and other issues to see how the system reacts.
- It can kill servers, introduce network issues, etc.
- It can be done with tools like Gremlin, or manually.
- Start in a disposable environment, limit the affected scope, and define stop conditions and recovery steps.

### Docker 

Docker is a tool to create, deploy, and run applications by using containers.
Linux containers run processes with isolated views of resources using namespaces, resource controls using cgroups, and a filesystem prepared from an image. Image layers can be shared and reused.
Containers share the kernel of the environment running them; Docker Desktop commonly runs Linux containers inside a Linux VM on macOS and Windows. They often start with less overhead than a full VM, while compatibility and isolation depend on the platform and configuration. See [Docker's container introduction](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/).

A container is not sufficient protection for arbitrary agent commands by itself. Review mounted paths, credentials, network access, privileges, and resource limits when using one as an execution environment.

#### Concepts

- **Image**: A read-only template with instructions for creating a Docker container
- **Container**: A runnable instance of an image
- **Volume**: A way to persist data
- **Network**: A way to connect containers
- **Dockerfile**: A file with instructions to create an image
- **Docker Compose**: A way to define and run multi-container Docker applications
- **Registry**: A place to store and share images (Docker Hub, Azure Container Registry, GitHub Container Registry)
- **ENV**: A way to pass environment variables to the container

### Infrastructure as Code

Infrastructure as Code is the practice of managing infrastructure using code.
Instead of clicking on buttons in a GUI or running commands in a terminal, you write code that defines your infrastructure.

On Azure we can use *ARM* templates or *Bicep*, on *AWS* we can use *CloudFormation*, and on *Google Cloud* we can use Terraform through [Infrastructure Manager](https://cloud.google.com/infrastructure-manager/docs/overview).
More generic tools are *Terraform* and *Pulumi*.

Google Cloud Deployment Manager has [reached end of support](https://docs.cloud.google.com/deployment-manager/docs); use current documentation when choosing a tool. Review infrastructure plans, access permissions, state storage, and recovery steps before applying changes, including AI-generated configuration.

*Bicep* example:

```bicep
resource name_resource 'Microsoft.Web/staticSites@2021-01-15' = {
  name: name
  location: location
  tags: resourceTags
  properties: {
    repositoryUrl: repositoryUrl
    branch: branch
    repositoryToken: repositoryToken
    buildProperties: {
      appLocation: appLocation
      apiLocation: apiLocation
      appArtifactLocation: appArtifactLocation
    }
  }
  sku: {
    tier: sku
    name: skucode
  }
}

resource name_appsettings 'Microsoft.Web/staticSites/config@2021-01-15' = {
  parent: name_resource
  name: 'appsettings'
  properties: appSettings
}
```

With the code above we can create a static website on Azure. The result is reproducible and can be versioned. A second parameter file can be used to change the values.

This excerpt requires definitions for the referenced variables and parameters. Verify API versions and properties against current provider documentation before deploying.

### Monitoring and Alerting

- **Monitoring**: is the practice of observing the system to check its health and performance.
- **Alerting**: is the practice of sending notifications when something goes wrong.

Ideally, you need to find the problem before the user does.

Log things like:
- Errors
- Warnings
- Request timing
- Dependencies timing

Create queries on logs to:
- Find errors
- Find slow requests
- Find the most used features
- Create dashboards
- Create alerts
- Create reports

You can use tools like *Prometheus*, *Grafana*, *Azure Monitor*, *AWS CloudWatch* to create dashboards and alerts.

### OpenTelemetry

OpenTelemetry is a set of APIs, SDKs, and tools for collecting, processing, and exporting telemetry data (metrics, logs, and traces) from your applications. 
It provides a vendor-neutral standard for observability.

#### Key Concepts

- **Traces**: Track requests through your system to understand latency and dependencies
- **Metrics**: Collect numerical measurements about your application (e.g., request count, latency, CPU usage)
- **Logs**: Structured logging with context and correlation IDs
- **Spans**: Individual operations within a trace
- **Exporters**: Send telemetry data to backends (Jaeger, Zipkin, Azure Monitor, DataDog, etc.)

#### Resource
- [OpenTelemetry](https://opentelemetry.io/): A standard for collecting telemetry data
- [Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/overview)
  - [Alerts](https://learn.microsoft.com/en-us/azure/azure-monitor/alerts/alerts-overview)
  - [Dashboard](https://learn.microsoft.com/en-us/azure/azure-monitor/visualize/tutorial-logs-dashboards)
- [Analyze anomalies with KQL](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/kql-machine-learning-azure-monitor)

## App security on Azure 

Security must be considered from the beginning. It's not something you can add later. A security breach can be very expensive or even fatal for a company.

### Azure KeyVault

**NEVER STORE KEYS IN THE REPOSITORY**

Azure KeyVault is a way to store secrets, certificates, and keys. It's a secure way to store sensitive information.

For example:
- Connection strings
- API keys
- Signing Certificates
- SSH keys

> Remember secrets should be rotated, and they should have a limited scope.

### Azure Entra ID

It's a way to manage access to resources. Both users and services can have an identity. It was called *Azure Active Directory* before.

If a service like a WebApp needs to access a database, it can use its identity to authenticate instead of a connection string. It is more secure and easier to manage.

#### Concepts

- **Managed Identity**: An Azure-managed identity for an application or workload to authenticate without managing its credentials. [List of available services](https://learn.microsoft.com/en-us/entra/identity/managed-identities-azure-resources/managed-identities-status)
- **Role-based access control (RBAC)**: A way to manage access to resources based on roles. [What is RBAC](https://learn.microsoft.com/en-us/azure/role-based-access-control/overview)
- **Azure Policy**: A way to enforce rules and standards on resources. [What is Azure Policy](https://learn.microsoft.com/en-us/azure/governance/policy/overview)
- [Using Managed Service Identity (MSI) to authenticate on Azure SQL DB](https://techcommunity.microsoft.com/t5/azure-database-support-blog/using-managed-service-identity-msi-to-authenticate-on-azure-sql/ba-p/1288248)

## Kubernetes

It's a container orchestration platform. It's used to deploy, scale, and manage containers. It let us abstract the distribution of the containers, failover, scaling, and more.

It works well with Microservices, but it's not the only way to do it.

Everything is defined in a YAML file, and it's declarative. Most concepts are similar to [Docker Compose](https://docs.docker.com/compose/), but they are more complex.

Choose Kubernetes when its scheduling, scaling, and operational capabilities justify the cost of running it. Compare with a managed application platform or a simpler container deployment for your workload.

### Basic Concepts

- **Node**: A server that runs containers
- **Cluster**: A set of nodes that run containerized applications
- **Pod**: A group of one or more containers, smallest deployable unit
- **Service**: A way to abstract a set of pods, load balance them, and more
- **Deployment**: A way to deploy a pod, and manage its replicas
- **Ingress**: A way to expose a service to the outside world. It can be used to route traffic, add SSL, and more
- **Namespace**: A scope for naming and organizing resources. Tenant isolation also requires authorization, network policy, and other controls; see [Kubernetes multi-tenancy](https://kubernetes.io/docs/concepts/security/multi-tenancy/).
- **ConfigMap**: A way to store configuration
- **Secret**: An object for sensitive configuration; access controls and encryption at rest must be configured appropriately.
- **Volume**: A way to persist data

### Example

[Documentation](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)

The image reference below is a placeholder. Replace it with a supported image pinned to a verified digest before applying this manifest.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: YOUR_REGISTRY/nginx@sha256:YOUR_VERIFIED_DIGEST
        ports:
        - containerPort: 80
```

## Helm

Helm is a package manager for *Kubernetes*. It's used to define, install, and upgrade *Kubernetes* applications.

It's like *Nuget* or *NPM* for Kubernetes.

### Concepts

- **Chart**: A package of pre-configured Kubernetes resources
- **Repository**: A place to store and share charts
- **Release**: A specific version of a chart
- **Values**: A way to customize a chart
