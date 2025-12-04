# 202549
Docker Aufbau Kurs

>Trainer: Michael Lindner


## Modul 01 - Wiederholung

Vorbereitung eines Ubuntu Betriebssystems [zum Inhalt](modul01/readme01.md)  
Die wichtigsten Docker Befehle und Beispiele [zum Inhalt](readme-docker.md)

## Modul 02 - Dockerfile Basics

Statische Website veröffentlichen mit einem Image, welches einen Webserver in sich trägt

**Inhalt:**

- Nginx-Webserver als Basis-Image
- HTML-Dateien in Container kopieren
- Einfaches Dockerfile erstellen und builden

## Modul 03 - Dockerfile Core Image

Image-Aufbau von einem Core-Image und einfacher Command-Ausführung

**Inhalt:**

- Minimale Linux-Basis-Images (Alpine, Ubuntu Core)
- Shell-Befehle im Container ausführen
- CMD vs. ENTRYPOINT verstehen

## Modul 04 - Dockerfile Python Anwendung

Image-Aufbau von einem Framework-Image und Ergänzung von Paketen bis zum Start einer eigenen Python-Anwendung

**Inhalt:**

- Python-Basis-Image verwenden
- Requirements installieren (pip)
- Python-Applikation containerisieren
- Umgebungsvariablen und Port-Mapping

## Modul 05 - Docker Compose Basics

Einführung in Docker Compose mit Multi-Container-Setup

**Inhalt:**

- docker-compose.yml Syntax
- Mehrere Services orchestrieren
- Service-Skalierung mit replicas
- Grundlagen der Container-Orchestrierung

## Modul 06 - Multi-Tier Web Application

Komplexe Multi-Container-Anwendung mit Reverse Proxy, Monitoring und TLS

**Inhalt:**

- Frontend und Backend Services
- Nginx als Reverse Proxy
- Prometheus für Monitoring
- Grafana für Visualisierung
- SSL/TLS-Zertifikate einbinden
- Docker Networks und Volumes

## Modul 07 - GitLab Container Registry

GitLab CE als Container-Registry und CI/CD-Plattform  
[zum Inhalt](modul07/readme.md)

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

Container-Orchestrierung mit Docker Swarm

**Inhalt:**

- Swarm-Cluster aufsetzen (Manager + Worker Nodes)
- Services im Cluster deployen
- Skalierung und Load Balancing
- High Availability und Failover
- Swarm-Befehlsreferenz [zum Inhalt](modul08/readme-swarm.md)



