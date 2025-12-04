# 202549
Docker Aufbau Kurs

>Trainer: Michael Lindner


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



