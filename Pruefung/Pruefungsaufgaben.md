# xReality Technologien – Kombinationsprüfung

**Hochschule Bielefeld · Software Engineering (PI) · Wintersemester 2026/2027**

**Prüfungsaufgaben für Studierende**

Dieses Dokument definiert die Aufgaben, Abgaben und Bewertungskriterien der Kombinationsprüfung. Ergänzende organisatorische Angaben und die noch zu bestätigenden Prüfungsmodalitäten sind in Abschnitt 6 aufgeführt und werden vor Beginn der Prüfungsbearbeitung bekannt gegeben. Maßgeblich bleiben die geltenden Hochschul- und Prüfungsregelungen.

## 1. Überblick

| Prüfungsteil | Arbeitsform | Punkte | Abgabe |
|---|---|---:|---|
| 1: Interaktive XR-Sortierstation | Einzelarbeit | 30 | 15.01.2027, 23:59 Uhr |
| 2: Eigenständige XR-Erfahrung | Vorzugsweise Zweiergruppe, nach Abstimmung maximal drei Personen | 70 | 15.01.2027, 23:59 Uhr |
| **Gesamt** | | **100** | |

Teil 1 weist Ihre individuellen XR-Grundkenntnisse nach. In Teil 2 entwickeln Sie gemeinsam eine eigene Anwendung und begründen, testen und dokumentieren Ihre Entscheidungen.

**Arbeitsaufwand als Orientierung:** Außerhalb der Präsenz sind insgesamt 80 Stunden pro Person vorgesehen: etwa 20 Stunden für Vor- und Nachbereitung, 18 Stunden für Teil 1 und 42 Stunden für Teil 2 einschließlich Tests und Dokumentation. Betreute Präsenzzeit kommt hinzu. Dies sind Planungswerte, keine nachzuweisenden Mindeststunden. In Woche 8–10 steht die vorgesehene Betreuung für die Prüfungsprojekte zur Verfügung; zusätzliche Lehrinhalte oder Prüfungsvorträge sind dort nicht vorgesehen.

## 2. Technische Rahmenbedingungen

- **Zielgerät:** Meta Quest 3; jeder Person steht ein Gerät zur Verfügung.
- **Unity:** 6000.3.18f1 LTS. Eine neuere Version ist bei einem konkreten technischen Problem nach vorheriger Abstimmung erlaubt. Dokumentieren Sie Anlass, Version und Ergebnis der Überprüfung.
- **XR-Stack:** frei wählbar. Dokumentieren Sie SDKs, Pakete und Versionen. Der reguläre technische Support orientiert sich am Referenzprojekt des Kurses; die Integration anderer Stacks liegt in Ihrer Verantwortung.
- **Teil 1:** Ein lauffähiges Unity-Projekt ist Pflicht. Die Ausführung im Editor beziehungsweise über eine geeignete XR-Anbindung ist zulässig. Eine Standalone-APK bringt Technikpunkte, ist aber keine Bestehensvoraussetzung.
- **Teil 2:** Erwartet wird eine installierbare Android-APK, die autonom auf der Quest 3 läuft. Für reproduzierbar gescheiterte Kompilierung gilt die Ausnahme in Abschnitt 4.6.
- **Standalone** bedeutet: Für die Ausführung werden weder Unity Editor noch Quest Link, Air Link, PC-VR oder ein angeschlossener Entwicklungsrechner benötigt.
- Bei Abgabe einer APK müssen Projekt und APK denselben Stand abbilden. Die APK muss frisch installierbar sein; setzen Sie keine nur auf Ihrem Entwicklungsgerät vorhandenen Zusatzdateien voraus.
- Beschreiben Sie Startweg, Einrichtung und erforderliche Berechtigungen. Der bewertete Kernablauf soll ohne externe Server, kostenpflichtige Dienste oder persönliche Zugangsdaten funktionieren. Optionale Netzwerkfunktionen stimmen Sie vorher ab.

Ein Video ersetzt kein ausführbares Projekt und keine erforderliche Fehlerdokumentation.

## 3. Prüfungsteil 1: Interaktive XR-Sortierstation – 30 Punkte

### 3.1 Ihre Aufgabe

Entwickeln Sie eigenständig eine kleine XR-Anwendung mit **drei physikalischen, greifbaren Objekten und jeweils passenden Zielbereichen**. Richtige Zuordnungen werden erkannt und verständlich angezeigt. Fortschritt und Abschluss sind erkennbar. Ein einfacher vollständiger Reset ermöglicht einen neuen Durchlauf.

Als Szenario eignet sich beispielsweise eine Werkbank mit drei Bauteilen und passenden Ablagen. Einfache geometrische Formen reichen aus. Bewertet wird eine funktionierende kleine Anwendung, nicht eine aufwendige Spielwelt.

Wählen Sie **eine** der folgenden Varianten. Die Varianten sind gleichwertig; eine technisch aufwendigere Wahl gibt nicht automatisch mehr Punkte.

#### Variante A: VR mit Fortbewegung

- Die Werkbank befindet sich in einer virtuellen Umgebung.
- Mindestens eine virtuelle Fortbewegungsart, beispielsweise Teleportation, ermöglicht es, die Werkbank von einer geeigneten Startposition aus zu erreichen. Ausschließlich reales Gehen genügt nicht.
- Ein einfacher sicher nutzbarer Bewegungsbereich beziehungsweise ein geeignetes Teleportationsziel reicht aus.
- Beschreiben Sie die Bedienung. Bodenhöhe und Erreichbarkeit bleiben nach der Bewegung plausibel.

Mehrere Bewegungsarten oder eine umfassende Behandlung ungültiger Bewegungsziele sind nicht erforderlich.

#### Variante B: AR mit manueller Platzierung

- Die reale Umgebung ist über Passthrough sichtbar.
- Die nutzende Person legt die Position der virtuellen Werkbank zur Laufzeit fest und bestätigt sie. Eine nur im Editor festgelegte Position genügt nicht.
- Einmaliges Platzieren zu Beginn reicht aus. Vorschau, frei einstellbare Ausrichtung und erneute Platzierung im laufenden Betrieb sind nicht gefordert. Für eine neue Platzierung darf die Anwendung neu gestartet werden.
- Objekte und Zielbereiche sind relativ zur Werkbank korrekt angeordnet. Der Aufgabenreset stellt diesen Ausgangszustand wieder her.

#### Variante C: AR mit automatischer Tischplatzierung

Diese technisch aufwendigere Variante ist eine freiwillige Alternative für Interessierte, keine Zusatzaufgabe zu Variante B.

- Die reale Umgebung ist über Passthrough sichtbar.
- Raumdaten mit geeigneter Flächen- beziehungsweise Tischklassifikation werden verwendet, um Objekte und Zielbereiche automatisch auf einem erkannten realen Tisch anzuordnen.
- Die Anordnung berücksichtigt die nutzbare Fläche. Passende Collider unterstützen die physikalischen Objekte.
- Dokumentieren Sie notwendige Raumeinrichtung, Berechtigungen und Anforderungen an einen geeigneten Tisch. Ein funktionierender Ablauf in einem entsprechend vorbereiteten Raum genügt.

Eine fest kodierte Position oder ein lediglich virtuell modellierter Tisch ersetzt die Erkennung nicht. Umfassende Behandlung fehlender Raumdaten, Berechtigungen oder ungeeigneter Tische wird nicht verlangt.

**Für beide AR-Varianten:** Die Platzierung bleibt während der Aufgabe räumlich stabil und sicher erreichbar. Eine Speicherung über Anwendungsneustarts hinweg ist nicht erforderlich. Prüfen Sie bei Nutzung einer Editor-/XR-Anbindung frühzeitig, ob diese Passthrough und gegebenenfalls Raumdaten tatsächlich unterstützt.

### 3.2 Gemeinsamer Funktionsumfang

| Bereich | Erwartete Funktion |
|---|---|
| XR-Aufbau | Funktionierendes Tracking, plausibler Maßstab, sinnvoll positionierte und erreichbare Station |
| Interaktion und Physik | Drei Objekte greifen, bewegen und loslassen; passende Collider und Rigidbodies für grundsätzlich plausibles Fallen und Kollidieren |
| Zuordnung | Passende und unpassende Zuordnungen unterscheiden; falsches Ablegen zählt nicht, richtige Zuordnungen werden nicht mehrfach gezählt |
| Feedback | Ein verständlicher Feedbackkanal zeigt richtige Zuordnungen, Fortschritt und Abschluss, beispielsweise farbige Zielbereiche oder drei Statusanzeigen |
| Reset | Vollständiger Neustart der Aufgabe ohne Editor-Eingriff, einschließlich Rücksetzen verlorener Objekte; eine dokumentierte Controllertaste genügt |

**Bewusst nicht gefordert:** mehrere Feedbackkanäle, World-Space-Canvas, interaktives Tutorial, automatische Objektrückholung, Handtracking, Multiplayer, eigene 3D-Modelle oder mehrere Szenen. Ein gut gestalteter Feedbackkanal genügt auch für volle Punkte.

Standardkomponenten Ihres XR-Frameworks für Greifen, Fortbewegung und Platzierung dürfen Sie verwenden. Ein eigenes Interaktionssystem wird nicht erwartet. Die aufgabenspezifische Zuordnungs-, Fortschritts- und Resetlogik setzen Sie eigenständig um. Verwendete Kursgrundlagen und Hilfsmittel kennzeichnen Sie gemäß Abschnitt 5.

### 3.3 Testcheckliste

Dokumentieren Sie zu diesen fünf Prüffällen jeweils kurz das Ergebnis:

1. Projekt über den beschriebenen Ausführungsweg starten.
2. Fortbewegung beziehungsweise Platzierung durchführen und die Werkbank bedienen.
3. Ein Objekt falsch ablegen und anschließend richtig zuordnen: Nur die richtige Zuordnung zählt.
4. Alle drei Objekte zuordnen und den Abschluss prüfen: Jedes Objekt zählt nur einmal.
5. Vollständigen Reset auslösen und erneut spielen; auch ein zuvor verlorenes Objekt steht wieder bereit.

Bei Abgabe einer APK testen Sie zusätzlich ihre frische Installation (also vorher alte Installation deinstallieren) und autonome Ausführung auf der Quest 3. Typische Bedienfälle wie wiederholte Kontakte oder ein Reset während der Bearbeitung können zur Qualitätsbewertung geprüft werden, verlangen aber keinen umfangreichen Testbericht. Erweiterte Prüfungen zu Neuplatzierung oder fehlenden Raumdaten sind freiwillig.

### 3.4 Abgabecheckliste

- [ ] Unity-Projekt mit `Assets`, `Packages` und `ProjectSettings`, ohne generierte Ordner wie `Library`.
- [ ] Knappe README mit Variante, Bedienung, eigener Logik und bekannten Problemen; keine vorgegebene Seitenzahl.
- [ ] Testcheckliste mit Ergebnissen; sie darf Teil der README sein.
- [ ] Quellen-, Asset- und Hilfsmittelangaben sowie Eigenständigkeitserklärung nach Hochschulvorgaben.
- [ ] Optional für Technikpunkte: installierbare, autonom lauffähige Quest-3-APK.

Ein Demonstrationsvideo ist freiwillig; sein Fehlen führt zu keinem Punktabzug. Auch ist kein git-Repo mit Versionsverlauf notwendig.

### 3.5 Bewertung

| Kriterium | Punkte | Ausreichende Grundfunktion | Sehr gute Umsetzung |
|---|---:|---|---|
| XR-Aufbau, Greifen/Ablegen und Physik | 7,5 | Szene im dokumentierten Setup nutzbar; drei greifbare Objekte mit grundsätzlich plausibler Physik (Collider, Gravitation) in plausiblem Maßstab | Zuverlässige Interaktion auch bei wiederholter Nutzung |
| VR-Fortbewegung oder AR-Platzierung | 6 | Gewählte Variante funktioniert; notwendige Voraussetzungen/Raum-Einrichtung beschrieben | Lösung zuverlässig, verständlich, komfortabel und räumlich sinnvoll umgesetzt |
| Zuordnungslogik, Fortschritt und Reset | 6 | Zuordnungen unterschieden, Abschluss erkennbar, vollständiger Reset funktioniert | Konsistente Zustände auch bei wiederholtem Ablegen und Mehrfachkontakten; Fortschritt gut erkennbar |
| Verständlichkeit, Feedback und Komfort | 6 | Bedienung mit kurzer Anleitung möglich; ein verständlicher Feedbackkanal | Klare Anleitung, eindeutige konsistente Rückmeldungen und komfortable Bedienung; keine zusätzlichen Kanäle nötig |
| Technik, Tests und Dokumentation einschließlich APK | 4,5 | Nachvollziehbare Einrichtung, Quellen und Tests; Punkte nach Aufteilung unten | Präzise kompakte README, aussagekräftige Testcheckliste, nachvollziehbare Lösung und lauffähige Standalone-APK |
| **Summe** | **30** | | |

Für die ersten vier Kriterien entspricht eine ausreichende Grundfunktion ungefähr der Hälfte der jeweiligen Maximalpunkte. Fehlende oder wesentlich mangelhafte Leistungen liegen darunter; zunehmende Vollständigkeit und Zuverlässigkeit ergeben höhere Punktzahlen. Viertelpunkte sind zulässig. Derselbe Fehler wird nicht pauschal mehrfach abgezogen; maßgeblich sind seine nachweisbaren Auswirkungen.

**Aufteilung der 4,5 Technikpunkte:**

- **3 Punkte** für technische Nachvollziehbarkeit, Tests und Dokumentation. Eine ausreichende Ausarbeitung erhält die Hälfte, eine präzise und reproduzierbare Ausarbeitung die volle Punktzahl. Ein umfangreicher Bericht ist nicht erforderlich.
- **1,5 Punkte** für eine frisch installierbare, autonom auf der Quest 3 laufende APK mit funktionierendem Kernablauf und passendem Projektstand. Ohne APK oder bei nicht installierbarer beziehungsweise im Kernablauf nicht lauffähiger APK gibt es hierfür 0 Punkte; kleinere Mängel erlauben Teilpunkte.

Ohne APK erfolgen keine weiteren Abzüge. In Teil 1 bleiben **28,5 von 30 Punkten** erreichbar; die Gesamtnote ergibt sich aus beiden Prüfungsteilen gemäß Abschnitt 6. Die geforderten Funktionen müssen im dokumentierten Ausführungsweg tatsächlich interaktiv prüfbar sein.

### 3.6 Bestehen von Teil 1

Für das Bestehen von Teil 1 sind mindestens **15 von 30 Punkten** und folgende Mindestanforderungen zu erfüllen:

1. Lauffähiges Unity-Projekt mit reproduzierbar ausführbarem Kernablauf und XR-Funktionen; keine APK-Pflicht.
2. Eine funktionsfähige Variante A, B oder C.
3. Drei greifbare und zuordbare Objekte; richtige und falsche Zuordnungen werden unterschieden, keine Mehrfachzählung.
4. Erkennbarer Fortschritt und Abschluss sowie vollständiger Reset einschließlich verlorener Objekte ohne Editor-Eingriff.
5. Mindestens ein verständlicher Feedbackkanal.
6. Ausreichende Start-/Bedienanleitung und Kennzeichnung bewertungsrelevanter Übernahmen und Hilfsmittel.

Eine ausreichende Leistung bietet einen einfachen funktionierenden Ablauf mit möglichen qualitativen Schwächen. Es wird keine Fehlerfreiheit verlangt. Zusätzliche Effekte können fehlende Grundlagen jedoch nicht kompensieren.

**Qualität statt Featuremenge:** Höhere Punktzahlen ergeben sich aus einer zuverlässigen Umsetzung derselben kleinen Aufgabe, komfortabler Bedienung und präziser knapper Dokumentation – nicht aus zusätzlichen Funktionen, automatischer Raumerkennung oder aufwendiger Grafik. Für Teil 1 wird keine separate Teilnote berechnet. Die einzige Notenskala für die Gesamtprüfung steht in Abschnitt 6.

## 4. Prüfungsteil 2: Eigenständige XR-Erfahrung – 70 Punkte

### 4.1 Ihre Aufgabe und Projektumfang

Entwickeln Sie in einer Zweiergruppe, nach Abstimmung maximal zu dritt, eine eigenständige XR-Anwendung in Unity. Sie soll ein klares Nutzungsziel verfolgen und räumliche Interaktion sinnvoll einsetzen. Spiele, Lernanwendungen, Trainingssimulationen und interaktive Exponate sind gleichermaßen möglich.

Stimmen Sie Thema, Mindestumfang und fachliche Vertiefung zu Beginn mit der Lehrperson ab. Planen Sie einen begrenzten vollständigen Kernablauf, als Orientierung etwa 3–5 Minuten. Bei wiederholbaren Anwendungen zählt ein vollständiger Durchlauf, keine künstliche Mindestspielzeit.

Pro Person sind ungefähr 42 Stunden außerhalb der Präsenz vorgesehen: insgesamt etwa 84 Personenstunden im Zweierteam beziehungsweise 126 im Dreierteam. Bei drei Personen wird ein angemessen größerer Umfang vereinbart, etwa eine zusätzliche integrierte Mechanik oder eine vertiefte Evaluation. Multiplayer ist nicht automatisch erforderlich.

### 4.2 Anforderungen

1. **Vollständiger Kernablauf:** verständlicher Einstieg, zentrale Tätigkeit und erkennbarer Abschluss beziehungsweise wiederholbarer Ablauf.
2. **Mindestens zwei unterschiedliche Interaktionsmechaniken:** etwa Greifen und gezieltes Einsetzen, Werkzeugbedienung, Hebel oder Schublade, räumliches Bedienelement, Zusammenfügen oder Werfen und Treffen. Mehrere lediglich greifbare Objekte zählen nicht als verschiedene Mechaniken. Fortbewegung ist nur erforderlich, wenn sie zum Konzept passt.
3. **Nachvollziehbare Anwendungszustände:** Fortschritt, Ergebnis, Fehlerbehandlung und Neustart statt unverbundener Interaktionsdemos.
4. **Feedback und Komfort:** lesbares räumliches UI, passende Rückmeldungen und sichere, möglichst komfortable Bedienung. Begründen Sie Ihre Entscheidungen.
5. **Eine fachliche Vertiefung:** beispielsweise physikalische Werkzeuginteraktion, räumliches Audio, MR-Platzierung, sitzende Bedienung, adaptives Onboarding oder Bewegungsaufzeichnung mit Trainingsfeedback.
6. **Evaluation und Überarbeitung:** Führen Sie einen kleinen formativen Test durch, beispielsweise mit drei Personen außerhalb Ihres Teams. Dokumentieren Sie Testfrage, Aufgaben, Beobachtungen und mindestens eine daraus abgeleitete Verbesserung. Repräsentative Aussagen werden nicht erwartet.

Mögliche Themen sind eine VR-Reparaturstation, ein kleiner Escape Room mit zwei zusammenhängenden Rätseln, eine Lernstation zum Zusammensetzen eines Modells, ein MR-Tischspiel, ein Bowlingtrainer oder ein interaktives Museumsexponat. Auch vorstellbar ist ein virtueller HSBI-Neubau-Simulator oder MR Umsetzung des HMI Labors.

Sie dürfen das Kurs-Bowling substanziell weiterentwickeln. Grenzen Sie Ausgangsstand und Eigenleistung klar ab. Neue Farben, Modelle oder einzelne Soundeffekte allein reichen nicht als Vertiefung. Netzwerk-Multiplayer, vollständiges Handtracking und realistische Spin-Simulation sind keine Pflichtanforderungen und sollten nur nach Umfangsabstimmung eingeplant werden.

### 4.3 Fortlaufende Projektdokumentation

Dokumentieren Sie während der Entwicklung, nicht erst am Ende. Ein Markdown-Bereich im Repository genügt; ein separates Wiki ist nicht erforderlich. Dokumentieren Sie die folgenden Inhalte knapp und nachvollziehbar. Nutzen Sie geeignete Abbildungen und Tabellen zur Veranschaulichung. Es gibt keine Mindestseitenzahl; bewertet werden Vollständigkeit, Verständlichkeit und die Begründung Ihrer Entscheidungen.

| Abschnitt | Erwarteter Inhalt |
|---|---|
| Projektübersicht | Titel, aktueller Screenshot, Kurzbeschreibung, Team, Rollen, Zielgerät und Video auf einen Blick |
| Konzept | Zielgruppe, Nutzungsziel, XR-Mehrwert, Kernablauf, Mindestumfang und bewusste Abgrenzungen |
| Ausgangsstand und Eigenleistung | Übernommene Grundlagen, Änderungen und neue Bestandteile |
| Technische Umsetzung | Komponenten, Zustände, XR-Stack und wesentliche Implementierungsentscheidungen |
| Interaktionsgestaltung | Bedienung, Feedback, räumliches UI, Komfort sowie beabsichtigte und erreichte Wirkung |
| Entwicklungsverlauf | Meilensteine, Entscheidungen, Probleme, Lösungen und Umfangsänderungen |
| Evaluation | Testfrage, Aufgaben, Ablauf, Beobachtungen, Grenzen und umgesetzte Verbesserungend |
| Beiträge und Reflexion | Individuelle Beiträge, grober Zeitaufwand, Lernerfahrungen und offene Probleme |
| Betrieb und Quellen | Build-/Startanleitung, Versionen, Berechtigungen, Einschränkungen, Assets, Lizenzen und Hilfsmittel |

Halten Sie drei Entwicklungsstände nachvollziehbar fest:

1. **Konzept:** Was entwickeln Sie für wen, und was ist der Mindestumfang?
2. **Spielbarer Kern:** Was funktioniert, welche Risiken sind aufgetreten und was ändern Sie?
3. **Abschluss:** Was wurde umgesetzt, getestet und verbessert?

Diese Stände werden in der vorgesehenen Betreuung besprochen. Es sind keine zusätzlichen eigenständigen Teilprüfungen oder Präsentationstermine.

### 4.4 Individueller Beitrag

Jede Person übernimmt einen substanziellen technischen Beitrag. Eine ausschließlich organisatorische, grafische oder dokumentierende Rolle genügt nicht. Rollen benennen Verantwortlichkeiten, schließen gemeinsame Arbeit aber nicht aus.

Geben Sie pro Person eine Beitragsbeschreibung von ungefähr 1-2 Seiten ab:

- verantwortete und gemeinsam bearbeitete Funktionen;
- mindestens eine wichtige technische oder gestalterische Entscheidung;
- ein aufgetretenes Problem und dessen Bearbeitung;
- Beteiligung an Tests und Überarbeitung;
- Reflexion des eigenen Lernfortschritts.

Verweisen Sie auf passende Code-, Aufgaben-, Commit- oder Dokumentationsstellen. Pair Programming ist erlaubt und als gemeinsame Arbeit zu kennzeichnen. Commit-Anzahl, Codezeilen und Arbeitsstunden sind keine direkten Leistungsmaße.

### 4.5 Bewertung

| Kriterium | Punkte | Zuordnung |
|---|---:|---|
| Konzept und sinnvolle XR-Nutzung | 8 | Gemeinsam |
| Technische Umsetzung und Robustheit | 17 | Gemeinsam |
| Interaktionsgestaltung, Feedback und Komfort | 12 | Gemeinsam |
| Evaluation und nachvollziehbare Überarbeitung | 9 | Gemeinsam |
| Gemeinsame Projektdokumentation und Reproduzierbarkeit | 4 | Gemeinsam |
| Individueller Beitrag und fachliche Reflexion | 20 | Individuell |
| **Summe** | **70** | |

Es werden 50 Punkte für das gemeinsame Ergebnis und 20 für den individuellen Beitrag vergeben. Die Punktzahl in Teil 2 kann sich dadurch zwischen den Teammitgliedern unterscheiden.

Die Dokumentation dient auch als Bewertungsgrundlage für Konzept, Technik, Gestaltung und Evaluation. Die vier gesonderten Dokumentationspunkte betreffen ihre übergreifende Qualität und die Reproduzierbarkeit. Ein guter Bericht ersetzt keine nachweisbare Umsetzung.

Zum Bestehen von Teil 2 sind pro Person mindestens **35 von 70 Punkten** erforderlich. Dafür werden die erreichten gemeinsamen und individuellen Punkte addiert. Für Teil 2 wird keine separate Teilnote berechnet; die Gesamtnote wird gemäß Abschnitt 6 ermittelt.

### 4.6 Wenn die APK-Kompilierung scheitert

Erwartet wird eine installierbare, autonom auf der Quest 3 lauffähige APK. Sollte die Kompilierung trotz nachvollziehbarer Lösungsversuche nicht gelingen, können Sie stattdessen das Unity-Projekt mit einer **nachvollziehbaren und reproduzierbaren Fehlerdokumentation** abgeben. Das führt nicht automatisch zum Nichtbestehen.

Reproduzieren Sie den Kompilierungsfehler mit **demselben abgegebenen Projektstand auf mindestens zwei unterschiedlichen Computern**. Dokumentieren Sie für beide Rechner:

- Betriebssystem, Unity-Version, relevante Paketversionen und Android-Buildwerkzeuge;
- Build-Einstellungen und konkrete Reproduktionsschritte aus dem abgegebenen Projekt;
- Fehlermeldungen und relevante Build-Logs, eindeutig dem jeweiligen Rechner zugeordnet;
- die Stelle des Scheiterns sowie die belegbare Ursache oder den eingegrenzten Fehlerstand;
- Lösungsversuche und deren Ergebnisse;
- einen Ausführungsweg, über den die funktionierenden Projektbestandteile geprüft werden können.

Eine Bildschirmaufnahme kann helfen, Ablauf und Fehler zu demonstrieren. Sie ersetzt nicht die notwendigen Angaben und Logs. „Build funktioniert nicht“ allein genügt nicht. Ist die Ursache ungeklärt, benennen Sie das ausdrücklich, statt eine Vermutung als gesicherte Erklärung darzustellen.

Bewertet werden die nachweisbare technische Umsetzung sowie Fehleranalyse und Dokumentation innerhalb des Technikrasters. Bei entsprechendem Nachweis ist weder ein pauschaler zusätzlicher Abzug noch ein automatisches Nichtbestehen allein wegen der fehlenden APK vorgesehen.

Diese Ausnahme betrifft die Kompilierung. Fehler bei Installation oder Ausführung einer erfolgreich erzeugten APK werden nach ihren Auswirkungen auf Funktionalität und Robustheit bewertet; auch sie führen nicht allein aufgrund dieser Regel automatisch zum Nichtbestehen.

### 4.7 Abgabecheckliste

- [ ] Installierbare Quest-3-APK oder Fehlerdokumentation nach Abschnitt 4.6 mit Nachweisen von mindestens zwei Computern.
- [ ] Unity-Projekt desselben Stands mit `Assets`, `Packages` und `ProjectSettings` inkl. aller `.meta` Dateien, ohne generierte Ordner wie `Library`.
- [ ] Gemeinsame fortlaufende Projektdokumentation einschließlich Evaluation und Überarbeitung.
- [ ] Demonstrationsvideo von etwa 3–5 Minuten.
- [ ] Individuelle Beitragsbeschreibung für jede Person.
- [ ] Git-Repository einschließlich Versionshistorie (`.git`-Verzeichnis) mit eindeutig markiertem Abgabestand, enthalten im ZIP-Archiv.
- [ ] Quellen-, Lizenz-, Hilfsmittel- und Eigenständigkeitsangaben nach Hochschulvorgaben.

Am letzten Kurstermin vor Weihnachten findet eine unbewertete Abschlussrunde statt, in der die Teams sich gegenseitig den aktuellen Stand ihrer Projekte vorführen. Ein fertiges Projekt wird zu diesem Zeitpunkt nicht vorausgesetzt. Die Abschlussrunde ist keine zusätzliche bewertete Prüfungsleistung und ersetzt nicht die spätere Abgabe.

## 5. Eigenständigkeit und Hilfsmittel

### Einzelarbeit und Zusammenarbeit

Teil 1 bearbeiten Sie eigenständig. Allgemeiner fachlicher Austausch und die Besprechung von Fehlermeldungen sind erlaubt. Nicht erlaubt sind die gemeinsame Implementierung der bewerteten Aufgabenlogik, die Übernahme von Lösungen anderer Studierender oder die Abgabe eines im Wesentlichen fertigen Fremdprojekts.

Im Gruppenprojekt ist gemeinsame Implementierung erlaubt; machen Sie die jeweiligen und gemeinsamen Beiträge nachvollziehbar.

### Kursmaterialien und Wiederverwendung

Sie dürfen Kursgrundlagen wie XR-Konfiguration, Eingabeanbindung und allgemeine Hilfskomponenten wiederverwenden. 

- In Teil 1 bleibt die aufgabenspezifische Zuordnungs-, Fortschritts- und Resetlogik Ihre eigene Leistung.
- In Teil 2 werden die ausgewiesenen eigenen Erweiterungen, ihre Integration, Gestaltung, Evaluation und Dokumentation bewertet. Unverändert übernommene Kursbestandteile zählen nicht als eigenständige Entwicklung.

### Externe Assets, Tutorials und KI

Externe Assets, Bibliotheken, Tutorials und KI-Werkzeuge dürfen unterstützend eingesetzt werden. Dokumentieren Sie Verwendung und Umfang; beachten Sie Lizenz- und Nutzungsbedingungen. Sie tragen die Verantwortung für Funktion, Verständlichkeit und Überprüfung aller abgegebenen Bestandteile. Eine lediglich umgestaltete fertige Fremdanwendung genügt nicht.

Eine kompakte Tabelle reicht aus:

| Quelle / Werkzeug | Verwendung | Betroffene Bestandteile | Eigene Anpassung und Prüfung |
|---|---|---|---|

Vollständige KI-Chatverläufe werden nicht verlangt. Die zu verwendende Eigenständigkeitserklärung und gegebenenfalls ergänzende Hochschulvorgaben sollten bekannt sein.

## 6. Termine, Bestehen und Gesamtnote

- **Abgabefrist für beide Prüfungsteile:** 15.01.2027, 23:59 Uhr.
- **Frühere Abgabe:** Beide Prüfungsteile dürfen bereits vor diesem Termin abgegeben werden.
- **Abgabeweg:** Upload in ILIAS als ein ZIP-Archiv je Prüfungsteil beziehungsweise Gruppenabgabe. Alle zugehörigen Abgabebestandteile sind im Archiv enthalten; beim Gruppenprojekt gehört dazu auch das Git-Repository einschließlich `.git`-Verzeichnis und Versionshistorie.
- **Gruppenabgabe:** Eine Person lädt stellvertretend für das gesamte Team alle Bestandteile hoch, einschließlich der individuellen Beitragsbeschreibungen und Erklärungen. Alle Teammitglieder sind in der Abgabe eindeutig zu benennen. Separate Uploads der übrigen Teammitglieder sind nicht erforderlich.
- **Dateigröße:** Halten Sie die in ILIAS angegebene Uploadgrenze ein. Lassen Sie generierte Unity-Daten weg und komprimieren Sie insbesondere das Video so, dass die relevanten Inhalte weiterhin erkennbar bleiben.
- **Ausnahme für große statische Dateien:** Nur wenn die Uploadgrenze trotz dieser Maßnahmen nicht eingehalten werden kann, dürfen große statische Binärdateien, etwa Video-, Audio- oder Texturdateien, extern bereitgestellt werden. Code-Bestandteile und Unity-Szenen dürfen nicht ausgelagert werden und müssen im ILIAS-Archiv enthalten sein. Dokumentieren Sie in der README die ausgelagerten Dateien, direkte Downloadlinks und gegebenenfalls deren Zielpfade im Projekt. Erforderliche `.meta`-Dateien bleiben im Projekt erhalten. Die externen Dateien müssen im abgegebenen Stand für die Bewertung zugänglich bleiben; die Ausnahme ersetzt nicht die ZIP-Abgabe in ILIAS.
- **Bestehen beider Teile:** Pro Person müssen mindestens **15 von 30 Punkten in Teil 1** und **35 von 70 Punkten in Teil 2** erreicht werden, also jeweils die Hälfte der möglichen Punkte. Zusätzlich gelten die Mindestanforderungen aus Abschnitt 3.6. Ein nicht bestandener Teil kann nicht durch Punkte des anderen Teils ausgeglichen werden.
- **Wiederholung:** Wird die Kombinationsprüfung nicht bestanden, müssen beide Prüfungsteile vollständig wiederholt werden. Ein bereits bestandener Prüfungsteil wird nicht übernommen.
- **Punkte und Gesamtnote:** Die erreichten Punkte aus Teil 1 und Teil 2 werden pro Person addiert. Maximal sind 100 Punkte möglich; die Gewichtung von 30:70 ist bereits enthalten. Es werden keine Teilnoten gebildet oder gemittelt. Die Summe wird vor der Notenzuordnung nicht aufgerundet.
- **Individuelle Gruppenbewertung:** Teil 2 umfasst 50 gemeinsame und 20 individuelle Punkte gemäß Abschnitt 4.5.
- **Verspätete Abgabe, Krankheit und Fristverlängerung:** Es gelten die Hochschul- und Prüfungsregelungen.

### Einheitliche Notenskala für die Gesamtprüfung

Die folgende Skala wird nur angewendet, wenn **beide Prüfungsteile bestanden** sind. Wird eine der Bestehensbedingungen nicht erfüllt, lautet die Gesamtnote unabhängig von der Punktesumme **5,0 (nicht bestanden)**.

| Gesamtpunkte von 100 | Note |
|---|---:|
| 95 bis 100 | 1,0 |
| 90 bis unter 95 | 1,3 |
| 85 bis unter 90 | 1,7 |
| 80 bis unter 85 | 2,0 |
| 75 bis unter 80 | 2,3 |
| 70 bis unter 75 | 2,7 |
| 65 bis unter 70 | 3,0 |
| 60 bis unter 65 | 3,3 |
| 55 bis unter 60 | 3,7 |
| 50 bis unter 55 | 4,0 |
| Unter 50 | 5,0 |

**Beispiele:** 15 Punkte in Teil 1 und 35 Punkte in Teil 2 ergeben bei erfüllten Mindestanforderungen 50 Gesamtpunkte und damit die Note 4,0. Mit 14 Punkten in Teil 1 und 60 Punkten in Teil 2 ist die Prüfung trotz 74 Gesamtpunkten nicht bestanden.
