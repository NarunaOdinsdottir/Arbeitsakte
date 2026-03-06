# ☢️ Vault-Tec Technical Archive

## 🐳 Containerisierung für die Post-Apokalypse

### Docker Field Manual – Overseer Edition

*Archiviert durch die IT-Sicherheitsabteilung von* **Vault-Tec**

---

## 📟 Eintrag im Overseer-Log

In einer Welt voller fehlerhafter Deployments, instabiler Systeme und mutierter Softwarepakete benötigen Überlebende eine Methode, Anwendungen **isoliert, reproduzierbar und kontrollierbar** auszuführen.

Das Werkzeug der Wahl: **Docker**

Docker kapselt Anwendungen in **Container**, kleine abgeschlossene Laufzeitumgebungen, die überall identisch funktionieren — egal ob im Laborserver oder tief im Datentunnel eines Vaults.

---

# 🗂️ Inhaltsverzeichnis des Vault-Terminals

1. Grundlegende Befehle
2. Image-Management
3. Container Lifecycle
4. Dockerfiles & Image Building
5. Docker Compose & Microservices
6. Architektur & Security

---

# 1️⃣ Grundlegende Befehle

### Standardwerkzeuge für Container-Techniker

| Befehl              | Bedeutung im Vault                                    |
| ------------------- | ----------------------------------------------------- |
| `docker ps`         | Zeigt aktive Container im System                      |
| `docker ps -a`      | Listet alle Container (auch deaktivierte Experimente) |
| `docker logs <ID>`  | Zeigt das Logbuch eines Containers                    |
| `docker stop/start` | Stoppt oder reaktiviert einen Container               |
| `docker rm`         | Entfernt einen Container dauerhaft                    |

Diese Kommandos sind das tägliche Werkzeug eines Container-Ingenieurs.

---

# 2️⃣ Image-Management

### Baupläne für digitale Lebensformen

Ein **Image** ist eine unveränderliche Vorlage, aus der Container entstehen.

### Image herunterladen

```
docker pull ubuntu:22.04
```

Ohne Versionsangabe wird automatisch das Tag `latest` verwendet.

### Lokale Images anzeigen

```
docker image ls
```

### Image entfernen

```
docker image rm <image-id>
```

⚠️ **Vault-Tec Sicherheitshinweis**

Das Tag `latest` ist bequem – aber gefährlich.

Wenn sich die Version im Hintergrund ändert, kann dein System plötzlich ein **anderes Image** starten.

Produktionssysteme sollten immer **explizite Versionstags** verwenden.

---

# 3️⃣ Container Lifecycle

### Geburt, Betrieb und Stilllegung eines Containers

Ein Container entsteht durch den Befehl:

```
docker run [OPTIONS] IMAGE [COMMAND]
```

Wichtige Optionen im Alltag:

| Option                | Bedeutung                                        |
| --------------------- | ------------------------------------------------ |
| `-d`                  | Detached Mode (läuft im Hintergrund)             |
| `-it`                 | Interaktive Shell                                |
| `-p 8080:80`          | Port-Mapping Host → Container                    |
| `-v /host:/container` | Volume Mount für persistente Daten               |
| `--rm`                | Container wird nach Beenden automatisch gelöscht |

Container sind **ephemere Systeme** — sie können jederzeit neu erstellt werden. Persistente Daten gehören daher in **Volumes**, nicht in den Container selbst.

---

# 4️⃣ Dockerfiles & Image Building

### Bauanleitung für reproduzierbare Systeme

Ein **Dockerfile** beschreibt exakt, wie ein Image erzeugt wird.

Beispiel: Minimaler Apache-Webserver.

```
FROM ubuntu:22.04

RUN apt-get update -y && \
    apt-get install apache2 -y

EXPOSE 80

CMD ["apache2ctl", "-D", "FOREGROUND"]
```

### Image bauen

```
docker build -t mein-webserver:v1 .
```

#### Vault-Tec Optimierungsregel

Mehrere Befehle sollten kombiniert werden, um unnötige **Layer** zu vermeiden.

Weniger Layer = kleinere Images = schnellere Deployments.

---

# 5️⃣ Docker Compose & Microservices

### Verwaltung komplexer Container-Ökosysteme

Moderne Anwendungen bestehen selten aus einem einzigen Dienst.

Typisches Setup:

* Webserver
* Backend-API
* Datenbank
* Cache

Zur Orchestrierung wird **Docker Compose** verwendet.

Die Infrastruktur wird als YAML-Datei definiert.

### Vorteile

• Infrastruktur als Code
• Automatische Netzwerke zwischen Services
• Einfaches Starten kompletter Stacks

### Wichtige Befehle

Starten aller Services:

```
docker-compose up -d
```

Alles stoppen und Netzwerk entfernen:

```
docker-compose down
```

---

# 6️⃣ Architektur & Security

### Container-Infrastruktur im Vault

Docker folgt einem **Client-Server-Modell**.

Der Client kommuniziert mit dem Docker-Daemon über eine Unix-Socket:

```
/var/run/docker.sock
```

Dieser Socket besitzt **volle Kontrolle über den Host**.

---

## ⚠️ Sicherheitswarnung: Container Escape

Wenn ein Container zu viele Rechte erhält, kann ein Angreifer versuchen, aus der isolierten Umgebung auszubrechen.

Ein Beispiel ist der Zugriff auf Host-Namespaces über Tools wie **nsenter**.

```
nsenter --target 1 --mount --uts --ipc --net /bin/bash
```

Wenn der Container den Host-Prozess `PID 1` sehen kann, wird ein Zugriff auf den Host möglich.

---

## 🔒 Sicherheitsrichtlinien für stabile Systeme

Vault-Tec empfiehlt folgende Regeln:

• Container niemals **privileged** starten
• Zugriff auf `docker.sock` strikt vermeiden
• Root-Container nach Möglichkeit verhindern
• Images regelmäßig aktualisieren
• Nur vertrauenswürdige Basis-Images verwenden

Container sind Isolation — aber **keine vollständige Sicherheitsgrenze**.

---

# ☢️ Abschlussnotiz aus dem Overseer-Terminal

Containerisierung ist eines der mächtigsten Werkzeuge moderner Infrastruktur.

Mit **Docker** lassen sich Anwendungen reproduzierbar, portabel und skalierbar betreiben.

Doch jede Technologie ist nur so sicher wie ihre Konfiguration.

Im Vault gilt daher eine einfache Regel:

> Baue Systeme so, als würden sie irgendwann angegriffen werden.

Die gute Nachricht:
Mit sauber konfigurierten Containern überlebt deine Infrastruktur selbst die schlimmsten digitalen Mutationen.

---

**Vault-Tec Container Operations Manual – Archivversion**
