# deye2mqtt

[Deutsche Version weiter unten (german version below)](#german)

This script reads the current performance data from a Deye inverter, which is often used in balcony power plants, and publishes it via MQTT. It uses the web interface provided by the inverter.

There are many other projects that read data from the cloud or directly communicate with the inverter via Modbus protocol. As I would like to operate it cloud-free and all Modbus variants did not work, this project was created based on an idea by bonnerchen from openhabforum.de.

It is designed to run as a system service and automatically start when the system starts. The script runs completely cloud-free, and the cloud function of the inverter can be prevented. To do this, simply block internet access for the inverter in the router.

## Compatible devices
All devices that provide this web interface are compatible:
![Inverter web interface](screenshot.png)

I operate the script on a Deye-SUN600 myself.

## Requirements
- A Deye inverter
- curl
- Mosquitto client (mosquitto_pub)

## Note on cloud-free operation
If internet access for the inverter is blocked, it can no longer correctly transmit the generated daily output, as the daily counter apparently resets nightly from the cloud and not by the device itself. The value of the total generated power is still transmitted, so the daily production can also be calculated by itself.

## Values transmitted via MQTT
The script transmits the following values to the broker:
- `deye/LWT`: Online/Offline (string). Indicates whether the service is running and the inverter is reachable.
- `deye/current_power`: Number. Indicates the current production power in watts.
- `deye/total_today`: Number. Indicates the production output of the current day in kWh (please note the note on cloud-free operation).
- `deye/total_lifetime`: Number. Indicates the total production output in kWh.

## Automatic installation
1. Simply download and install the file `deye2mqtt.deb`: `sudo dpkg -i deye2mqtt.deb`.
2. Proceed to the configuration section.

## Manual installation
1. Download the files from `src`.
2. Ensure that the required packages are installed (see Requirements).
3. Copy the file `deye2mqtt` to `/usr/bin` and make it executable (`sudo chmod +x /usr/bin/deye2mqtt`).
4. Copy the file `deye2mqtt.conf` to `/etc`.
5. Copy the file `deye2mqtt.service` to `/etc/system/system`.
6. Reload the daemon: `sudo systemctl daemon-reload`.
7. Enable the system service: `sudo systemctl enable deye2mqtt`.
8. Adjust configuration file `/etc/deye2mqtt.conf`.
9. Start system service: `sudo systemctl start deye2mqtt`.

## Configuration
The configuration is done via the file `/etc/deye2mqtt.conf`. The file contains the following settings:
- `broker_address`: The IP address or hostname of the MQTT broker.
- `broker_port`: The port on which the MQTT broker is running.
- `mqtt_topic`: The base topic under which the data will be published.
- `deye_ip`: The IP address of the inverter.
- `deye_user`: The username of the inverter login.
- `deye_pass`: The password of the inverter login.
- `interval`: The number of seconds the script should pause between querying the inverter.

## Uninstallation
To remove the script and configuration file, simply run `sudo dpkg -r deye2mqtt`. This will remove the script and configuration file and restore all system files to the state before the package was installed (only if it was installed automatically).

<a name="german"></a>
# deye2mqtt - Deutsche Version

Dieses Skript liest die aktuellen Leistungsdaten von einem Deye-Wechselrichter, die oft bei Balkonkraftwerken zum Einsatz kommen, aus und veröffentlicht sie über MQTT. Es nutzt dazu das Web-Frontend, das der Wechselrichter bereitstellt.

Es gibt viele andere Projekte, die die Daten aus der Cloud auslesen ODER direkt via Modbus-Protokoll mit dem Wechselrichter sprechen. Da ich diesen gerne cloudfrei betreiben möchte und alle Modbus-Varianten nicht funktionierten, ist dieses Projekt auf einer Idee von bonnerchen aus dem openhabforum.de entstanden.

Es ist so ausgelegt, dass es als Systemdienst ausgeführt wird und automatisch beim Starten des Systems gestartet wird. Das Skript läuft komplett Cloud-frei, die Cloud-Funktion des Wechselrichtes kann unterbunden werden. Dazu einfach für den Wechselrichter im Router den Internetzugang sperren.

## Kompatible Geräte
Es sind alle Geräte kompatibel, die dieses Webinterface zur Verfügung stellen:
![Webinterface des Wechselrichters](screenshot.png)

Ich selbst betreibe das Skript an einem Deye-SUN600.

## Anforderungen
- Ein Deye-Wechselrichter
- curl
- Mosquitto-Client (mosquitto_pub)

## Hinweis zum cloudfreien Betrieb
Wenn der Internetzugang für den Wechselrichter gesperrt wird, kann dieser die erzeugte Tagesleistung nicht mehr korrekt übermitteln, da der Tageszähler offenbar nächtlich von der Cloud und nicht durch das Gerät selbst zurückgesetzt wird. Der Wert des insgesamt erzeugten Stroms wird weiterhin übermittelt, sodass sich die Tagesproduktion auch dadurch selbst berechnen lässt.

## Übermittelte Werte per MQTT
Das Skript übermittelt folgende Werte an den Broker:
- `deye/LWT`: Online/Offline (String). Gibt an, ob der Dienst läuft und der Wechselrichter erreichbar ist.
- `deye/current_power`: Zahl. Gibt in Watt die aktuelle Produktionsleistung an.
- `deye/total_today`: Zahl. Gibt die Produktionsleistung des aktuellen Tages in kWh an (bitte Hinweis zum cloudfreien Betrieb beachten) 
- `deye/total_liftetime`: Zahl. Gibt die gesamte Produktionsleistung in kWh an.

## Automatische Installation
1. Einfach die Datei `deye2mqtt.deb` herunterladen und installieren: `sudo dpkg -i deye2mqtt.deb`.
2. Weiter geht's im Abschnitt Konfiguration.

## Manuelle Installation
1. Die Dateien aus `src` herunterladen.
2. Sicherstellen, dass die erforderlichen Pakete installiert sind (siehe Anforderungen).
3. Die Datei `deye2mqtt` nach `/usr/bin` kopieren und ausführbar machen (`sudo chmod +x /usr/bin/deye2mqtt`).
4. Die Datei `deye2mqtt.conf` nach `/etc` kopieren.
5. Die Datei `deye2mqtt.service` nach `/etc/system/system` kopieren.
6. Den daemon neu laden: `sudo systemctl daemon-reload`.
7. Systemdienst aktivieren: `sudo systemctl enable deye2mqtt`.
8. Konfigurationsdatei `/etc/deye2mqtt.conf` anpassen.
9. Systemdienst starten: `sudo systemctl start deye2mqtt`.

## Konfiguration
Die Konfiguration erfolgt über die Datei `/etc/deye2mqtt.conf`. Die Datei enthält die folgenden Einstellungen:
- `broker_address`: Die IP-Adresse oder der Hostname des MQTT-Brokers.
- `broker_port`: Der Port, auf dem der MQTT-Broker ausgeführt wird.
- `mqtt_topic`: Das Basis-Thema, unter dem die Daten veröffentlicht werden.
- `deye_ip`: Die IP-Adresse des Inverters.
- `deye_user`: Der Benutzername des Inverter-Logins.
- `deye_pass`: Das Passwort des Inverter-Logins.
- `interval`: Die Anzahl der Sekunden, die das Skript zwischen den Abfragen des Inverters pausieren soll.

## Deinstallation
Um das Skript und die Konfigurationsdatei zu entfernen, einfach `sudo dpkg -r deye2mqtt` ausführen. Dies entfernt das Skript und die Konfigurationsdatei und stellt alle Systemdateien auf den Zustand vor der Installation des Pakets zurück (nur, wenn es automatisch installiert wurde).