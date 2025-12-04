# Docker Befehlsreferenz

Eine umfassende Übersicht der wichtigsten Docker-Befehle mit praktischen Beispielen.

## Inhaltsverzeichnis
- [Docker Images](#docker-images)
- [Docker Container](#docker-container)
- [Docker Volumes](#docker-volumes)
- [Docker Networks](#docker-networks)
- [Docker Compose](#docker-compose)
- [Docker Registry](#docker-registry)
- [System-Befehle](#system-befehle)
- [Dockerfile](#dockerfile)

---

## Docker Images

### Image herunterladen
```bash
docker pull <image-name>:<tag>
docker pull nginx:latest
docker pull python:3.11-slim
```

### Images auflisten
```bash
docker images
docker images -a          # Alle Images (inkl. Zwischenlayer)
docker images --filter "dangling=true"  # Ungenutzte Images
```

### Image erstellen
```bash
docker build -t <image-name>:<tag> .
docker build -t myapp:1.0 .
docker build -t myapp:latest -f Dockerfile.prod .
docker build --no-cache -t myapp:latest .  # Ohne Cache
```

### Image löschen
```bash
docker rmi <image-id>
docker rmi nginx:latest
docker rmi $(docker images -q)  # Alle Images löschen
```

### Image inspizieren
```bash
docker inspect <image-name>
docker history <image-name>  # Build-Historie anzeigen
```

### Image taggen
```bash
docker tag <source-image> <target-image>
docker tag myapp:1.0 myregistry.azurecr.io/myapp:1.0
```

---

## Docker Container

### Container starten
```bash
docker run <image-name>
docker run -d nginx                    # Im Hintergrund (-d = detached)
docker run -it ubuntu bash             # Interaktiv (-it)
docker run --name mycontainer nginx    # Mit Namen
docker run -p 8080:80 nginx           # Port-Mapping
docker run -v /host/path:/container/path nginx  # Volume
docker run -e ENV_VAR=value nginx     # Umgebungsvariable
docker run --rm nginx                 # Auto-remove nach Stop
```

### Container auflisten
```bash
docker ps                  # Laufende Container
docker ps -a               # Alle Container (inkl. gestoppte)
docker ps -q               # Nur IDs
docker ps --filter "status=exited"  # Nach Status filtern
```

### Container stoppen/starten
```bash
docker stop <container-id>
docker start <container-id>
docker restart <container-id>
docker pause <container-id>    # Pausieren
docker unpause <container-id>  # Fortsetzen
```

### Container löschen
```bash
docker rm <container-id>
docker rm -f <container-id>           # Erzwungenes Löschen
docker rm $(docker ps -aq)            # Alle Container löschen
docker container prune                # Alle gestoppten Container löschen
```

### Container inspizieren
```bash
docker inspect <container-id>
docker logs <container-id>            # Logs anzeigen
docker logs -f <container-id>         # Logs live verfolgen
docker logs --tail 100 <container-id> # Letzte 100 Zeilen
docker stats                          # Ressourcenverbrauch
docker stats <container-id>           # Für spezifischen Container
docker top <container-id>             # Prozesse im Container
```

### In Container ausführen
```bash
docker exec -it <container-id> bash
docker exec -it <container-id> sh
docker exec <container-id> ls /app
docker exec -u root <container-id> bash  # Als root-User
```

### Dateien kopieren
```bash
docker cp <container-id>:/path/in/container /host/path  # Aus Container
docker cp /host/path <container-id>:/path/in/container  # In Container
```

### Container committen (Image erstellen)
```bash
docker commit <container-id> <new-image-name>
docker commit mycontainer myapp:snapshot
```

---

## Docker Volumes

### Volume erstellen
```bash
docker volume create <volume-name>
docker volume create mydata
```

### Volumes auflisten
```bash
docker volume ls
docker volume ls --filter "dangling=true"  # Ungenutzte Volumes
```

### Volume inspizieren
```bash
docker volume inspect <volume-name>
```

### Volume mit Container verwenden
```bash
docker run -v <volume-name>:/path/in/container nginx
docker run -v mydata:/app/data nginx
docker run -v $(pwd):/app nginx  # Aktuelles Verzeichnis mounten
docker run --mount source=mydata,target=/app nginx  # Alternative Syntax
```

### Volume löschen
```bash
docker volume rm <volume-name>
docker volume prune  # Alle ungenutzten Volumes löschen
```

---

## Docker Networks

### Network erstellen
```bash
docker network create <network-name>
docker network create --driver bridge mynetwork
docker network create --subnet=172.20.0.0/16 mynetwork
```

### Networks auflisten
```bash
docker network ls
```

### Network inspizieren
```bash
docker network inspect <network-name>
docker network inspect bridge  # Standard bridge network
```

### Container mit Network verbinden
```bash
docker run --network <network-name> nginx
docker network connect <network-name> <container-id>
docker network disconnect <network-name> <container-id>
```

### Network löschen
```bash
docker network rm <network-name>
docker network prune  # Alle ungenutzten Networks löschen
```

---

## Docker Compose

### Services starten
```bash
docker-compose up
docker-compose up -d              # Im Hintergrund
docker-compose up --build         # Mit rebuild
docker-compose up --force-recreate  # Container neu erstellen
docker-compose up <service-name>  # Nur bestimmten Service
```

### Services stoppen
```bash
docker-compose stop
docker-compose down               # Stoppen und löschen
docker-compose down -v            # Inkl. Volumes löschen
docker-compose down --rmi all     # Inkl. Images löschen
```

### Services verwalten
```bash
docker-compose ps                 # Status anzeigen
docker-compose logs               # Logs anzeigen
docker-compose logs -f            # Logs live verfolgen
docker-compose logs <service-name>  # Logs für Service
docker-compose exec <service-name> bash  # In Service einloggen
docker-compose restart            # Neu starten
docker-compose pause              # Pausieren
docker-compose unpause            # Fortsetzen
```

### Build-Befehle
```bash
docker-compose build              # Alle Services builden
docker-compose build --no-cache   # Ohne Cache
docker-compose build <service-name>  # Nur einen Service
```

### Konfiguration validieren
```bash
docker-compose config             # Konfiguration anzeigen
docker-compose config --services  # Alle Services auflisten
```

### Skalierung
```bash
docker-compose up --scale <service-name>=3  # Service auf 3 Instanzen skalieren
```

---

## Docker Registry

### Image zu Registry pushen
```bash
docker login                      # Bei Docker Hub anmelden
docker login <registry-url>       # Bei privater Registry anmelden
docker push <image-name>:<tag>
docker push myregistry.azurecr.io/myapp:1.0
```

### Image von Registry pullen
```bash
docker pull <registry-url>/<image-name>:<tag>
docker pull myregistry.azurecr.io/myapp:1.0
```

### Registry abmelden
```bash
docker logout
docker logout <registry-url>
```

---

## System-Befehle

### System-Informationen
```bash
docker version                    # Docker-Version
docker info                       # System-Informationen
docker system df                  # Speicherverbrauch
```

### Aufräumen
```bash
docker system prune               # Ungenutzte Ressourcen löschen
docker system prune -a            # Alle ungenutzten Ressourcen
docker system prune -a --volumes  # Inkl. Volumes
docker image prune                # Nur ungenutzte Images
docker container prune            # Nur gestoppte Container
docker volume prune               # Nur ungenutzte Volumes
docker network prune              # Nur ungenutzte Networks
```

### Events verfolgen
```bash
docker events                     # Echtzeit-Events
docker events --since '2024-01-01'  # Events seit Datum
docker events --filter 'container=mycontainer'  # Gefilterte Events
```

---

## Dockerfile

### Grundstruktur
```dockerfile
# Basis-Image
FROM python:3.11-slim

# Metadaten
LABEL maintainer="your-email@example.com"
LABEL version="1.0"

# Arbeitsverzeichnis
WORKDIR /app

# Dateien kopieren
COPY requirements.txt .
COPY . .

# Befehle ausführen
RUN pip install --no-cache-dir -r requirements.txt

# Umgebungsvariablen
ENV APP_ENV=production
ENV PORT=8000

# Port freigeben
EXPOSE 8000

# Volume definieren
VOLUME /app/data

# User wechseln
USER appuser

# Startbefehl
CMD ["python", "app.py"]
# oder
ENTRYPOINT ["python"]
CMD ["app.py"]
```

### Wichtige Dockerfile-Befehle

| Befehl | Beschreibung | Beispiel |
|--------|--------------|----------|
| `FROM` | Basis-Image festlegen | `FROM ubuntu:22.04` |
| `WORKDIR` | Arbeitsverzeichnis setzen | `WORKDIR /app` |
| `COPY` | Dateien kopieren | `COPY . /app` |
| `ADD` | Dateien kopieren (mit Extraktion) | `ADD archive.tar.gz /app` |
| `RUN` | Befehl während Build ausführen | `RUN apt-get update` |
| `CMD` | Standard-Befehl (überschreibbar) | `CMD ["npm", "start"]` |
| `ENTRYPOINT` | Haupt-Befehl (nicht überschreibbar) | `ENTRYPOINT ["python"]` |
| `ENV` | Umgebungsvariable setzen | `ENV NODE_ENV=production` |
| `EXPOSE` | Port dokumentieren | `EXPOSE 8080` |
| `VOLUME` | Volume-Mount-Point | `VOLUME /data` |
| `USER` | User für nachfolgende Befehle | `USER node` |
| `ARG` | Build-Argument | `ARG VERSION=1.0` |
| `LABEL` | Metadaten hinzufügen | `LABEL version="1.0"` |

### Dockerfile Best Practices
```dockerfile
# Multi-stage Build für kleinere Images
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:18-slim
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY package*.json ./
RUN npm ci --only=production
USER node
CMD ["node", "dist/index.js"]
```

### .dockerignore Beispiel
```
node_modules
npm-debug.log
.git
.gitignore
.env
.vscode
*.md
Dockerfile
docker-compose.yml
```

---

## Praktische Beispiele

### Nginx Webserver
```bash
docker run -d \
  --name mynginx \
  -p 8080:80 \
  -v $(pwd)/html:/usr/share/nginx/html \
  nginx:latest
```

### PostgreSQL Datenbank
```bash
docker run -d \
  --name postgres \
  -e POSTGRES_PASSWORD=mysecret \
  -e POSTGRES_DB=mydb \
  -v pgdata:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:15
```

### Python-Anwendung
```bash
docker run -d \
  --name myapp \
  -v $(pwd):/app \
  -w /app \
  -p 5000:5000 \
  python:3.11 \
  python app.py
```

### Redis Cache
```bash
docker run -d \
  --name redis \
  -p 6379:6379 \
  redis:7-alpine
```

---

## Nützliche Kombinationen

### Container-Logs durchsuchen
```bash
docker logs mycontainer 2>&1 | grep "ERROR"
```

### Alle Container einer bestimmten Image löschen
```bash
docker ps -a | grep "nginx" | awk '{print $1}' | xargs docker rm -f
```

### Speicherplatz freigeben
```bash
docker system prune -a --volumes -f
```

### Container mit Health Check starten
```bash
docker run -d \
  --name webapp \
  --health-cmd="curl -f http://localhost/ || exit 1" \
  --health-interval=30s \
  --health-timeout=3s \
  --health-retries=3 \
  nginx
```

### Container-IP-Adresse anzeigen
```bash
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container-id>
```

---

## Tipps & Tricks

1. **Aliases verwenden** (für PowerShell):
   ```powershell
   function dps { docker ps $args }
   function dpa { docker ps -a $args }
   function di { docker images $args }
   function dex { docker exec -it $args }
   ```

2. **Container automatisch neu starten**:
   ```bash
   docker run -d --restart=unless-stopped nginx
   docker run -d --restart=always nginx
   ```

3. **Resource Limits setzen**:
   ```bash
   docker run -d --memory="512m" --cpus="1.5" nginx
   ```

4. **Container benennen**:
   ```bash
   docker run -d --name my-descriptive-name nginx
   ```

5. **Labels für Organisation**:
   ```bash
   docker run -d --label environment=production --label app=api nginx
   docker ps --filter "label=environment=production"
   ```

---

## Weitere Ressourcen

- [Offizielle Docker Dokumentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [Docker Compose Dokumentation](https://docs.docker.com/compose/)
- [Dockerfile Best Practices](https://docs.docker.com/develop/dev-best-practices/)
