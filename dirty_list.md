# MALIS 19.3 WPM_T9.2.2: Shell-Aufgabe
Autorin: Sigune Kussek

Aufgabenstellung: Die Datei "2020-05-23-Article_list_dirty.tsv" ist nicht sauber strukturiert und soll bereinigt werden. Mit einem Shell-Script soll ein neue "saubere" Datei erstellt werden, die zudem nur noch die zwei Spalten mit den ISSNs und den Veröffentlichungsjahren enthält. Name der neuen Datei: "2020-05-23-Dates_and_ISSNs.tsv"

## 1. Schritt: Download der "dirty" Datei
Die Datei "2020-05-23-Article_list_dirty.tsv" lade ich in den Ordner "dirty" hinunter und lege sie gleichzeitig in eine neue Datei "anders1.tsv" ab (so bleibt die Ursprungsdatei bei meinen weiteren Schritten unverändert):

(base) mia@mia-550P5C-550P7C:~/dirty **$ cat 2020-05-23-Article_list_dirty.tsv > anders1.tsv**

## 2. Schritt: Die gewünschten zwei Spalten ausschneiden
Ich lasse mir den Inhalt der neuen Datei "anders1.tsv" anzeigen, um die Namen und die Position der einzelnen Spalten sehen zu können:
(base) mia@mia-550P5C-550P7C:~/dirty **$ cat anders1.tsv**

Creator	Issue	Volume	Journal	ISSN	ID	Citation	Title	Place Labe	Language	Publisher	Date		
Andrews, E. M.	2	41	AUSTRALIAN JOURNAL OF POLITICS AND HISTORY	ISSN: 0004-9522	(Uk)RN000249208	AUSTRALIAN JOURNAL OF POLITICS AND HISTORY 41(2), 239-252. (1995)	"""For Australia's Wartime Interests"": W. M. Hughes and the Push Against Asquith, Britain, March-July 1916"	at	eng	THE UNIVERSITY OF QUEENSLAND PRESS	1995		
Otte, T. G.	439	110	ENGLISH HISTORICAL REVIEW	0013-8266	(Uk)RN000490775	ENGLISH HISTORICAL REVIEW 110(439), 1157-1179. (1995)	Great Britain, Germany, and the Far-Eastern Crisis of 1897-8	xxk	eng	LONGMAN GROUP LIMITED	1995		
Nesbitt, E.	2	15	SOUTH ASIA RESEARCH	0262-7280	(Uk)RN000733465	SOUTH ASIA RESEARCH 15(2), 221-240. (1995)	Panjabis in Britain: Cultural History and Cultural Choices	xxk	eng	OXFORD UNIVERSITY PRESS	1995 \[...]

Jetzt kann ich sehen, dass die Spalte "ISSN" an 5. Stelle und "Date" an 12. Stelle steht. Mit dem Befehl *cut* schneide ich die beiden Spalten aus und lege das Ergebnis in der neuen Datei "anders2.tsv" ab:

(base) mia@mia-550P5C-550P7C:~/dirty **$ cat anders1.tsv | cut -f 5,12 > anders2.tsv**

Dann lasse ich mir mit *cat* den Inhalt der neuen Datei anzeigen:
(base) mia@mia-550P5C-550P7C:~/dirty **$ cat anders2.tsv**

ISSN	Date
ISSN: 0004-9522	1995
0013-8266	1995
0262-7280	1995
0022-4529	1995
0022-4529	1995




0022-1953	1996
1056-8190	1995
0030-8684	1995
0129-797X	1995
0018-2648	1996
ISSN:0265-6914	1996
0265-6914	1996
ISSN:0008-4107	1995
	
	
	
	
0950-9224	1995
0707-5332	1995
0019-7211	1996
0022-2801	1995
0022-2801	1995
0018-2613	1996
0968-7475	1995
0003-813X	1996
0013-2683	1995
6	eng
6	eng
27	eng
39	eng
29	eng
29	eng
29	eng
44	eng
0022-2801	1996
0022-2801	1996
0707-5332	1996
issn: 0707-5332	1996 \[...]

neu: Test
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


### Zwischenfazit:
Die Datei "anders2.tsv" enthält nur noch die gewünschten zwei Spalten "ISSN" und "Date". Zugleich ist nun deutlicher sichtbar, was bereinigt werden soll:
- Leerzeilen entfernen
- Doppelte Zeilen entfernen
- Alle Zeilen mit der Silbe "eng" entfernen
- Die drei verschiedenen Wörter "ISSN:", "issn:" und "Issn:", die einigen Zeilen vorangestellt sind, entfernen
- Die Tabelle nummerisch sorierten

