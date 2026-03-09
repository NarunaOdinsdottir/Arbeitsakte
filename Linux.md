🛡️ # VAULT-TEC FIELD MANUAL
Grundlagen & Sicherheit von Linux-Systemen

"Ein gut gewartetes System ist der Unterschied zwischen einer funktionierenden Vault… und einem Experiment."

Dieses Dokument vermittelt die grundlegenden Werkzeuge und Sicherheitsmechanismen eines Linux-Systems.
Es richtet sich an Systemadministratoren, Security-Analysten und technisch neugierige Vault-Bewohner, die ihre Umgebung verstehen und schützen wollen.

📟 ## 1. Systemaufklärung – Wer bin ich und wo bin ich?

Bevor man ein System absichert, muss man verstehen, wer man ist, wo man sich befindet und welche Rechte man besitzt.

Linux bietet dafür mehrere schnelle Diagnosebefehle.

🔎 ## Wichtige Systembefehle
| Befehl     | Funktion                                              |
| ---------- | ----------------------------------------------------- |
| `whoami`   | Zeigt den aktuell angemeldeten Benutzer               |
| `hostname` | Gibt den Namen des Systems aus                        |
| `id`       | Zeigt Benutzer-ID, Gruppen und Berechtigungen         |
| `uname -a` | Liefert Kernel-, System- und Architekturinformationen |

# Beispiel
whoami
hostname
id
uname -a

Diese Informationen helfen Administratoren zu verstehen:

Welche Berechtigungen vorhanden sind

Auf welchem Systemtyp gearbeitet wird

Ob Privilegieneskalation möglich ist

📂 # 2. Dateisystem-Erkundung

Linux-Systeme enthalten oft tausende Konfigurationsdateien.
Das wichtigste Werkzeug zur Navigation ist der legendäre find-Befehl.

🔍 # Dateien suchen
find / -type f -name "*.conf"

Häufige Suchparameter:

Parameter	Bedeutung
-type f	Nur Dateien
-name *.conf	Bestimmte Dateiendungen
-user root	Dateien eines bestimmten Benutzers
-size +20k	Dateien größer als 20 KB

✏️ # Dateien bearbeiten mit VIM

Der Editor vim ist ein Klassiker in der Linux-Welt.

| Modus              | Funktion       |
| ------------------ | -------------- |
| Normalmodus        | Navigation     |
| Insert-Modus (`i`) | Text schreiben |

Speichern & Beenden
:wq

Suche im Dokument
/Suchbegriff

Vim ist am Anfang etwas wie ein Vault-Tec Trainingssimulator: verwirrend, aber nach kurzer Zeit extrem effizient.

🌐 # 3. Fernzugriff und Netzwerk

Linux-Server werden häufig ohne grafische Oberfläche betrieben und über das Netzwerk administriert.

Das wichtigste Werkzeug dafür ist:

🔐 SSH – Secure Shell

SSH ermöglicht verschlüsselten Fernzugriff auf Systeme.

Beispiel:

ssh user@server

Vorteile:
- verschlüsselte Kommunikation
- geringer Ressourcenverbrauch
- ideal für Serververwaltung

# Netzwerkdiagnose

| Tool       | Funktion                           |
| ---------- | ---------------------------------- |
| `ip`       | Anzeige von Netzwerkschnittstellen |
| `ifconfig` | ältere Alternative                 |
| `ss`       | zeigt aktive Netzwerkverbindungen  |
| `netstat`  | klassisches Netzwerkdiagnose-Tool  |

🧱 ## 4. Systemhärtung (Hardening)

Ein ungeschütztes System ist wie eine Vault ohne Tür.
Linux bietet mehrere Schutzmechanismen.

🔑 # Festplattenverschlüsselung (LUKS)

LUKS – Linux Unified Key Setup ermöglicht vollständige Festplattenverschlüsselung.

Vorteile:

-Schutz bei Diebstahl von Hardware
-Daten bleiben ohne Passwort unlesbar
-verbreiteter Standard in Linux-Distributionen

🔥# Firewall-Schutz

Firewalls kontrollieren ein- und ausgehenden Netzwerkverkehr.

Kernel-Firewalls

iptables
nftables

Diese arbeiten direkt im Kernel und erlauben präzise Filterregeln.

# UFW – Uncomplicated Firewall

Ein einfaches Frontend für Firewallregeln.

Beispiel:

sudo ufw allow 22/tcp

Dies erlaubt Zugriff auf SSH (Port 22).

🔄 # System-Updates

Regelmäßige Updates schließen Sicherheitslücken.

Ein berühmtes Beispiel war die Kernel-Schwachstelle Dirty COW, die Privilegieneskalation erlaubte.

System aktualisieren:

sudo apt update && sudo apt upgrade

📜# 5. Logdateien – Die Erinnerung des Systems

Logs sind das Gedächtnis eines Servers.

Die meisten befinden sich in:
/var/log

Wichtige Logdateien

| Datei               | Inhalt                                |
| ------------------- | ------------------------------------- |
| `/var/log/auth.log` | Login- und Authentifizierungsversuche |
| `/var/log/syslog`   | allgemeine Systemmeldungen            |


# Loganalyse

Letzte Zeilen anzeigen
tail -n 50 /var/log/syslog

Nach bestimmten Ereignissen suchen
grep FAILED /var/log/auth.log

Kernelmeldungen auslesen
dmesg

Dies zeigt den Kernel-Ringpuffer, der Hardware- und Bootmeldungen enthält.

🧠## Vault-Tec Erkenntnis

Ein sicheres Linux-System basiert auf drei Prinzipien:

Transparenz – Verstehen, was im System passiert

Kontrolle – Zugriff und Netzwerkverkehr regulieren

Beobachtung – Logs analysieren und Anomalien erkennen

Oder anders gesagt:

"Die größte Bedrohung für eine Vault ist nicht die Außenwelt… sondern ein Administrator, der seine Logs nicht liest."
