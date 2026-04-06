# 🕹 Vault-Tec Terminal Guide: Dein erster Cron-Job

Willkommen, Vault-Bewohner! Du hast das große Vault-Terminal geöffnet und willst jetzt lernen, wie man es programmiert, um deine automatischen Helferlein zu steuern – deine Cron-Jobs. Keine Sorge, wir brechen nichts… meistens. 😏

## 🛠 Schritt 1 – Das Terminal aktivieren

Jeder Vault-Bewohner hat seine eigene Aufgabenliste, die Crontab genannt wird. Tippe ins Terminal:

crontab -e

💡 Vault-Fun Fact: „cron“ kommt vom griechischen Chronos, der Gott der Zeit. Perfekt, um die Uhr in deinem Vault zu kontrollieren, oder?

## 🛠 Schritt 2 – Wähle deinen Vault-Schreiber

Linux fragt dich, welchen Editor du benutzen willst.

Tippe 1 für nano – der einfachste Editor für Vault-Neulinge.

## 🛠 Schritt 3 – Das 5-Sterne-System verstehen ⭐⭐⭐⭐⭐

Ein Cron-Job sieht so aus:

* * * * * Befehl

Vault-Übersetzung:

Minute (0–59)
Stunde (0–23)
Tag des Monats (1–31)
Monat (1–12)
Wochentag (0–6, 0 = Sonntag)

## 🛠 Schritt 4 – Deinen ersten Vault-Job

Wir lassen den Butler eine Nachricht schreiben:

* * * * * echo "Cron läuft!" >> ~/cron_test.txt
Jede Minute schreibt er brav in cron_test.txt.

## 🛠 Schritt 5 – Speichern & den Vault verlassen

In nano: Strg + O → Enter → Strg + X
Du siehst: crontab: installing new crontab ✅
💡 Vault-Tipps & Ninja-Logik 🥷
* = jeden (Minute, Stunde, Tag…)
/ = Intervall: */5 * * * * = alle 5 Minuten
, = Mehrfachauswahl: 0 8,17 * * * = 8:00 & 17:00

Fun Fact: Cron-Jobs sind die Vampire des Vault-Systems. Viele Backups laufen nachts, wenn alle schlafen – die Garbage Collection Hour 🌙

🔍 Überprüfen ob dein Butler arbeitet
cat ~/cron_test.txt
Wenn dort Cron läuft! steht – Glückwunsch, du bist jetzt Vault-Automatisierer.

## ⚠️ Achtung, Vault-Pitfall

Cron sieht keine GUI. Alles, was du laufen lassen willst, mit absolutem Pfad angeben:

/home/vaultuser/skript.sh
Andernfalls bleibt dein Job nur ein Geist im Terminal-Keller 👻

## 🔥 Praxislevel: Cron als Vault-Hacker

Output umleiten = Loot sichern

Beispiel 1 – ps Log
0 10 * * 2 ps aux > /home/vaultuser/ps_log.txt
Jeden Dienstag um 10 Uhr:
ps aux läuft
Ergebnis landet in Datei

Beispiel 2 – top im Batch-Modus
0 10 * * 2 top -b -n 1 > /home/vaultuser/top_log.txt
-b = Batch
-n 1 = nur einmal

Beispiel 3 – Systemfehler checken
0 10 * * * journalctl -p err -n 50 > /home/vaultuser/error_log.txt
Holt die letzten 50 Fehler-Logs täglich um 10 Uhr

## 😏 Hacker-Move: Benachrichtigungen

Klassisch: Mail
0 10 * * * journalctl -p err -n 50 | mail -s "Vault Errors" you@example.com
Modern: Telegram, Discord, oder dein Pipboy

💡 Bonus: Cron-jeden-zweiten-Tag
0 10 */2 * * command
Startet alle 2 Tage
Achtung: beginnt vom Monatsanfang

## 🛠 Fehlersuche für Ödland-Wanderer (Troubleshooting)

Wenn dein Butler streikt und die cron_test.txt leer bleibt, checke diese drei Überlebensregeln:

## 1. Die "Heilige Leerzeile" 📜

Cron ist altmodisch. Er braucht am Ende der Datei zwingend eine leere Zeile. Wenn dein Befehl die absolut letzte Zeile ohne einen finalen Zeilenumbruch ist, wird er oft ignoriert.

Fix: Geh in crontab -e, fahre ans Ende deines Befehls und drücke einmal kräftig Enter. Speichern, fertig.

## 2. Der Pfad-Fluch 🗺️
Cron kennt deine Umgebungsvariablen nicht. Er weiß nicht, wo python oder dein spezielles Skript liegt.

Fix: Nutze immer absolute Pfade.
Statt python script.py schreibe /usr/bin/python3 /home/vaultuser/script.py.

Du findest den Pfad zu Programmen mit dem Befehl which:
which python3 -> Gibt dir den Pfad aus.

## 3. Das Log-Orakel befragen 🔮

Wenn du wissen willst, ob Cron überhaupt versucht hat zu arbeiten, schau in die System-Logs.

Befehl: grep CRON /var/log/syslog (auf Ubuntu/Debian) oder journalctl -u cron.

Dort steht schwarz auf weiß, ob der Job gestartet wurde oder ob er wegen eines Syntax-Fehlers ("Bad minute", "Bad day-of-month") abgebrochen ist.

## 💡 Vault-Fun Fact: Die "Epoch-Sekunde"

Computer zählen die Zeit nicht in Tagen oder Monaten, sondern in Sekunden seit dem 1. Januar 1970 (dem Unix-Epoch). Wenn ein Cron-Job abstürzt, liegt es oft daran, dass irgendwo eine Zeitberechnung mit dieser riesigen Zahl durcheinandergekommen ist. Wir leben technisch gesehen im Jahr 1.7 Milliarden+ nach Unix-Rechnung!

Vault-Bewohner, dein Terminal wartet… dein Cron-Job ist jetzt dein kleiner, stiller Helfer im Hintergrund. Willkommen im Club der stillen Ninja-Automatisierer 🥷💥

## Defensive Vault-Programmierung per Script und Cron

#!/bin/bash

# 1. Umgebungsvariablen für die GUI (Damit Cron das Pop-up senden darf)

export DISPLAY=:0
export DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/$(id -u)/bus

# 2. Pfade

LOGFILE="/home/$USER/vault_status.log"
TIMESTAMP=$(date "+%d.%m.%Y %H:%M:%S")

# 3. Systemdaten sammeln
# Wir nehmen nur die Ganzzahl der CPU-Last für den Vergleich

CPU_LOAD=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}' | cut -d'.' -f1)
RAM_USAGE=$(free | grep Mem | awk '{print $3/$2 * 100.0}' | cut -d'.' -f1)

# 4. Den Bericht in die Datei schreiben

echo "[VAULT-OS MONITOR] - $TIMESTAMP | CPU: $CPU_LOAD% | RAM: $RAM_USAGE%" >> $LOGFILE

# 5. DER ALARM-CHECK (Wenn RAM > 80% oder CPU > 90%)

if [ "$RAM_USAGE" -gt 80 ]; then
    notify-send "⚠️ VAULT-ALARM" "Kritische RAM-Auslastung: $RAM_USAGE%" --icon=dialog-warning
fi

if [ "$CPU_LOAD" -gt 90 ]; then
    notify-send "🔥 REAKTOR-HITZE" "CPU-Last extrem hoch: $CPU_LOAD%" --icon=error
fi
