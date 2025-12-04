# Docker Swarm - Setup und Befehlsreferenz

Docker Swarm ist die native Clustering- und Orchestrierungslösung von Docker für die Verwaltung von Container-Clustern.

## Inhaltsverzeichnis
- [Docker Swarm Aktivierung](#docker-swarm-aktivierung)
- [Swarm Befehlsreferenz](#swarm-befehlsreferenz)

---

## Docker Swarm Aktivierung

### Voraussetzungen

**Erforderliche Ports:**
- TCP Port 2377 - Cluster-Management-Kommunikation
- TCP/UDP Port 7946 - Kommunikation zwischen Knoten
- UDP Port 4789 - Overlay-Netzwerk-Traffic

**Beispiel-Setup:**
- 1 Manager-Node (Master): `192.168.1.10`
- 2 Worker-Nodes: `192.168.1.11` und `192.168.1.12`

---

### Schritt 1: Swarm auf dem Manager-Node initialisieren

Auf dem **Manager-Node** (Master):

```bash
# Swarm initialisieren
docker swarm init --advertise-addr 192.168.1.10

# Output (Beispiel):
# Swarm initialized: current node (abc123xyz) is now a manager.
# 
# To add a worker to this swarm, run the following command:
#     docker swarm join --token SWMTKN-1-xxxxxxxxxxxxx 192.168.1.10:2377
#
# To add a manager to this swarm, run 'docker swarm join-token manager'
```

**Hinweis:** Der Output enthält den Join-Token für Worker-Nodes. Diesen Token benötigen Sie im nächsten Schritt.

---

### Schritt 2: Worker-Nodes zum Swarm hinzufügen

Auf **Worker-Node 1** (`192.168.1.11`):

```bash
# Worker zum Swarm hinzufügen (Token aus Schritt 1 verwenden)
docker swarm join --token SWMTKN-1-xxxxxxxxxxxxx 192.168.1.10:2377

# Output:
# This node joined a swarm as a worker.
```

Auf **Worker-Node 2** (`192.168.1.12`):

```bash
# Worker zum Swarm hinzufügen (gleicher Token wie bei Worker 1)
docker swarm join --token SWMTKN-1-xxxxxxxxxxxxx 192.168.1.10:2377

# Output:
# This node joined a swarm as a worker.
```

---

### Schritt 3: Swarm-Status überprüfen

Zurück auf dem **Manager-Node**:

```bash
# Alle Nodes im Swarm anzeigen
docker node ls

# Output (Beispiel):
# ID                            HOSTNAME   STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
# abc123xyz *                   manager1   Ready     Active         Leader           24.0.0
# def456uvw                     worker1    Ready     Active                          24.0.0
# ghi789rst                     worker2    Ready     Active                          24.0.0
```

**Erklärung der Spalten:**
- `*` = aktueller Node
- `MANAGER STATUS: Leader` = Manager-Node
- `STATUS: Ready` = Node ist einsatzbereit
- `AVAILABILITY: Active` = Node nimmt Tasks an

---

### Join-Token erneut abrufen

Falls Sie den Join-Token verloren haben:

```bash
# Worker-Token anzeigen
docker swarm join-token worker

# Manager-Token anzeigen (zum Hinzufügen weiterer Manager)
docker swarm join-token manager

# Token erneuern (Sicherheit)
docker swarm join-token --rotate worker
```

---

### Firewall-Konfiguration (falls erforderlich)

**Auf allen Nodes** (Linux mit ufw):

```bash
# Management-Port
sudo ufw allow 2377/tcp

# Node-Kommunikation
sudo ufw allow 7946/tcp
sudo ufw allow 7946/udp

# Overlay-Netzwerk
sudo ufw allow 4789/udp

# Firewall neu laden
sudo ufw reload
```

**Auf Windows** (PowerShell als Administrator):

```powershell
# Management-Port
New-NetFirewallRule -DisplayName "Docker Swarm Management" -Direction Inbound -Protocol TCP -LocalPort 2377 -Action Allow

# Node-Kommunikation
New-NetFirewallRule -DisplayName "Docker Swarm Node Communication TCP" -Direction Inbound -Protocol TCP -LocalPort 7946 -Action Allow
New-NetFirewallRule -DisplayName "Docker Swarm Node Communication UDP" -Direction Inbound -Protocol UDP -LocalPort 7946 -Action Allow

# Overlay-Netzwerk
New-NetFirewallRule -DisplayName "Docker Swarm Overlay Network" -Direction Inbound -Protocol UDP -LocalPort 4789 -Action Allow
```

---

### Swarm-Konfiguration abschließen

**Optional: Labels zu Nodes hinzufügen** (für gezielte Service-Platzierung):

```bash
# Label zu Worker 1 hinzufügen
docker node update --label-add environment=production worker1
docker node update --label-add region=eu-west worker1

# Label zu Worker 2 hinzufügen
docker node update --label-add environment=production worker2
docker node update --label-add region=eu-east worker2

# Labels anzeigen
docker node inspect worker1 --format '{{.Spec.Labels}}'
```

**Node-Verfügbarkeit ändern:**

```bash
# Node auf "drain" setzen (keine neuen Tasks, bestehende werden migriert)
docker node update --availability drain worker1

# Node wieder aktivieren
docker node update --availability active worker1
```

---

## Swarm Befehlsreferenz

### Swarm-Management

#### Swarm initialisieren
```bash
docker swarm init
docker swarm init --advertise-addr <IP-ADRESSE>
docker swarm init --advertise-addr 192.168.1.10
docker swarm init --advertise-addr eth0  # Interface-Name verwenden
```

#### Swarm-Informationen
```bash
docker swarm join-token worker          # Worker-Token anzeigen
docker swarm join-token manager         # Manager-Token anzeigen
docker swarm join-token --rotate worker # Token erneuern
docker info                             # Swarm-Status im System-Info
```

#### Node zum Swarm hinzufügen
```bash
docker swarm join --token <TOKEN> <MANAGER-IP>:2377
docker swarm join --token SWMTKN-1-xxx 192.168.1.10:2377
```

#### Swarm verlassen
```bash
docker swarm leave              # Als Worker verlassen
docker swarm leave --force      # Als Manager verlassen
```

#### Swarm-Konfiguration aktualisieren
```bash
docker swarm update --autolock=true     # Auto-lock aktivieren
docker swarm update --autolock=false    # Auto-lock deaktivieren
docker swarm update --task-history-limit 10  # Task-Historie begrenzen
```

---

### Node-Management

#### Nodes auflisten
```bash
docker node ls
docker node ls --filter role=manager    # Nur Manager
docker node ls --filter role=worker     # Nur Worker
docker node ls --format "table {{.Hostname}}\t{{.Status}}\t{{.Availability}}"
```

#### Node inspizieren
```bash
docker node inspect <NODE-ID>
docker node inspect worker1
docker node inspect --pretty worker1    # Lesbare Ausgabe
docker node inspect worker1 --format '{{.Status.State}}'
```

#### Node aktualisieren
```bash
docker node update --availability active <NODE-ID>
docker node update --availability pause <NODE-ID>    # Keine neuen Tasks
docker node update --availability drain <NODE-ID>    # Tasks migrieren
docker node update --label-add type=web worker1      # Label hinzufügen
docker node update --label-rm type worker1           # Label entfernen
docker node update --role manager worker1            # Zum Manager befördern
```

#### Node aus Swarm entfernen
```bash
docker node rm <NODE-ID>
docker node rm worker1
docker node rm --force worker1          # Erzwungenes Entfernen
```

#### Node befördern/degradieren
```bash
docker node promote worker1             # Worker zum Manager befördern
docker node demote manager2             # Manager zum Worker degradieren
```

---

### Service-Management

#### Service erstellen
```bash
docker service create --name <SERVICE-NAME> <IMAGE>
docker service create --name web nginx
docker service create --name web --replicas 3 nginx
docker service create --name web -p 8080:80 nginx
docker service create --name web --replicas 3 -p 8080:80 nginx:latest

# Mit Umgebungsvariablen
docker service create --name db -e POSTGRES_PASSWORD=secret postgres

# Mit Volume
docker service create --name web --mount type=volume,source=webdata,target=/data nginx

# Mit Constraints (Placement)
docker service create --name web --constraint node.labels.region==eu-west nginx
docker service create --name web --constraint node.role==worker nginx

# Mit Ressourcen-Limits
docker service create --name web --limit-cpu 0.5 --limit-memory 512M nginx
docker service create --name web --reserve-cpu 0.25 --reserve-memory 256M nginx

# Mit Update-Konfiguration
docker service create --name web --update-delay 10s --update-parallelism 2 nginx

# Mit Restart-Policy
docker service create --name web --restart-condition on-failure --restart-max-attempts 3 nginx
```

#### Services auflisten
```bash
docker service ls
docker service ls --filter name=web
docker service ls --format "table {{.Name}}\t{{.Replicas}}\t{{.Image}}"
```

#### Service inspizieren
```bash
docker service inspect <SERVICE-NAME>
docker service inspect web
docker service inspect --pretty web     # Lesbare Ausgabe
```

#### Service-Details anzeigen
```bash
docker service ps <SERVICE-NAME>
docker service ps web
docker service ps web --filter "desired-state=running"
docker service ps web --no-trunc        # Vollständige Ausgabe
```

#### Service skalieren
```bash
docker service scale <SERVICE-NAME>=<ANZAHL>
docker service scale web=5
docker service scale web=10 api=3       # Mehrere Services gleichzeitig
```

#### Service aktualisieren
```bash
docker service update <SERVICE-NAME>
docker service update --image nginx:latest web
docker service update --replicas 5 web
docker service update --env-add NEW_VAR=value web
docker service update --env-rm OLD_VAR web
docker service update --publish-add 9090:80 web
docker service update --publish-rm 8080 web
docker service update --limit-memory 1G web
docker service update --constraint-add node.role==worker web
docker service update --label-add version=2.0 web

# Rolling Update
docker service update --update-delay 30s --update-parallelism 1 --image nginx:1.20 web

# Rollback
docker service rollback web
```

#### Service löschen
```bash
docker service rm <SERVICE-NAME>
docker service rm web
docker service rm web api db            # Mehrere Services
```

#### Service-Logs anzeigen
```bash
docker service logs <SERVICE-NAME>
docker service logs web
docker service logs -f web              # Live verfolgen
docker service logs --tail 100 web      # Letzte 100 Zeilen
docker service logs --since 30m web     # Letzte 30 Minuten
```

---

### Stack-Management (Docker Compose mit Swarm)

#### Stack deployen
```bash
docker stack deploy -c docker-compose.yml <STACK-NAME>
docker stack deploy -c docker-compose.yml myapp
docker stack deploy -c compose.yml -c compose.prod.yml myapp  # Mehrere Dateien
```

#### Stacks auflisten
```bash
docker stack ls
```

#### Stack-Services anzeigen
```bash
docker stack services <STACK-NAME>
docker stack services myapp
docker stack services --filter name=myapp_web myapp
```

#### Stack-Tasks anzeigen
```bash
docker stack ps <STACK-NAME>
docker stack ps myapp
docker stack ps --filter "desired-state=running" myapp
```

#### Stack entfernen
```bash
docker stack rm <STACK-NAME>
docker stack rm myapp
```

---

### Netzwerk-Management

#### Overlay-Netzwerk erstellen
```bash
docker network create --driver overlay <NETWORK-NAME>
docker network create --driver overlay mynet
docker network create --driver overlay --attachable mynet
docker network create --driver overlay --subnet 10.0.0.0/24 mynet
docker network create --driver overlay --encrypted secure-net  # Verschlüsselt
```

#### Netzwerke auflisten
```bash
docker network ls
docker network ls --filter driver=overlay
```

#### Netzwerk inspizieren
```bash
docker network inspect <NETWORK-NAME>
docker network inspect mynet
```

#### Netzwerk löschen
```bash
docker network rm <NETWORK-NAME>
docker network rm mynet
```

---

### Secret-Management (Passwörter, Tokens, etc.)

#### Secret erstellen
```bash
echo "mein_passwort" | docker secret create db_password -
docker secret create db_password ./password.txt
printf "super_secret" | docker secret create api_key -
```

#### Secrets auflisten
```bash
docker secret ls
```

#### Secret inspizieren
```bash
docker secret inspect db_password
docker secret inspect --pretty db_password
```

#### Secret in Service verwenden
```bash
docker service create --name db --secret db_password postgres
docker service create --name db --secret source=db_password,target=/run/secrets/db_pwd postgres

# Secret zu bestehendem Service hinzufügen
docker service update --secret-add db_password db
```

#### Secret löschen
```bash
docker secret rm db_password
```

---

### Config-Management (Konfigurationsdateien)

#### Config erstellen
```bash
docker config create nginx_config ./nginx.conf
echo "key=value" | docker config create app_config -
```

#### Configs auflisten
```bash
docker config ls
```

#### Config inspizieren
```bash
docker config inspect nginx_config
docker config inspect --pretty nginx_config
```

#### Config in Service verwenden
```bash
docker service create --name web --config source=nginx_config,target=/etc/nginx/nginx.conf nginx
docker service update --config-add nginx_config web
```

#### Config löschen
```bash
docker config rm nginx_config
```

---

## Praktische Beispiele

### Beispiel 1: Nginx-Webserver mit 3 Replicas

```bash
# Service erstellen
docker service create \
  --name web \
  --replicas 3 \
  --publish 80:80 \
  --update-delay 10s \
  --update-parallelism 1 \
  nginx:latest

# Status prüfen
docker service ps web

# Skalieren
docker service scale web=5

# Logs anzeigen
docker service logs -f web
```

### Beispiel 2: PostgreSQL-Datenbank mit Secret

```bash
# Secret erstellen
echo "mein_sicheres_passwort" | docker secret create db_password -

# Volume erstellen
docker volume create pgdata

# Service erstellen
docker service create \
  --name postgres \
  --secret db_password \
  -e POSTGRES_PASSWORD_FILE=/run/secrets/db_password \
  --mount type=volume,source=pgdata,target=/var/lib/postgresql/data \
  --replicas 1 \
  --constraint 'node.role==manager' \
  postgres:15

# Status prüfen
docker service ps postgres
```

### Beispiel 3: Multi-Tier Applikation (Web + API + DB)

```bash
# Overlay-Netzwerk erstellen
docker network create --driver overlay app-network

# Datenbank
docker service create \
  --name db \
  --network app-network \
  -e POSTGRES_PASSWORD=secret \
  --mount type=volume,source=dbdata,target=/var/lib/postgresql/data \
  postgres:15

# API-Backend
docker service create \
  --name api \
  --network app-network \
  --replicas 3 \
  -e DATABASE_URL=postgresql://db:5432/mydb \
  myapi:latest

# Web-Frontend
docker service create \
  --name web \
  --network app-network \
  --replicas 3 \
  --publish 80:80 \
  -e API_URL=http://api:3000 \
  myweb:latest

# Status aller Services
docker service ls
```

### Beispiel 4: Stack mit docker-compose.yml

**docker-compose.yml:**
```yaml
version: '3.8'

services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    deploy:
      replicas: 3
      update_config:
        parallelism: 1
        delay: 10s
      restart_policy:
        condition: on-failure
    networks:
      - webnet

  api:
    image: myapi:latest
    deploy:
      replicas: 2
      placement:
        constraints:
          - node.role==worker
    networks:
      - webnet

  db:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password
    secrets:
      - db_password
    volumes:
      - dbdata:/var/lib/postgresql/data
    deploy:
      replicas: 1
      placement:
        constraints:
          - node.role==manager
    networks:
      - webnet

networks:
  webnet:
    driver: overlay

volumes:
  dbdata:

secrets:
  db_password:
    external: true
```

**Deployment:**
```bash
# Secret erstellen
echo "mein_passwort" | docker secret create db_password -

# Stack deployen
docker stack deploy -c docker-compose.yml myapp

# Status prüfen
docker stack services myapp
docker stack ps myapp

# Logs anzeigen
docker service logs myapp_web

# Stack entfernen
docker stack rm myapp
```

### Beispiel 5: Global Service (auf jedem Node)

```bash
# Monitoring-Agent auf allen Nodes
docker service create \
  --name monitoring-agent \
  --mode global \
  --mount type=bind,source=/var/run/docker.sock,target=/var/run/docker.sock \
  monitoring-agent:latest

# Status prüfen (sollte auf jedem Node laufen)
docker service ps monitoring-agent
```

### Beispiel 6: Service mit Health Check

```bash
docker service create \
  --name web \
  --replicas 3 \
  --publish 80:80 \
  --health-cmd "curl -f http://localhost/ || exit 1" \
  --health-interval 30s \
  --health-timeout 3s \
  --health-retries 3 \
  nginx:latest

# Health-Status prüfen
docker service ps web
```

---

## Monitoring und Troubleshooting

### Service-Status überprüfen
```bash
# Übersicht aller Services
docker service ls

# Details eines Services
docker service ps web

# Service-Logs
docker service logs web
docker service logs -f --tail 100 web

# Node-Status
docker node ls

# Swarm-Informationen
docker info
```

### Problembehebung

**Container auf Node finden:**
```bash
# Auf jedem Node
docker ps --filter "label=com.docker.swarm.service.name=web"
```

**Service neu starten (Force Update):**
```bash
docker service update --force web
```

**Node aus Rotation nehmen:**
```bash
docker node update --availability drain worker1
# Tasks warten, dann Wartung durchführen
docker node update --availability active worker1
```

**Service-Inspect für Details:**
```bash
docker service inspect --pretty web
```

---

## Best Practices

1. **Manager-Nodes:**
   - Verwenden Sie 3, 5 oder 7 Manager für Hochverfügbarkeit (ungerade Anzahl)
   - Nur 1 Manager ist für Entwicklung ausreichend

2. **Worker-Nodes:**
   - Beliebige Anzahl von Worker-Nodes
   - Platzieren Sie rechenintensive Workloads auf Workers

3. **Secrets:**
   - Verwenden Sie immer Secrets für sensible Daten
   - Niemals Passwörter in Umgebungsvariablen

4. **Update-Strategien:**
   - Definieren Sie `update-delay` und `update-parallelism`
   - Testen Sie Updates zuerst in einer Staging-Umgebung

5. **Health Checks:**
   - Implementieren Sie Health Checks für alle Services
   - Stellen Sie automatisches Failover sicher

6. **Ressourcen-Limits:**
   - Setzen Sie CPU- und Memory-Limits
   - Vermeiden Sie Ressourcen-Überbuchung

7. **Netzwerke:**
   - Verwenden Sie Overlay-Netzwerke für Service-Kommunikation
   - Isolieren Sie Services durch verschiedene Netzwerke

8. **Logging:**
   - Zentralisieren Sie Logs mit einem Log-Aggregator
   - Verwenden Sie `docker service logs` für Debugging

---

## Nützliche Aliases (PowerShell)

Fügen Sie diese Ihrer PowerShell-Profile hinzu (`$PROFILE`):

```powershell
# Swarm-Aliases
function dsls { docker service ls $args }
function dsps { docker service ps $args }
function dsl { docker service logs $args }
function dnls { docker node ls $args }
function dss { docker stack services $args }
function dsp { docker stack ps $args }

# Service-Management
function dsrm { docker service rm $args }
function dsscale { docker service scale $args }
function dsupdate { docker service update $args }
```

---

## Weitere Ressourcen

- [Docker Swarm Dokumentation](https://docs.docker.com/engine/swarm/)
- [Docker Service Dokumentation](https://docs.docker.com/engine/reference/commandline/service/)
- [Docker Stack Dokumentation](https://docs.docker.com/engine/reference/commandline/stack/)
- [Swarm Mode Tutorial](https://docs.docker.com/engine/swarm/swarm-tutorial/)
