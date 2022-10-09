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
Die Cell Broadcast MessageID wird schon immer mit zwei Byte angegeben. Es sind also 65536 Message IDs möglich. Eine technische Limitierung auf 999 gibt es nicht - das wäre in einer digitalen Welt die sich nicht um das Dezimalsystem schwert auch eine sehr seltsame Grenze.
Vierstellige Message IDs sind nicht neuer als die Dreistelligen.

Begünstigt wird dieses Gerücht aber durch tatsächlich existierende Beschränkungen einiger weniger alter Mobiltelefone. So erlaubt z.B. Nokia OS (z.B. Nokia 3310, Baujahr 2000) nur das manuelle Abo von Message IDs von 1-999, weil das Eingabefeld schlicht nur drei Stellen annimmt. Vermutlich ein simpler Design-Fehler.

iOS und DE-Alert
================
iOS unterstützt Cell Broadcast und Warnsysteme wie CMAS und EU-Alert, konfiguriert diese aber abhängig von der Länderkennung des genutzten Mobilfunknetzes. Daher funktioniert z.B. EU-Alert in den Niederlanden seit Jahren auf iOS, in Deutschland aber bisher nicht. Apple hat angekündigt EU-Alert in Deutschland ab iOS 16 zu unterstützen.

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
