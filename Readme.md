*Disclaimer: As this is dealing with DE-Alert, the german implementation of EU-Alert (ETSI TS 102 900), the following text is german. An english translation might me provided later*

DE-Alert
========

DE-Alert ist die deutsche Implementierung von EU-Alert, einem Warnsystem auf Basis der Mobilfunktechnologie Cell Broadcast (SMS-CB). 
Die Spezifikation beschreibt, auf welchen Cell Broadcast Kanälen / Message IDs (es gibt 65536) dabei welche Nachrichten-Typen übertragen werden und wie die Entgeräte diese signalisieren sollen.
Die Nachrichten-Typen richten sich dabei nach [ETSI TS 123 041 (Version 11.4, PDF)](https://www.etsi.org/deliver/etsi_ts/123000_123099/123041/11.04.00_60/ts_123041v110400p.pdf) Abschnitt 9.4.1.2.2 und sind über die Kanäle 4370 - 4399 definiert. u.A.:

| Message ID(s)    | Bedeutung | 
| ---------------- | --------- |
| 4370 (4383), 919 | EU-Alert Level 1 / CMAS Presidential Level Alerts |
| 4372 (4385)      | EU-Alert Level 2 / CMAS Extreme Alerts with Severity of Extreme | 
| 4378 (4391)      | EU-Alert Level 3 / CMAS Severe Alerts with Severity of Severe | 
| 4396 (4397)      | EU-Alert Level 4 / CMAS Public Safety Alerts | 
| 4398 (4399)      | EU-Alert EU-Test / CMAS State/Local WEA Test |

Die primäre Message-ID ist für Warnungen in der Landessprache, die sekundäre in Klammern jeweils für weitere Sprachen. 919 wurde zusätzlich ergänzt und kann z.B. auf alten Nokia-Geräten deren UI nur die Eingabe bis 999 zulässt abonniert werden.

Was ist Cell Broadcast
======================
Cell Broadcast ist vor allem eines nicht, ein Warnsystem.
Cell Broadcast ist eine Transportprotokoll für Nachrichten im Mobilfunknetz. Am besten vergleichbar mit UDP Broadcasts in Netzwerken. Und was bei TCP/UDP die Ports sind, sind bei Cell Broadcasts die Message IDs.
Und genauso wie ein Windows PC mit IP Stack nicht automatisch ein Webserver ist, ist Cell Broadcast auch nicht automatisch ein Warnsystem.

Für ein Warnsystem auf Basis von Cell Broadcast braucht man zusätzliche Dinge. Ein Stück Software auf dem Mobilgerät das die Cell Broadcasts empfängt und angemessen präsentiert und ein gemeinsames Verständnis darüber welche Nachrichten man über welche Message IDs sendet.

Nicht jedes Gerät das Cell Broadcast "kann" unterstützt Warnsysteme. Es mag zwar eine Software zum Empfang von Cell Broadcasts mitbringen, aber diese kennt vielleicht das Konzept der Warnmeldung nicht.
So bringt das Nokia 3310 z.B. eine Anwendung mit, mit der man Cell Broadcasts auf den Kanälen 1-999 abonnieren kann. Empfängt es dann eine Nachricht auf einem abonnierten Kanal, wird diese wie eine handelsübliche SMS angezeigt. Es erfolgt keine lautstarke Warnung, aber man kann die Nachricht zumindest lesen.

Andere Geräte bringen möglicherweise garkeine Anwendung mit um Cell Broadcasts zu empfangen.

Übrigens werden anderorts und wurden hierzulande Cell Broadcasts auch für ganz andere Nachrichten wie Wetterberichte, Sportticker, Horoskope oder anderes verwendet. https://twitter.com/Fruchti/status/1553417912790179840

Telefónica o2 sendet noch heute auf Kanal 221 die Gauß-Krüger Koordinaten der Basisstation.


Die größten Mythen
==================

Da die deutschen Medien, allen voran der Heise-Verlag, nicht müde werden Fake-News zum Thema DE-Alert zu verbreiten, habe ich mir die Mühe gemacht und hier einiges zusammen getragen.

Deutschland geht einen Sonderweg
--------------------------------
Das könnte falscher nicht sein. Die technische Richtlinie [TR DE-Alert (Entwurf vom 08.12.2021, PDF)](https://www.bundesnetzagentur.de/SharedDocs/Downloads/DE/Sachgebiete/Telekommunikation/Unternehmen_Institutionen/Anbieterpflichten/OeffentlicheSicherheit/DEAlert/Technische%20Richtlinie%20DE-Alert.pdf) ist die deutsche Implementierung des Standards EU-Alert, beschrieben in [ETSI TS 102 900 (Oktober 2010, PDF)](https://www.etsi.org/deliver/etsi_ts/102900_102999/102900/01.01.01_60/ts_102900v010101p.pdf).
Für den hier betrachteten relevanten Teil, also die Kommunikation von Mobilfunkanbieter zu Mobilgerät, übernimmt die Richtlinie die Vorgaben von EU-Alert. Nämlich die Übertragung der Warnungen als Cell Broadcast über die Kanäle / Message IDs 4370 - 4399 (Seite 21, TR DE-Alert).

EU-Alert selbst hat diese Message IDs auch nicht definiert, sondern wiederum aus dem z.B. in den USA genutzten System CMAS übernommen (siehe ETSI TS 102 900, Seite 10).

Beschrieben sind die Message IDs 4370 - 4399 ursprünglich in [ETSI TS 123 041 (Version 11.4, PDF)](https://www.etsi.org/deliver/etsi_ts/123000_123099/123041/11.04.00_60/ts_123041v110400p.pdf) in Abschnitt 9.4.1.2.2.

Deutschland geht hier also keinen Sonderweg, sondern nutzt den Standard der seit über 12 Jahren spezifiziert ist und übrigens auch von den anderen EU Ländern wie Niederlande eingesetzt wird.

Dreistellig vs. Vierstellig
---------------------------
Es hält sich hartnäckig das Gerücht dass Deutschland ja mit den vierstelligen IDs völlig inkompatibel ist, da die ja viel zu neu sind.
Das das nicht stimmen kann beweist ja schon ETSI TS 123 041 mit CMAS/EU-Alert.
Die Cell Broadcast MessageID wird schon immer mit zwei Byte angegeben. Es sind also 65536 Message IDs möglich. Eine technische Limitierung auf 999 gibt es nicht - das wäre in einer digitalen Welt die sich nicht um das Dezimalsystem schert auch eine sehr seltsame Grenze.
Vierstellige Message IDs sind nicht neuer als die Dreistelligen.

Begünstigt wird dieses Gerücht aber durch tatsächlich existierende Beschränkungen einiger weniger alter Mobiltelefone. So erlaubt z.B. Nokia OS (z.B. Nokia 3310, Baujahr 2000) nur das manuelle Abo von Message IDs von 1-999, weil das Eingabefeld schlicht nur drei Stellen annimmt. Vermutlich eine simple Design-Limitierung.

iOS und DE-Alert
================
iOS unterstützt Cell Broadcast und Warnsysteme wie CMAS und EU-Alert, konfiguriert diese aber [abhängig von der Länderkennung](https://chlosta.me/cbs-ios.html) des genutzten Mobilfunknetzes. Daher funktioniert z.B. EU-Alert in den Niederlanden seit Jahren auf iOS, in Deutschland aber bisher nicht. Apple hat angekündigt EU-Alert in Deutschland ab iOS 16 zu unterstützen.

Android und DE-Alert
====================
Es ist kompliziert 😬 

Der folgende Abschnitt basieren auf dem frei zugänglichen Android Basis-Code:
https://cs.android.com/android/platform/superproject/+/master:packages/apps/CellBroadcastReceiver/

Sowie bis Android 7: 
https://cs.android.com/android/platform/superproject/+/android-7.1.1_r61:frameworks/opt/telephony/src/java/com/android/internal/telephony/gsm/SmsCbConstants.java  
Und ab Android 8: 
https://cs.android.com/android/platform/superproject/+/android-8.0.0_r1:frameworks/base/telephony/java/com/android/internal/telephony/gsm/SmsCbConstants.java

Prinzipiell unterstützt auch Android schon immer Warnsysteme wie CMAS und EU-Alert über Cell Broadcast. Wie bei iOS hat auch Android eine länderspezifische Konfiguration. Allerdings hat Android im Gegensatz zu iOS einen sinnvollen Fallback, sodass das Warnsystem auch verfügbar ist wenn es keine landesspezifische Konfiguration gibt.

Der Code lässt sich hier für jede Android-Version abrufen. Die Konfiguration der genutzten Message-IDs ist dabei im Code hinterlegt bzw. kann der Konfiguration `res/values/config.xml` zu entnehmen, die UI-Beschreibung `res/values/strings.xml`.
Dabei ist zu beachten dass diese Dateien mit spezifischeren Dateien, je Mobilfunknetz, Mobilfunkanbieter und Gerätesprache überlagert werden können. So enthält z.B. `res/values-de/strings.xml` die ins deutsche übersetzten UI Texte. `res/values-mcc262/config.xml` enthält die Konfiguration für das Land mit mcc 262 (Deutschland) und `res/values-mcc262-de/strings.xml` enthält die deutsche Übersetzung der UI explizit für das deutsche System (EU-Alert).

Eine spezifische Konfiguration für Deutschland (mcc 262) ist ab Android 13 enthalten: 
https://cs.android.com/android/platform/superproject/+/android-13.0.0_r1:packages/apps/CellBroadcastReceiver/res/values-mcc262/

In den vorhergehenden Android Versionen gelten die Standardwerte aus:
https://cs.android.com/android/platform/superproject/+/master:packages/apps/CellBroadcastReceiver/res/values/

Die Unterstützung der EU-Alert Level lässt sich also für jede Android-Version ablesen:

| Message ID / Level | ab Android Version | 
| ------------------ | ------------------ |
| 4370 (0x1112) Level 1 | [Android 2.3.6](https://cs.android.com/android/platform/superproject/+/android-2.3.6_r1:frameworks/base/telephony/java/android/telephony/SmsCbConstants.java;l=53)
| 4372 (0x1114) Level 2 | [Android 2.3.6](https://cs.android.com/android/platform/superproject/+/android-2.3.6_r1:frameworks/base/telephony/java/android/telephony/SmsCbConstants.java;l=59) |
| 4378 (0x111A) Level 3 | [Android 2.3.6](https://cs.android.com/android/platform/superproject/+/android-2.3.6_r1:frameworks/base/telephony/java/android/telephony/SmsCbConstants.java;l=77) |
| 4396 (0x112C) Level 4 | [Android 10](https://cs.android.com/android/platform/superproject/+/android-10.0.0_r1:frameworks/base/telephony/java/com/android/internal/telephony/gsm/SmsCbConstants.java;l=202;bpv=0)
| 4398 (0x112E) EU-Test | [Android 10](https://cs.android.com/android/platform/superproject/+/android-10.0.0_r1:frameworks/base/telephony/java/com/android/internal/telephony/gsm/SmsCbConstants.java;l=210;bpv=0)

EU-Alert Level 1-3 werden also von allen Android-Versionen untertstützt. Der neuere Level 4 erst ab Android 10.

Android ist nicht Android
-------------------------
Das Problem ist, dass Android nicht gleich Android ist. Prinzipiell kann und muss jeder Mobilgerätehersteller Android für sein Gerät anpassen und kann dabei auch die Software und Konfiguration zum Empfang der Cell Broadcasts und Warnmeldungen anpassen.

2G, 3G, 4G und 5G
-----------------
[3GPP TS 23.041 / ETSI TS 123 041](https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=748) gilt für Mobilfunk-Generationen 2G bis 5G und somit sollte es eigentlich keinen Unterschied machen, welche Mobilfunkgeneration zum Einsatz kommt, auch wenn die Art und weise wie CellBroadcasts übertragen werden sich je Mobilfunk-Generation auf Transportebene unterscheidet. 

Allerdings unterscheidet Android bei der Konfiguration der CellBroadcast Message-IDs für Warnsysteme zwischen `gsm` und `cdma`, wie [hier in der Standard-Konfiguration](https://cs.android.com/android/platform/superproject/+/android-13.0.0_r1:packages/apps/CellBroadcastReceiver/res/values/config.xml;l=120?q=rat%3Dcdma) zu erkennen ist. Verstanden habe ich das bisher nicht.

Updates
-------
Wenn ich Googles [Dokumentation zu CellBroadcast](https://source.android.com/docs/core/architecture/modular-system/cellbroadcast#module-format) richtig verstehe, wurde die CellBroadcastReceiver App bis Android 10 als Sourcecode bereitgestellt und durch die Smartphone-Hersteller beim Erzeugen der Firmware compiliert und signiert. Daher können nur die Smartphone-Hersteller Updates für die App ausliefern.
Ab Android 11 wird die App fertig compiliert und von Google signiert ausgeliefert und Smartphone-Hersteller integrieren sie einfach nur. Damit kann Google später signierte updates ausliefern.

Das bedeutet aber auch dass man auf gerooteten Geräten relativ simpel die App austauschen und aktualisieren könnte. Ebenso könnten Smartphone-Hersteller Updates der App liefern ohne ganze System-Images neu bauen zu müssen. Die App muss ja nur richtig signiert sein.

Ausserdem gibt es die Möglichkeit die CellBroadcastReceiver über [runtime resource overlays (RROs)](https://source.android.com/docs/core/architecture/rros) zu modifizieren. Damit kann man zwar nicht den Code und die Logik der App anpassen, aber die Ressourcen. Damit ist eine Anpassung der Bezeichnungen ("DE-Alert" statt "presidential Alert"), der Alarmtöne und auch der zu empfangenen Message-IDs möglich. Es können zwar keine neuen Nachrichten-Typen ergänzt werden, aber es können weitere Message-IDs in bestehenden Kategorien abonniert werden, indem die passenden Einträge der `values.xml` überlagert werden.

Unter Android 8.0 (Oreo) habe ich eine Anpassung mittels RRO erfolgreich getestet: https://github.com/xsrf/android-de-alert

Interessanter weise konnte ich ohne spezielle Berechtigungen oder root die Konfiguration der App unter Oreo überlagern 🤔. Unter Android 12 funktioniert das nicht, da bekomme ich einen Fehler dass die Signatur nicht passt.
Offensichtlich ging das bis Android 10. Ab Android 11 müssen Ressourcen entweder über eine `overlayable.xml` zur Überlagerung explizit freigegeben sein, die Overlay-App muss die selbe Signatur haben, oder muss als System-App installiert sein.
Ab Android 11 bringt der CellBroadcastReceiver eine [overlayable.xml](https://cs.android.com/android/platform/superproject/+/android-11.0.0_r1:packages/apps/CellBroadcastReceiver/res/values/overlayable.xml) mit.
So oder so sind die Möglichkeiten durch die RROs aber begrenzt.

Was ich bis heute nicht verstanden habe ist, was die App so besonders macht und warum nicht einfach jeder eine CellBroadcastReceiver App programmieren und bereitstellen kann...

Empfangene CellBroadcast / EU-Alert Meldungen
---------------------------------------------
Folgende CellBroadcasts habe ich mittlerweile aufgeschnappt und entweder Screenshots gemacht und/oder auf gerooteten Geräten die CellBroadcasts direkt vom Handy aus `%appdata%/com.android.cellbroadcastreceiver/databases/cell_broadcasts.db` gezogen.


```json
[
    {
        "body": "DIES IST EINE ERPROBUNG\nDi. 22.11.2022 - 17:07 Uhr - Das BBK erprobt das neue Handy-Warn-System in Ihrer Region. Es besteht keine Gefahr. Bitte ignorieren Sie die Nachricht. Mehr Infos finden Sie unter http://www.bbk.bund.de/cellbroadcast B Sollten Sie diese Nachricht erhalten, so verfügt Ihr Smartphone ggf. nicht über den aktuellen Softwarestand. Wir empfehlen daher, Ihr Endgerät auf den aktuellen Softwarestand zu prüfen. Bei Fragen hierzu wenden Sie sich bitte an Ihren Gerätehersteller.",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 4,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1669133250740,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "26203",
        "priority": 3,
        "read": 1,
        "serial_number": 16400,
        "service_category": 4380
    },
    {
        "body": "TEST ALERT, NATIONWIDE ALERT DAY 2022\nThu 2022/12/08 - 10:59 am - Test alert - for Deutschland - There is no danger. - Further information: https://warnung.bund.de/meldungen - Published by: Bundesamt für Bevölkerungsschutz und Katastrophenhilfe, Nationale Warnzentrale 1 Bonn",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": -1,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1670493567229,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "en",
        "plmn": "26203",
        "priority": 3,
        "read": 1,
        "serial_number": 16384,
        "service_category": 4383
    },
    {
        "body": "PROBEWARNUNG, BUNDESWEITER WARNTAG 2022\nDo. 08.12.2022 - 10:59 Uhr - Probewarnung - für Deutschland - Es besteht keine Gefahr. - Weitere Infos auf https://warnung.bund.de/meldungen - Herausgegeben von: Bundesamt für Bevölkerungsschutz und Katastrophenhilfe, Nationale Warnzentrale 1 Bonn",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 0,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1670493694121,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "26203",
        "priority": 3,
        "read": 1,
        "serial_number": 16384,
        "service_category": 4370
    },
    {
        "body": "Dies ist eine Test Warnnachricht. Bitte ignorieren Sie diese. Es besteht keine Gefahr.",
        "cid": null,
        "cmas_category": null,
        "cmas_certainty": null,
        "cmas_message_class": null,
        "cmas_response_type": null,
        "cmas_severity": null,
        "cmas_urgency": null,
        "date": 1668430290126,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": null,
        "plmn": "26203",
        "priority": 3,
        "read": 1,
        "serial_number": 16416,
        "service_category": 5002
    },
    {
        "body": "THIS IS A TEST\nTue 2022/11/22 - 04:43 pm - BBK is testing the new mobile phone warning system in your region. There is no danger. Please ignore the message. You can find more information at http://www.bbk.bund.de/cellbroadcast B If you receive this message, your smartphone may not have the latest software version. We therefore recommend that you check your device for the latest software version. If you have any questions, please contact your device manufacturer.",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 4,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1669132170323,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "en",
        "plmn": "26203",
        "priority": 3,
        "read": 1,
        "serial_number": 16384,
        "service_category": 4393
    },
        {
        "body": "Dies ist ein Test für Cell Broadcast. Die Telekom erprobt das neue Handy-Warn-System in Ihrer Region. Es besteht keine Gefahr, daher können Sie diese Test-Nachricht ignorieren. Sie möchten keine weiteren Test-Nachrichten bekommen oder mehr Informationen? Wir helfen gern weiter: https://telekom.com/cell-broadcast 0 DE",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 4,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1670234644858,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "",
        "priority": 3,
        "read": 0,
        "serial_number": 25968,
        "service_category": 4380
    },
    {
        "body": "Dies ist ein Test für Cell Broadcast. Die Telekom erprobt das neue Handy-Warn-System in Ihrer Region. Es besteht keine Gefahr, daher können Sie diese Test-Nachricht ignorieren. Sie möchten keine weiteren Test-Nachrichten bekommen oder mehr Informationen? Wir helfen gern weiter: https://telekom.com/cell-broadcast 1 DE",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 4,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1670238265857,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "",
        "priority": 3,
        "read": 0,
        "serial_number": 26144,
        "service_category": 4380
    },
    {
        "body": "Dies ist ein Test für Cell Broadcast. Die Telekom erprobt das neue Handy-Warn-System in Ihrer Region. Es besteht keine Gefahr, daher können Sie diese Test-Nachricht ignorieren. Sie möchten keine weiteren Test-Nachrichten bekommen oder mehr Informationen? Wir helfen gern weiter: https://telekom.com/cell-broadcast 3 EN",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 4,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1670238324164,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "en",
        "plmn": "",
        "priority": 3,
        "read": 0,
        "serial_number": 16496,
        "service_category": 4393
    },
    {
        "body": "Dies ist ein Test für Cell Broadcast. Die Telekom erprobt das neue Handy-Warn-System in Ihrer Region. Es besteht keine Gefahr, daher können Sie diese Test-Nachricht ignorieren. Sie möchten keine weiteren Test-Nachrichten bekommen oder mehr Informationen? Wir helfen gern weiter: https://telekom.com/cell-broadcast 2 EN",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 4,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1670238331721,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "en",
        "plmn": "",
        "priority": 3,
        "read": 0,
        "serial_number": 16480,
        "service_category": 4393
    },
    {
        "body": "TEST ALERT, NATIONWIDE ALERT DAY 2022 Thu 2022/12/08 - 10:59 am - Test alert - for Deutschland - There is no danger. - Further information: https://warnung.bund.de/meldungen - Published by: Bundesamt für Bevölkerungsschutz und Katastrophenhilfe, Nationale Warnzentrale 1 Bonn",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 0,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1670493691588,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "en",
        "plmn": "",
        "priority": 3,
        "read": 1,
        "serial_number": 16608,
        "service_category": 4383
    },
    {
        "body": "Dies ist eine Test Warnnachricht. Bitte ignorieren Sie diese. Es besteht keine Gefahr. Wenn Sie solche Nachrichten nicht empfangen wollen, dass deaktivieren Sie bitte in den Cell-Broadcast Einstellungen Ihres Endgeräts die Option \"alle Kanäle empfangen\".",
        "cid": null,
        "cmas_category": null,
        "cmas_certainty": null,
        "cmas_message_class": null,
        "cmas_response_type": null,
        "cmas_severity": null,
        "cmas_urgency": null,
        "date": 1673363217931,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "26203",
        "priority": 3,
        "read": 1,
        "serial_number": 16384,
        "service_category": 5013
    },
    {
        "body": "Dies ist eine Test Warnnachricht. Bitte ignorieren Sie diese. Es besteht keine Gefahr. Wenn Sie solche Nachrichten nicht empfangen wollen, dass deaktivieren Sie bitte in den Cell-Broadcast Einstellungen Ihres Endgeräts die Option \"alle Kanäle empfangen\".",
        "cid": null,
        "cmas_category": null,
        "cmas_certainty": null,
        "cmas_message_class": null,
        "cmas_response_type": null,
        "cmas_severity": null,
        "cmas_urgency": null,
        "date": 1675336506748,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "26203",
        "priority": 3,
        "read": 1,
        "serial_number": 16384,
        "service_category": 5069
    },
    {
        "body": "bla2foo",
        "cid": null,
        "cmas_category": null,
        "cmas_certainty": null,
        "cmas_message_class": null,
        "cmas_response_type": null,
        "cmas_severity": null,
        "cmas_urgency": null,
        "date": 1676887836340,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "26203",
        "priority": 3,
        "read": 1,
        "serial_number": 16992,
        "service_category": 5001
    },
    {
        "body": "bla2foo",
        "cid": null,
        "cmas_category": null,
        "cmas_certainty": null,
        "cmas_message_class": null,
        "cmas_response_type": null,
        "cmas_severity": null,
        "cmas_urgency": null,
        "date": 1676887863662,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "26203",
        "priority": 3,
        "read": 1,
        "serial_number": 16960,
        "service_category": 5002
    },
    {
        "body": "GEFAHRENDURCHSAGE, HOCH Mo. 21.02.2023 - 10:13 Uhr - Dies ist ein Test für Cell Broadcast. Die Deutsche Telekom erprobt das neue Handy-Warn-System in Ihrer Region. Es besteht keine Gefahr. Bitte ignorieren Sie diese Test-Nachricht. Sie möchten keine weiteren Test-Nachrichten bekommen oder mehr Informationen? Wir helfen gern weiter: https://telekom.com/cell-broadcast (A)",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 4,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1676970937242,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "26201",
        "priority": 3,
        "read": 1,
        "serial_number": 23216,
        "service_category": 4380
    },
    {
        "body": "TEST MI. 22.02.2023 - 14:26 Uhr - Dies ist ein Test für Cell Broadcast. Die Deutsche Telekom erprobt das neue Handy-Warn-System in Ihrer Sie diese Test-Nachricht. Sie möchten keine weiteren Test-Nachrichten bekommen oder mehr Informationen? Wir helfen gern weiter: https://telekom.com/cell-broadcast",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 4,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1677072593040,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "26201",
        "priority": 3,
        "read": 1,
        "serial_number": 23216,
        "service_category": 4380
    },
    {
        "body": "TEST MI. 22.02.2023 - 15:18 Uhr - Dies ist ein Test für Cell Broadcast. Die Deutsche Telekom erprobt das neue Handy-Warn-System in Ihrer Region. Es besteht keine Gefahr. Bitte ignorieren Sie diese Test-Nachricht. Sie möchten keine weiteren Test-Nachrichten bekommen oder mehr Informationen? Wir helfen gern weiter: https://telekom.com/cell-broadcast",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 4,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1677075873388,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "26201",
        "priority": 3,
        "read": 1,
        "serial_number": 32400,
        "service_category": 4380
    },
    {
        "body": "TEST WED 2023/02/22 - 2:26 pm - This is a test for Cell Broadcast. Deutsche Telekom is testing the new cell phone warning system in your area. There is no danger. Please ignore this test message. You don't want to receive any more test messages or want more information? We are pleased to help: https://telekom.com/cell-broadcast",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 4,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1677072855846,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "en",
        "plmn": "26201",
        "priority": 3,
        "read": 1,
        "serial_number": 23216,
        "service_category": 4393
    },
    {
        "body": "TEST WED 2023/02/22 - 3:18 pm - This is a test for Cell Broadcast. Deutsche Telekom is testing the new cell phone warning system in your area. There is no danger. Please ignore this test message. You don't want to receive any more test messages or want more information? We are pleased to help: https://telekom.com/cell-broadcast",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 4,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1677076201836,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "en",
        "plmn": "26201",
        "priority": 3,
        "read": 1,
        "serial_number": 32400,
        "service_category": 4393
    },
    {
        "body": "PROBEWARNUNG, BUNDESWEITER WARNTAG 2023\nDo. 14.09.2023 - 10:59 Uhr - Probewarnung - für Deutschland - Es besteht keine Gefahr. - Weitere Infos auf https://warnung.bund.de/m/S_jcy037ETGT - Herausgegeben von: Bundesamt für Bevölkerungsschutz und Katastrophenhilfe, Nationale Warnze",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 0,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1694682080000,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "",
        "priority": 3,
        "read": 1,
        "serial_number": 16800,
        "service_category": 4370
    },
    {
        "body": "TEST ALERT, NATIONWIDE ALERT DAY 2023\nThu 2023/09/14 - 10:59 am - Test alert - for Germany - There is no danger. - Further information:  by: Bundesamt für Bevölkerungsschutz und Katastrophenhilfe, Nationale Warnzentrale 1 Bonn",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": -1,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1694682081087,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "en",
        "plmn": "26203",
        "priority": 3,
        "read": 1,
        "serial_number": 16800,
        "service_category": 4383
    },
    {
        "body": "PROBEWARNUNG, BUNDESWEITER WARNTAG 2023\nDo. 14.09.2023 - 10:59 Uhr - Probewarnung - für Deutschland - Es besteht keine Gefahr. - Weitere Infos auf https://warnung.bund.de/m/S_jcy037ETGT - Herausgegeben von: Bundesamt für Bevölkerungsschutz und Katastrophenhilfe, Nationale Warnzentrale 1 Bonn",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 0,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1694682087797,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "26203",
        "priority": 3,
        "read": 1,
        "serial_number": 16800,
        "service_category": 4370
    },
    {
        "body": "TEST ALERT, NATIONWIDE PRE ALERT DAY 2023 Wed 2023/09/13 - 11:00 am - Test alert - for Germany - There is no danger. - Further information: https://warnung.bund.de/m/2i_zhY4_d4sj - Published by: Bundesamt für Bevölkerungsschutz und Katastrophenhilfe, Nationale Warnzentrale 1 Bonn",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 4,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1694595007054,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "en",
        "plmn": "26201",
        "priority": 3,
        "read": 1,
        "serial_number": 16912,
        "service_category": 4393
    },
    {
        "body": "TEST - This is a test for Cell Broadcast. Deutsche Telekom is testing the new cell phone warning system in your area. There is no danger. Please ignore this test message. You do not want to receive any more test messages or want more information? We are pleased to help: https://telekom.com/cell-broadcast",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 4,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1694595714309,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "en",
        "plmn": "26201",
        "priority": 3,
        "read": 1,
        "serial_number": 16928,
        "service_category": 4393
    },
    {
        "body": "Dies ist ein Test für Cell Broadcast. Die Telekom erprobt das neue Handy-Warn-System in Ihrer Region. Es besteht keine Gefahr, daher können sie diese Test-Nachricht ignorieren. Sie möchten keine weiteren Test-Nachrichten bekommen oder mehr Informationen? Wir helfen gern weiter: https://telekom.com/cell-broadcast",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 4,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1694595753711,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "26201",
        "priority": 3,
        "read": 1,
        "serial_number": 28992,
        "service_category": 4380
    },
    {
        "body": "PROBEWARNUNG, BUNDESWEITER WARNTAG 2023 Do. 14.09.2023 - 10:59 Uhr - Probewarnung - für Deutschland - Es besteht keine Gefahr. - Weitere Infos auf https://warnung.bund.de/m/S_jcy037ETGT - Herausgegeben von: Bundesamt für Bevölkerungsschutz und Katastrophenhilfe, Nationale Warnzentrale 1 Bonn",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 0,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1694681981519,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "26201",
        "priority": 3,
        "read": 1,
        "serial_number": 16800,
        "service_category": 4370
    },
    {
        "body": "TEST ALERT, NATIONWIDE ALERT DAY 2023 Thu 2023/09/14 - 10:59 am - Test alert - for Germany - There is no danger. - Further information: https://warnung.bund.de/m/S_jcy037ETGT - Published by: Bundesamt für Bevölkerungsschutz und Katastrophenhilfe, Nationale Warnzentrale 1 Bonn",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 0,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1694682014757,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "en",
        "plmn": "26201",
        "priority": 3,
        "read": 1,
        "serial_number": 16800,
        "service_category": 4383
    },
    {
        "body": "PROBEWARNUNG, BUNDESWEITER WARNTAG 2023 Do. 14.09.2023 - 10:59 Uhr - Probewarnung - für Deutschland - Es besteht keine Gefahr. - Weitere Infos auf https://warnung.bund.de/m/S_jcy037ETGT - Herausgegeben von: Bundesamt für Bevölkerungsschutz und Katastrophenhilfe, Nationale Warnzentrale 1 Bonn",
        "cid": null,
        "cmas_category": null,
        "cmas_certainty": null,
        "cmas_message_class": null,
        "cmas_response_type": null,
        "cmas_severity": null,
        "cmas_urgency": null,
        "date": 1694682057635,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "26201",
        "priority": 0,
        "read": 1,
        "serial_number": 16800,
        "service_category": 919
    },
    {
        "body": "PROBEWARNUNG, BUNDESWEITER WARNTAG 2023\nDo. 14.09.2023 - 10:59 Uhr - Probewarnung - für Deutschland - Es besteht keine Gefahr. - Weitere Infos auf https://warnung.bund.de/m/S_jcy037ETGT - Herausgegeben von: Bundesamt für Bevölkerungsschutz und Katastrophenhilfe, Nationale Warnzentrale 1 Bonn",
        "cid": null,
        "cmas_category": null,
        "cmas_certainty": null,
        "cmas_message_class": null,
        "cmas_response_type": null,
        "cmas_severity": null,
        "cmas_urgency": null,
        "date": 1694681954055,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "26203",
        "priority": 0,
        "read": 1,
        "serial_number": 16800,
        "service_category": 919
    },
    {
        "body": "PROBEWARNUNG, BUNDESWEITER WARNTAG 2023\nDo. 14.09.2023 - 10:59 Uhr - Probewarnung - für Deutschland - Es besteht keine Gefahr. - Weitere Infos auf https://warnung.bund.de/m/S_jcy037ETGT - Herausgegeben von: Bundesamt für Bevölkerungsschutz und Katastrophenhilfe, Nationale Warnzentrale 1 Bonn",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 0,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1694681954439,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "26203",
        "priority": 3,
        "read": 1,
        "serial_number": 16800,
        "service_category": 4370
    },
    {
        "body": "TEST ALERT, NATIONWIDE ALERT DAY 2023\nThu 2023/09/14 - 10:59 am - Test alert - for Germany - There is no danger. - Further information: https://warnung.bund.de/m/S_jcy037ETGT - Published by: Bundesamt für Bevölkerungsschutz und Katastrophenhilfe, Nationale Warnzentrale 1 Bonn",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": -1,
        "cmas_message_class": 0,
        "cmas_response_type": -1,
        "cmas_severity": -1,
        "cmas_urgency": -1,
        "date": 1694681955711,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "en",
        "plmn": "26203",
        "priority": 3,
        "read": 1,
        "serial_number": 16800,
        "service_category": 4383
    },
    {
        "body": "TEST ALERT for Bayern Thu 2024/03/14 - 11:00 am - Test alert - for Bayern - There is no danger. - Further information: https://warnung.bund.de/m/6j6FMGb29LWd - Published by: Bayerisches Staatsministerium des Innern, Lagezentrum Bayern",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": 1,
        "cmas_message_class": 1,
        "cmas_response_type": -1,
        "cmas_severity": 0,
        "cmas_urgency": 0,
        "date": 1710410416317,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "en",
        "plmn": "26201",
        "priority": 3,
        "read": 1,
        "serial_number": 18160,
        "service_category": 4385
    },
    {
        "body": "PROBEWARNUNG in Bayern Do. 14.03.2024 - 11:00 Uhr - Probewarnung - für Bayern - Es besteht keine Gefahr. - Weitere Infos auf https://warnung.bund.de/m/6j6FMGb29LWd - Herausgegeben von: Bayerisches Staatsministerium des Innern, Lagezentrum Bayern",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": 1,
        "cmas_message_class": 1,
        "cmas_response_type": -1,
        "cmas_severity": 0,
        "cmas_urgency": 0,
        "date": 1710410419441,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "26201",
        "priority": 3,
        "read": 1,
        "serial_number": 18160,
        "service_category": 4372
    },
    {
        "body": "TEST ALERT for Bayern\nThu 2024/03/14 - 11:00 am - Test alert - for Bayern - There is no danger. - Further information: https://warnung.bund.de/m/6j6FMGb29LWd - Published by: Bayerisches Staatsministerium des Innern, Lagezentrum Bayern",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": 1,
        "cmas_message_class": 1,
        "cmas_response_type": -1,
        "cmas_severity": 0,
        "cmas_urgency": 0,
        "date": 1710410593024,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "en",
        "plmn": "26203",
        "priority": 3,
        "read": 1,
        "serial_number": 18160,
        "service_category": 4385
    },
    {
        "body": "PROBEWARNUNG in Bayern\nDo. 14.03.2024 - 11:00 Uhr - Probewarnung - für Bayern - Es besteht keine Gefahr. - Weitere Infos auf https://warnung.bund.de/m/6j6FMGb29LWd - Herausgegeben von: Bayerisches Staatsministerium des Innern, Lagezentrum Bayern",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": 1,
        "cmas_message_class": 1,
        "cmas_response_type": -1,
        "cmas_severity": 0,
        "cmas_urgency": 0,
        "date": 1710410593642,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "26203",
        "priority": 3,
        "read": 1,
        "serial_number": 18160,
        "service_category": 4372
    },
    {
        "body": "PROBEWARNUNG in Bayern\nDo. 14.03.2024 - 11:00 Uhr - Probewarnung - für Bayern - Es besteht keine Gefahr. - Weitere Infos auf https://warnung.bund.de/m/6j6FMGb29LWd - Herausgegeben von: Bayerisches Staatsministerium des Innern, Lagezentrum Bayern",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": 1,
        "cmas_message_class": 1,
        "cmas_response_type": -1,
        "cmas_severity": 0,
        "cmas_urgency": 0,
        "date": 1710410725061,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "26203",
        "priority": 3,
        "read": 1,
        "serial_number": 18160,
        "service_category": 4372
    },
    {
        "body": "TEST ALERT for Bayern\nThu 2024/03/14 - 11:00 am - Test alert - for Bayern - There is no danger. - Further information: https://warnung.bund.de/m/6j6FMGb29LWd - Published by: Bayerisches Staatsministerium des Innern, Lagezentrum Bayern",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": 1,
        "cmas_message_class": 1,
        "cmas_response_type": -1,
        "cmas_severity": 0,
        "cmas_urgency": 0,
        "date": 1684125728857,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "en",
        "plmn": "",
        "priority": 3,
        "read": 1,
        "serial_number": 18160,
        "service_category": 4385
    },
    {
        "body": "PROBEWARNUNG in Bayern\nDo. 14.03.2024 - 11:00 Uhr - Probewarnung - für Bayern - Es besteht keine Gefahr. - Weitere Infos auf https://warnung.bund.de/m/6j6FMGb29LWd - Herausgegeben von: Bayerisches Staatsministerium des Innern, Lagezentrum Bayern",
        "cid": null,
        "cmas_category": -1,
        "cmas_certainty": 1,
        "cmas_message_class": 1,
        "cmas_response_type": -1,
        "cmas_severity": 0,
        "cmas_urgency": 0,
        "date": 1684125733975,
        "etws_warning_type": null,
        "format": 1,
        "geo_scope": 1,
        "lac": null,
        "language": "de",
        "plmn": "",
        "priority": 3,
        "read": 1,
        "serial_number": 18160,
        "service_category": 4372
    }
]
```

