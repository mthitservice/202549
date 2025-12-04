# 202549
Docker Aufbau Kurs

>Trainer: Michael Lindner

## Über Container-Standards

Container-Technologien wie Docker basieren auf offenen Standards, die von der **[Open Container Initiative (OCI)](https://opencontainers.org/)** definiert und verwaltet werden. Die OCI wurde 2015 gegründet und ist ein Projekt der Linux Foundation.

**Die OCI spezifiziert:**

- 📦 **Image Specification** - Format und Aufbau von Container-Images
- 🚀 **Runtime Specification** - Wie Container ausgeführt werden
- 📊 **Distribution Specification** - Verteilung von Container-Images über Registries

Dies gewährleistet Interoperabilität zwischen verschiedenen Container-Plattformen (Docker, Podman, containerd, CRI-O, etc.) und verhindert Vendor Lock-in.

**Weitere Informationen:**

- [OCI Official Website](https://opencontainers.org/)
- [OCI Image Specification](https://github.com/opencontainers/image-spec)
- [OCI Runtime Specification](https://github.com/opencontainers/runtime-spec)

---

## Modul 01 - Wiederholung

📁 [Zum Modul-Verzeichnis](modul01/)  
📖 Vorbereitung eines Ubuntu Betriebssystems [zum Inhalt](modul01/readme.md)  
📖 Die wichtigsten Docker Befehle und Beispiele [zum Inhalt](modul01/readme-docker.md)

## Modul 02 - Dockerfile Basics

📁 [Zum Modul-Verzeichnis](modul02/)

Statische Website veröffentlichen mit einem Image, welches einen Webserver in sich trägt

**Inhalt:**

- Nginx-Webserver als Basis-Image
- HTML-Dateien in Container kopieren
- Einfaches Dockerfile erstellen und builden

## Modul 03 - Dockerfile Core Image

📁 [Zum Modul-Verzeichnis](modul03/)

Image-Aufbau von einem Core-Image und einfacher Command-Ausführung

**Inhalt:**

- Minimale Linux-Basis-Images (Alpine, Ubuntu Core)
- Shell-Befehle im Container ausführen
- CMD vs. ENTRYPOINT verstehen

## Modul 04 - Dockerfile Python Anwendung

📁 [Zum Modul-Verzeichnis](modul04/)

Image-Aufbau von einem Framework-Image und Ergänzung von Paketen bis zum Start einer eigenen Python-Anwendung

**Inhalt:**

- Python-Basis-Image verwenden
- Requirements installieren (pip)
- Python-Applikation containerisieren
- Umgebungsvariablen und Port-Mapping

## Modul 05 - Docker Compose Basics

📁 [Zum Modul-Verzeichnis](modul05/)

Einführung in Docker Compose mit Multi-Container-Setup

**Inhalt:**

- docker-compose.yml Syntax
- Mehrere Services orchestrieren
- Service-Skalierung mit replicas
- Grundlagen der Container-Orchestrierung

## Modul 06 - Multi-Tier Web Application

📁 [Zum Modul-Verzeichnis](modul06/)  
📖 [Zur Dokumentation](modul06/readme.md)

Komplexe Multi-Container-Anwendung mit Reverse Proxy, Monitoring und TLS

**Inhalt:**

- Frontend und Backend Services
- Nginx als Reverse Proxy
- Prometheus für Monitoring
- Grafana für Visualisierung
- SSL/TLS-Zertifikate einbinden
- Docker Networks und Volumes

## Modul 07 - GitLab Container Registry

📁 [Zum Modul-Verzeichnis](modul07/)  
📖 [Zur Dokumentation](modul07/readme.md)

GitLab CE als Container-Registry und CI/CD-Plattform

**Inhalt:**

- CI/CD-Systeme im Enterprise-Umfeld
- Zentrale Image- und Package-Repositories
- On-Premise Absicherung
- Security Operations (SecOps) Integration
- GitLab Community Edition deployen
- Container Registry konfigurieren
- Health Checks implementieren
- Volumes für Persistenz
- Port-Konfiguration und Networking

## Modul 08 - Docker Swarm

📁 [Zum Modul-Verzeichnis](modul08/)  
📖 [Swarm-Befehlsreferenz](modul08/readme-swarm.md)

Container-Orchestrierung mit Docker Swarm

**Inhalt:**

- Swarm-Cluster aufsetzen (Manager + Worker Nodes)
- Services im Cluster deployen
- Skalierung und Load Balancing
- High Availability und Failover

**Weiterführende Orchestrierungs-Technologien:**

- 🚀 **[Kubernetes](https://kubernetes.io/)** - Industry-Standard für Container-Orchestrierung
  - Cloud Native Computing Foundation (CNCF) Projekt
  - Umfangreicher als Swarm, komplexere Features
  - Große Community und Ecosystem
  - Ideal für große, komplexe Produktionsumgebungen
  
- 🎯 **[Red Hat OpenShift](https://www.redhat.com/de/technologies/cloud-computing/openshift)** - Enterprise Kubernetes Platform
  - Kubernetes + zusätzliche Enterprise-Features
  - Integrierte CI/CD, Registry, Monitoring
  - Developer & Operations Tools
  - Support und Security Hardening von Red Hat
  - Ideal für regulierte Branchen und Enterprise-Umgebungen

---

## Literaturverzeichnis & Wichtige Unternehmen

### Container-Plattformen & Runtimes

#### Docker

- [Docker Inc.](https://www.docker.com/) - Ursprünglicher Entwickler der Docker-Technologie
- [Docker Hub](https://hub.docker.com/) - Größtes öffentliches Container-Registry
- [Docker Documentation](https://docs.docker.com/) - Offizielle Dokumentation

#### Alternative Container-Runtimes

- [Podman (Red Hat)](https://podman.io/) - Daemonless Container Engine
- [containerd (CNCF)](https://containerd.io/) - Industry-Standard Container Runtime
- [CRI-O (CNCF)](https://cri-o.io/) - Lightweight Container Runtime für Kubernetes

### Orchestrierung

#### Kubernetes Ecosystem

- [Kubernetes (CNCF)](https://kubernetes.io/) - Container-Orchestrierung
- [Cloud Native Computing Foundation (CNCF)](https://www.cncf.io/) - Kubernetes und Cloud-Native Projekte
- [Red Hat OpenShift](https://www.openshift.com/) - Enterprise Kubernetes Platform
- [Rancher (SUSE)](https://www.rancher.com/) - Multi-Cluster Kubernetes Management
- [VMware Tanzu](https://tanzu.vmware.com/) - Kubernetes Platform für Enterprises

#### Weitere Orchestrierung

- [Docker Swarm](https://docs.docker.com/engine/swarm/) - Native Docker Orchestrierung
- [Nomad (HashiCorp)](https://www.nomadproject.io/) - Workload Orchestrator

### Container Registries

#### Cloud Provider

- [Amazon Elastic Container Registry (ECR)](https://aws.amazon.com/ecr/) - AWS Container Registry
- [Azure Container Registry (ACR)](https://azure.microsoft.com/services/container-registry/) - Microsoft Azure Registry
- [Google Container Registry (GCR)](https://cloud.google.com/container-registry) - Google Cloud Registry
- [GitHub Container Registry](https://docs.github.com/packages/working-with-a-github-packages-registry/working-with-the-container-registry) - GitHub integrierte Registry

#### Self-Hosted & Enterprise

- [Harbor (CNCF)](https://goharbor.io/) - Open Source Enterprise Registry
- [JFrog Artifactory](https://jfrog.com/artifactory/) - Universal Artefakt Repository
- [Sonatype Nexus](https://www.sonatype.com/products/nexus-repository) - Repository Manager
- [GitLab Container Registry](https://docs.gitlab.com/ee/user/packages/container_registry/) - In GitLab integriert
- [Quay.io (Red Hat)](https://quay.io/) - Container Registry mit Security-Scanning

### CI/CD & DevOps Platforms

- [GitLab](https://about.gitlab.com/) - Complete DevOps Platform
- [Jenkins](https://www.jenkins.io/) - Open Source Automation Server
- [GitHub Actions](https://github.com/features/actions) - CI/CD direkt in GitHub
- [Azure DevOps](https://azure.microsoft.com/services/devops/) - Microsoft DevOps Platform
- [CircleCI](https://circleci.com/) - Cloud-native CI/CD
- [Travis CI](https://www.travis-ci.com/) - CI/CD Platform
- [Argo CD](https://argo-cd.readthedocs.io/) - GitOps CD für Kubernetes

### Security & Compliance

- [Aqua Security](https://www.aquasec.com/) - Container Security Platform
- [Snyk](https://snyk.io/) - Developer Security & Vulnerability Scanning
- [Sysdig](https://sysdig.com/) - Container Security & Monitoring
- [Twistlock (Palo Alto Networks)](https://www.paloaltonetworks.com/prisma/cloud) - Cloud Native Security
- [Anchore](https://anchore.com/) - Container Security & Compliance
- [Trivy (Aqua Security)](https://trivy.dev/) - Open Source Vulnerability Scanner

### Monitoring & Observability

- [Prometheus](https://prometheus.io/) - Monitoring System
- [Grafana](https://grafana.com/) - Observability & Visualization
- [Datadog](https://www.datadoghq.com/) - Monitoring & Analytics
- [Elastic Stack (ELK)](https://www.elastic.co/) - Logging & Analytics
- [Splunk](https://www.splunk.com/) - Data Platform
- [New Relic](https://newrelic.com/) - Application Performance Monitoring

### Cloud Provider Container-Services

#### Container-Services der großen Cloud-Anbieter

- [AWS (Amazon Web Services)](https://aws.amazon.com/containers/) - ECS, EKS, Fargate
- [Microsoft Azure](https://azure.microsoft.com/solutions/containers/) - AKS, ACI, Container Apps
- [Google Cloud](https://cloud.google.com/containers) - GKE, Cloud Run, Anthos
- [IBM Cloud](https://www.ibm.com/cloud/container-service) - Kubernetes Service
- [Oracle Cloud](https://www.oracle.com/cloud/compute/container-engine-kubernetes/) - Container Engine

### Standards & Organisationen

- [Open Container Initiative (OCI)](https://opencontainers.org/) - Container Standards
- [Cloud Native Computing Foundation (CNCF)](https://www.cncf.io/) - Cloud Native Projekte
- [Linux Foundation](https://www.linuxfoundation.org/) - Open Source Projekte
- [Docker, Inc.](https://www.docker.com/company/) - Docker Entwickler
- [Container Storage Interface (CSI)](https://github.com/container-storage-interface/spec) - Storage Standard

### Weitere wichtige Projekte

#### Networking

- [Calico](https://www.tigera.io/project-calico/) - Container Networking
- [Cilium](https://cilium.io/) - eBPF-based Networking
- [Istio](https://istio.io/) - Service Mesh
- [Linkerd (CNCF)](https://linkerd.io/) - Service Mesh

#### Storage

- [Rook (CNCF)](https://rook.io/) - Cloud-Native Storage
- [Longhorn (CNCF)](https://longhorn.io/) - Distributed Block Storage
- [Portworx](https://portworx.com/) - Container Storage Solution

#### Build Tools

- [Buildah](https://buildah.io/) - OCI Container Image Builder
- [Skopeo](https://github.com/containers/skopeo) - Container Image Tool
- [Kaniko (Google)](https://github.com/GoogleContainerTools/kaniko) - Container Builder

#### Package Manager

- [Helm](https://helm.sh/) - Kubernetes Package Manager
- [Kustomize](https://kustomize.io/) - Kubernetes Configuration Management

---

**Letzte Aktualisierung:** Dezember 2025


