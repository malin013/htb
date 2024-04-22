# Dokumentation 
Name: Mattis Lindemann, BDF (2023)

Aufgabennummer: Aufgabe 1 // Flagge im Bild 


## 1 Aufgabenstellung

Gemäß Aufgabenstellung liegt ein Bild im jpeg-Format vor. Die Zielsetzung ist das Extrahieren einer Datei, die mittels staganographischer Verfahren in diesem Bild versteckt worden ist. 

Zunächst soll sich mit Staganographiebefasst werden. Es sollen Tools identifiziert werden, die zur Extraktion der Datei verwendet werden können.

Anschließend gilt es den sowohl Passwortschutz der Datei, als auch die Verschlüsselung der Flagge in der Datei zu überwinden.


## 2 Vorgehensweise
### 2.1 Extraktion der Datei
Zunächst wurden verschiedene Steganographie-Tools ermittelt, die für die o.g. Aufgabenstellung in Betracht kommen könnten. Insbesondere wurde sich mit den Open-Source-Tools "Steghide" und "OpenStego" befasst. Die Wahl fiel auf das aus BDF104/ BDF106 bereits bekannte Tool "Steghide", da die Bedienung bereits bekannt ist. Die Durchführung erfolgt über VirtualBox/ Ubuntu, da Steghide nicht für Windows als 64-Bit-Version verfügbar ist.

Zunächst wurde mit dem Befehl **$ steghide extract -sf aufgabe1.jpeg** versucht die Datei zu extrahieren. Es folgte eine Passwortabfrage, welche mit einem manuellen Brute-Force-Angriff (Top 10 Passwörter) überwunden werden sollte - ohne Erfolg. Die Top 10 Passwörter wurden über www.chip.de ermittelt (https://www.chip.de/news/Erschreckend-simpel-Diese-Passwoerter-sind-die-beliebtesten-Passwoerter-2023-in-Deutschland-und-leicht-zu-knacken_185076495.html).

Recherchen ergaben, dass mit dem Tool Stegseek eine Passwortliste auf passwortgeschütze Dateien angewendet werden kann, die mit Steghide verschlüsselt wurden. Es wurde sich für Stegseek entschieden, weil es im Vergleich zu anderen Tools, wie z.B. StegCracker, sehr schnell ist.

Die Eingabe: 

**stegseek Downloads/aufgabe1.jpeg Downloads/10-million-password-list-top-100000.txt** 

ergab die Ausgabe:

**StegSeek 0.6 - https://github.com/RickdeJager/StegSeek**

**Found passfrase: "admin"
Original filename: "topsecret.zip".
Extracting to "aufgabe1.jpeg.out".**

![Foto Ubuntu Komandozeile, Stegseek](https://github.com/malin013/htb/blob/main/IMG_5827.jpg?raw=true)

### 2.2 Entschlüsselung der Flagge
Die verschlüsselte Flagge aus der extrahierten Datei ist als Hexadezimalzahl angegeben. Die erst vier Symbole "HTB{" sind aus dem Bsp. der Aufgabenstellung bekannt. Folglich kann der Schlüssel zu den ersten vier Hexadezimalzahlen (0x5B, 0x1E, 0xB4, 0x9A) errechnet werden. Nachfolgend wurde diese Sequenz XOR auf die restlichen Hexadezimalzahlen der verschlüsselten Flagge angewendet. Es wurde https://xor.pw/ für die weitere Berechnung verwendet und überprüft, ob das Ergebnis sinnvoll ist - dem ist so. 

**Ergebnis: HTB{rep34t3d_x0r_n0t_s0_s3cur3}**


## 3 Flagge

HTB{rep34t3d_x0r_n0t_s0_s3cur3}

  
## 4 Probleme
Zunächst hatte ich mit der Installation von Stekseek gekämpft. Die Steegseek-Befehle konnten nicht gefunden werden. Das Problem konnte jedoch mit einem direkten Download über https://github.com/RickdeJager/stegseek/releases behoben werden.

Das Formatieren in ein pdf-Dokument in CodiMD hat nicht funktioniert. Lösung: Installation von pandoc und texlive in Ubuntu.

## 5 Fazit/Feedback zur Aufgabe

Die Aufgabe war sehr lehrreich – allerdings auch sehr zeitaufwendig (>5h), da ich keine Vorkenntnisse in diesem Bereich hatte, sehr viel recherchieren und ausprobieren musste.