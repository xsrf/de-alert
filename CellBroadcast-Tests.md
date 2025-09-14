# CellBroadcast Tests in Deutschland (MCC 262)

*Hinweis: Mit meinen Testgeräten kann ich nur CellBroadcasts im 2G und 4G Netz testen. Die Geräte sind nicht immer an und ich muss SIM Karten tauschen.*

## Telekom (MNC 01)
Seit mitte 2025 scheint die Telekom regelemäßige automatische Tests mittels CellBroadcast für DE-Alert durchzuführen. Dabei werden über ausgewählte (jedenfalls in meiner Region nur ein paar) 4G Basisstationen alle 10 Minuten CellBroadcasts über Message-ID `4380` also Kategorie ausgestrahlt. Konkret erfolgt die Ausstrahlung jeweils ab Minute 2 für die Dauer von 4 Minuten. Im nächsten Intervall folgt dann eine neue Seriennummer und somit eine neue Nachricht.

Hier der Wortlaut:
```
Dies ist ein Test für das Cell-Broadcast Monitoring mit WSQM. Es besteht keine Gefahr. Bitte ignorieren Sie diese Test-Nachricht!
```

Die Message ID `4380` entspricht Kategorie `EU-Monthly Test - Für Mobilfunknetzbetreiber-interne Tests`. Um diese auf einem normalen Endgerät empfangen zu können, muss man z.B. unter Android die Kategorie `Cell Broadcast Test` aktivieren. Diese steht überhaupt erst zur verfügung, wenn man mittels USSD Code `*#*#CMAS#*#*` bzw. `*#*#2627#*#*` die Testoptionen aktiviert.

## Vodafone (MNC 02)
Vodafone führt jeden ersten Dienstag im Monat um 12 Uhr einen CellBroadcast test durch. Mangels explizitem testgerät fehlen mir hier bisher Angaben, aber ich gehe davon aus dass ebenfalls Message ID `4380` genutzt wird und dass der Aussand bundesweit erfolgt.

Hier der Wortlaut:
```
Dies ist ein Test für Cell Broadcast. Vodafone erprobt das neue Handy-Warn-System in Ihrer Region. Es besteht keine Gefahr. Bitte ignorieren Sie diese Test-Nachricht. Sie möchten keine weiteren Test-Nachrichten bekommen oder mehr Informationen? Wir helfen gern weiter: https://www.vodafone.de/cellbroadcast [ISO-Timestamp Auslösezeitpunkt]
```

## o2 (MNC 03)
Regelmäßige Tests im Netz der Telefónica o2 sind mir bisher nicht bekannt. Der letzte von mir aufgezeichnete Test war die Nachricht `bla2foo` mit Message ID `5002` im Februar 2023.