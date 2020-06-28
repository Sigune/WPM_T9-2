# MALIS 19.3 WPM_T9.2.2: Shell-Aufgabe

Autorin: Sigune Kussek

Aufgabenstellung: Die Datei "2020-05-23-Article_list_dirty.tsv" ist nicht sauber strukturiert und soll bereinigt werden. Mit einem Shell-Script soll eine neue "saubere" Datei erstellt werden, die zudem nur noch die zwei Spalten mit den ISSNs und den Veröffentlichungsjahren enthält. Name der neuen Datei: "2020-05-23-Dates_and_ISSNs.tsv"

## 1. Schritt: Download der "dirty" Datei

Die Datei "2020-05-23-Article_list_dirty.tsv" lade ich in den neu erstellten Ordner "dirty" und lege sie gleichzeitig in eine neue Datei namens "anders1.tsv" ab (so bleibt die Ursprungsdatei bei meinen weiteren Schritten unverändert):

(base) mia@mia-550P5C-550P7C:~/dirty
**$ cat 2020-05-23-Article_list_dirty.tsv > anders1.tsv**

## 2. Schritt: Die gewünschten zwei Spalten ausschneiden

Ich lasse mir mit dem Befehl *cat* den Inhalt der neuen Datei "anders1.tsv" anzeigen, um die Namen und die Position der einzelnen Spalten sehen zu können:
(base) mia@mia-550P5C-550P7C:~/dirty
**$ cat anders1.tsv**

Creator    Issue    Volume    Journal    ISSN    ID    Citation    Title    Place Labe    Language    Publisher    Date        
Andrews, E. M.    2    41    AUSTRALIAN JOURNAL OF POLITICS AND HISTORY    ISSN: 0004-9522    (Uk)RN000249208    AUSTRALIAN JOURNAL OF POLITICS AND HISTORY 41(2), 239-252. (1995)    """For Australia's Wartime Interests"": W. M. Hughes and the Push Against Asquith, Britain, March-July 1916"    at    eng    THE UNIVERSITY OF QUEENSLAND PRESS    1995        
Otte, T. G.    439    110    ENGLISH HISTORICAL REVIEW    0013-8266    (Uk)RN000490775    ENGLISH HISTORICAL REVIEW 110(439), 1157-1179. (1995)    Great Britain, Germany, and the Far-Eastern Crisis of 1897-8    xxk    eng    LONGMAN GROUP LIMITED    1995        
Nesbitt, E.    2    15    SOUTH ASIA RESEARCH    0262-7280    (Uk)RN000733465    SOUTH ASIA RESEARCH 15(2), 221-240. (1995)    Panjabis in Britain: Cultural History and Cultural Choices    xxk    eng    OXFORD UNIVERSITY PRESS    1995 \[...]

Jetzt kann ich erkennen, dass die Spalte "ISSN" an 5. Stelle und "Date" an 12. Stelle steht. Mit dem Befehl *cut* schneide ich die beiden Spalten aus und lege das Ergebnis in der neuen Datei "anders2.tsv" ab:

(base) mia@mia-550P5C-550P7C:~/dirty
**$ cat anders1.tsv | cut -f 5,12 > anders2.tsv**

Anschließend lasse ich mir mit *cat* den Inhalt der neuen Datei anzeigen:
**$ cat anders2.tsv**
ISSN    Date
ISSN: 0004-9522    1995
0013-8266    1995
0262-7280    1995
0022-4529    1995
0022-4529    1995

0022-1953    1996
1056-8190    1995
0030-8684    1995
0129-797X    1995
0018-2648    1996
ISSN:0265-6914    1996
0265-6914    1996
ISSN:0008-4107    1995

0950-9224    1995
0707-5332    1995
0019-7211    1996
0022-2801    1995
0022-2801    1995
0018-2613    1996
0968-7475    1995
0003-813X    1996
0013-2683    1995
6    eng
6    eng
27    eng
39    eng
29    eng
29    eng
29    eng
44    eng
0022-2801    1996
0022-2801    1996
0707-5332    1996
issn: 0707-5332    1996 \[...]

### Zwischenfazit:

Die Datei "anders2.tsv" enthält nur noch die gewünschten zwei Spalten "ISSN" und "Date". Zugleich ist deutlicher sichtbar, was bereinigt werden soll:

- Leerzeilen entfernen
- Doppelte Zeilen entfernen
- Alle Zeilen mit der Silbe "eng" entfernen
- Die drei verschiedenen Wörter "ISSN:", "issn:" und "Issn:", die einigen Zeilen vorangestellt sind, entfernen
- "ISSN" und "DATE" aus der Überschrift entfernen
- Die Tabelle nummerisch sorieren

## 3. Schritt

Als erstes möchte ich alle Zeilen mit der Silbe "eng" entfernen, da sie weder eine ISSN noch ein Publikationsjahr enthalten. Hierfür verwende ich die Befehle *grep* und *-v*, um nicht nur die Silbe, sondern die ganze Zeile aus der Tabelle rausnehmen zu können:

(base) mia@mia-550P5C-550P7C:~/dirty
**$ cat anders2.tsv | grep -v eng > anders3.tsv**

Überprüfen: **$ cat anders3.tsv**

-> hat geplappt: es fehlen alle "eng"-Zeilen

## 4. Schritt

Um die überflüssigen Wörter samt Doppelpunkt "ISSN:", "issn:" und "Issn:" zu entfernen, wandel ich zunächst alle Varianten in eine einheitliche, groß geschriebene Variante ("ISSN:") um:

(base) mia@mia-550P5C-550P7C:~/dirty
**$ tr \[:lower:] \[:upper:] < anders3.tsv > anders4.tsv**

Überprüfen: **$ cat anders4.tsv**

ISSN    DATE
ISSN: 0004-9522    1995
0013-8266    1995
0262-7280    1995
0022-4529    1995
0022-4529    1995 \[...]

Nun lösche ich alle "ISSN:"-Silben inklusive aller Leerzeichen:

(base) mia@mia-550P5C-550P7C:~/dirty
**$ tr -ds 'ISSN: ' '' < anders4.tsv > anders5.tsv**

Überprüfen: **$ cat anders5.tsv**

DATE
0004-9522    1995
0013-8266    1995
0262-7280    1995
0022-4529    1995
0022-4529    1995

0022-1953    1996
1056-8190    1995
0030-8684    1995
0129-797X    1995
0018-2648    1996
0265-6914    1996
0265-6914    1996
0008-4107    1995 

\[...]

Nun entferne ich noch das Wörtchen "DATE", indem ich einfach alle Zeilen (*-v*) mit *grep* rausnehme:

(base) mia@mia-550P5C-550P7C:~/dirty
**$ cat anders5.tsv | grep -v DATE > anders6.tsv**

Überprüfen: **$ cat anders6.tsv**

0004-9522    1995
0013-8266    1995
0262-7280    1995
0022-4529    1995
0022-4529    1995

0022-1953    1996
1056-8190    1995
0030-8684    1995
0129-797X    1995
0018-2648    1996
0265-6914    1996
0265-6914    1996
0008-4107    1995

\[... ]

## 5.  und letzter Schritt

Als letzten Schritt möchte ich die schon weit bereinigte Datei noch nach Nummern sortieren und zugleich doppelte Zeilen sowie alle Leerzeilen löschen. Hierfür verwende ich den Befehl *sort -u*:

(base) mia@mia-550P5C-550P7C:~/dirty
**$ sort -u anders6.tsv > 2020-05-23-Dates_and_ISSNs.tsv**

Überprüfen: **$ cat 2020-05-23-Dates_and_ISSNs.tsv**

0003-598X 1996
0003-813X 1996
0004-9522 1995
0008-4107 1995
0008-4107 1996
0009-4455 1995
0010-4175 1996
0011-3212 1996
0012-8449 1996
0013-2683 1995
0013-8266 1995
0013-8266 1996
0018-246X 1996
0018-2613 1996
0018-2648 1996
0018-2753 1996
0019-7211 1996
0020-8590 1996
0021-8537 1996
0021-9118 1996
0022-0094 1996
0022-1953 1996
0022-2801 1995
0022-2801 1996
0022-4529 1995
0022-4529 1996
0022-4634 1996
0022-5037 1996
0023-656X 1996
0029-3652 1995
0030-8684 1995
0037-6779 1996
0037-6795 1996
0048-8003 1995
0079-4848 1996
0095-1390 1995
0095-1390 1996
0129-797X 1995
0142-7962 1996
0143-781X 1996
0262-7280 1995
0265-6914 1996
0265-7325 1996
0305-8034 1996
0313-6221 1996
0315-7997 1996
0707-5332 1995
0707-5332 1996
0950-9224 1995
0968-7475 1995
0968-7475 1996
1056-8190 1995
1069-5834 1996
1078-6279 1996
1331-3134 1994
1350-7486 1996
1361-2343 1994
1361-9462 1994

Ja, es hat funktioniert: Die Datei **2020-05-23-Dates_and_ISSNs.tsv** ist, wie laut Aufgabe gewünscht, bereinigt!

Ein letzter Test zum Überprüfen, ob ich wirklich nichts übersehen habe: Ich vergleiche mit *diff* die Musterlösung des Dozenten mit meinem Ergebnis:

(base) mia@mia-550P5C-550P7C:~/dirty
**$ diff 2020-05-23-Dates_and_ISSNs_v1.tsv 2020-05-23-Dates_and_ISSNs.tsv**
0a1

Es wurden keine Unterschiede gefunden!

## Ergebnis:

Shell-Script zur Bereinigung der "dirty" Datei **2020-05-23-Article_list-dirty.tsv**, die nur die zwei Spalten mit den ISSNs und den Publikationsjahren enthält:

**$ cat 2020-05-23-Article_list_dirty.tsv > anders1.tsv**

**$ cat anders1.tsv | cut -f 5,12 > anders2.tsv**

**$ cat anders2.tsv | grep -v eng > anders3.tsv**

**$ tr [:lower:] [:upper:] < anders3.tsv > anders4.tsv**

**$ tr -ds 'ISSN: ' '' < anders4.tsv > anders5.tsv**

**$ cat anders5.tsv | grep -v DATE > anders6.tsv**

**$ sort -u anders6.tsv > 2020-05-23-Dates_and_ISSNs.tsv**

#### Quellen:

https://wiki.ubuntuusers.de

https://librarycarpentry.org/lc-shell/06-free-text/index.html

Lernvideos zur Bash von Konrad Förstner (MALIS 19.3 WPM T9)
