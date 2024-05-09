# Dokumentation 
Name: Mattis Lindemann, BDF (2023)

Aufgabennummer: Aufgabe 2 // DFIR-Untersuchung: Erpressung durch Kompromittierung 


## 1 Aufgabenstellung

Auf einem Desktop erschien eine Notiz, in der behauptet wird, dass das System kompromittiert sei und sensible Daten gestohlen worden seien. Die Täter drohen nun mit der Veröffentlichung dieser Daten, sollten ihre Forderungen nicht erfüllt werden. Der Arbeitsrechner des Entwicklers enthielt sensible Dateien. Ich habe die Aufgabe mittels "DFIR"-Methoden den Angriff zu analysieren und einen Kontaktweg zu den Angreifern zu finden, die ihre eigenen Kontaktinformationen (versehentlich) unzugänglich gemacht haben.


## 2 Vorgehensweise
### 2.1 What is the full path of the script used by Simon for AWS operations?
Es galt den vollständigen Pfad des für AWS-Operationen zu ermitteln. Dieser konnte im oberen Teil der config.xml Datei abgelesen werden. Dieser Tag in der config.xml Datei bedeutet, dass die Datei AWS_objects migration.pl kürzlich in Notepad++ geöffnet wurde oder Teil einer gespeicherten Sitzung ist.

**Antwort: C:\Users\Simon.stark\Documents\Dev_Ops\AWS_objects migration.pl**

![Foto AWS_objects](https://raw.githubusercontent.com/malin013/htb/main/IMG_5953.JPEG)

### 2.2 The attacker duplicated some program code and compiled it on the system, knowing that the victim was a software engineer and had all the necessary utilities. They did this to blend into the environment and didn't bring any of their tools. This code gathered sensitive data and prepared it for exfiltration. What is the full path of the program's source file?
Der vollständige Pfad der Quelldatei des Programms lässt sich in der session.xml Datei ablesen. Die sessions.xml Datei in Notepad++ dient dazu, Informationen über geöffnete Dokumente und deren Zustand innerhalb von Notepad++ über Sitzungen hinweg zu speichern.

**Antwort: C:\Users\simon.stark\Desktop\LootAndPurge.java**

![Foto Java Script LootAndPurge.java](https://raw.githubusercontent.com/malin013/htb/main/IMG_5951.JPEG)

### 2.3 What's the name of the final archive file containing all the data to be exfiltrated?
Hier gibt ein Blick in den Schadcode Aufschluss über die endgültige Archvdatei. Der Java-Code sammelt Dateien auf dem Desktop eines Benutzers und komprimiert diese in ein passwortgeschütztes ZIP-Archiv mit dem Namen:

**Antwort: Forela-Dev-Data.zip**

![Foto ZIP-Datei](https://raw.githubusercontent.com/malin013/htb/main/IMG_5954.JPEG)

### 2.4 What's the timestamp in UTC when attacker last modified the program source file?
Die letzte Änderung der Quelldaten des Java-Programms lässt sich in der session.xml ermitteln. Hier sind ein high- und ein low-Timestamp vermerkt. Unter Anwendung des beigefügten Python-Scriptes war es unproblematisch beide miteinander zu kombinieren. Die zwei Teile des Zeitstempels (high und low, je 32 Bits) werden zu einem vollständigen 64-Bit-Wert kombiniert, der in Sekunden umgewandelt wird, um das Datum und die Uhrzeit zu bestimmen, die seit dem 01.01.1601 vergangen ist. Zur schnellen Analyse des Cods wurde ChatGPT 4.0 verwendet. 

*Initialisierung der Zeitstempelteile:* timestamp_low und timestamp_high repräsentieren die unteren und oberen 32 Bits eines 64-Bit Zeitstempels.

*Kombination der Zeitstempelteile:* Durch die Operation (timestamp_high << 32) wird der high-Teil um 32 Bit nach links verschoben, um die oberen 32 Bits zu bilden. timestamp_low & 0xFFFFFFFF stellt sicher, dass nur die unteren 32 Bits des low-Teils genutzt werden. Beide Teile werden dann mittels einer OR-Operation (|) kombiniert, um den vollständigen 64-Bit-Zeitstempel zu erhalten.

*Umwandlung in Sekunden:* full_timestamp / 10**7 wandelt die Zeit von 100-Nanosekunden-Intervallen (wie sie in einigen Systemen wie Windows FILETIME verwendet werden) in Sekunden um.

*Berechnung des Datums:* Das Datum und die Uhrzeit werden berechnet, indem die berechneten Sekunden zu dem Basisdatum 1. Januar 1601 hinzugefügt werden. Dies erfolgt mit der datetime.timedelta Funktion, die diese Sekunden zum Basisdatum addiert.

*Ausgabe des Datums:* Schließlich wird das berechnete Datum und die Uhrzeit ausgegeben, die den Moment repräsentieren, der dem ursprünglichen 64-Bit Zeitstempel entspricht.

**Antwort: 2023-07-24 09:53:23**

![Foto Timestamp](https://raw.githubusercontent.com/malin013/htb/main/IMG_5952.JPEG)

![Foto Ergebnis](https://raw.githubusercontent.com/malin013/htb/main/IMG_5955.JPEG)

### 2.5 The attacker wrote a data extortion note after exfiltrating data. What is the crypto wallet address to which attackers demanded payment?
Der zweite Link aus dem Erpresserschreiben funktioniert. Jedoch stößt man direkt auf eine Passworteingabe, bevor der Klartext der Nachricht angezeigt wird. Im Erpresserschreiben selbst ist kein Passwort vermerkt. Das Passwort aus dem Java-Script/ der Schadsoftware, welches eigentich als Passwort für das ZIP-Archiv mit den gesammelten Daten fungiert, erlaubt eine Entschlüsselung. 

Die ETH-Adresse sowie die Kontakt-E-Mail-Adresse sind im Klartext abesbar.

**Antwort: 0xca8fa8f0b631ecdb18cda619c4fc9d197c8affca**

![Erpresserschreiben](https://raw.githubusercontent.com/malin013/htb/main/IMG_5956.JPEG)

### 2.6 What's the email address of the person to contact for support?
Siehe 2.5

**Antwort: CyberJunkie@mail2torjgmxgexntbrmhvgluavhj7ouul5yar6ylbvjkxwqf6ixkwyd.onion**


## 3 Probleme
Ich habe den Python Code unter den übersetzten Aufgabenstellungen nicht gelesen. Entsprechend lange habe ich an dem Problem gesessen. 

## 4 Fazit/Feedback zur Aufgabe

Hat Spaß gemacht - allerdings fehlt noch einiges an Hintergrundwissen, um die beigefügten Dateien in Gänze und Zeile für Zeil zu verstehen. 
