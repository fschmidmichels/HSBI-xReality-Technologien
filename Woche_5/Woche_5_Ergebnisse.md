# Woche 5: Ergebnisse – Immersion durch Feedback, Audio und räumliches UI

## Block 1 – Vorgestellte KI-Szenen aus Woche 4

| Szene | Werkzeuge | Gelang schnell | Musste nachgearbeitet werden | KI lag falsch bei … |
|---|---|---|---|---|
| Dosenwerfen | Gemini Argon | ca 1/2 Stunde | nein | evtl. "Wurfgefühl" noch nicht optimal |

Weitere Szenen wurden leider nicht vorgestellt.

**Erwartete Muster über alle Szenen:**

- **Schnell mit KI:** Umgebung aus einfachen Körpern, Materialien, einfache Spiellogik (Zähler, Trigger, Reset), Erklärungen von Fehlermeldungen.
- **Nacharbeit nötig:** Greifen und Werfen (falsche oder veraltete Komponenten statt Interaction SDK), Maßstab und Platzierung im Raum, UI im *Screen Space* statt *World Space*.
- **Typische KI-Fehler:** `velocity` statt `linearVelocity`, ältere OVR-Komponenten, erfundene Menüpfade, Lösungen, die im Editor funktionieren, auf der Quest aber nicht.
- **Beobachtung zu Feedback:** Die meisten KI-Szenen haben kaum Rückmeldungen. Greifen und Treffer sind oft nur daran zu erkennen, dass sich etwas bewegt; Geräusche und Haptik fehlen fast immer. Das ist die Überleitung zu Block 2.

## Block 2 – Feedback-Inventur

### Erwartete Tabelle für die Bowlingszene

| Moment | Was muss die Person wissen? | Aktuell vorhanden? | Kanal | Umsetzung in Block 3/4 |
|---|---|---|---|---|
| **Greifen** | Kann ich die Kugel greifen? Habe ich sie in der Hand? | Hand bzw. Controller schließt sich um die Kugel; keine weitere Rückmeldung | haptisch, visuell | kurzer Vibrationsimpuls am greifenden Controller; ggf. Hervorhebung der Kugel beim Annähern |
| **Loslassen** | Ist sie losgelassen, war der Wurf gültig? | Kugel fliegt bzw. rollt | auditiv | Aufsetz- und Rollgeräusch; Haptik beim Loslassen meist unnötig |
| **Kollision** | Was wurde getroffen und wie stark? | Pin-Geräusche aus dem Paket von Woche 3; Pins fallen sichtbar | auditiv, visuell | Treffer-Sound der Kugel, Lautstärke nach Aufprallgeschwindigkeit; Pin-Geräusche räumlich |
| **Auswertung** | Wie viele Pins sind gefallen? Ist die Zählung abgeschlossen? | Score-Anzeige, aktualisiert sich teils schon während des Wurfs | visuell, auditiv | Anzeige erst nach der Auswertung aktualisieren; kurzer Bestätigungston; optional Jubel bzw. „Aww“ |
| **Neustart** | Wann darf ich wieder werfen? Muss ich etwas tun? | Kugel kehrt zurück; Neustart nur per Taste, die nicht erklärt ist | visuell | Kugel liegt wieder auf der Ablage; Neustart-Schaltfläche im Raum |

**Erwartete Lücken:** Greifen (keine Bestätigung), Ende der Auswertung (unklar, ob die Zählung fertig ist) und Neustart (Taste ist unsichtbar).

### Erwartete Antworten zur Kanalwahl

- **Haptik** nur für Greifen (und ggf. Loslassen). Vibration beim Umfallen der Pins in 18 m Entfernung wirkt unplausibel.
- **Audio** ist der wichtigste Kanal für Ereignisse auf der Bahn, weil die Person beim Wurf meist der Kugel nachschaut und die Anzeige nicht im Blick hat.
- **Visuell** für den Punktestand und den Zustand „bereit“.
- **Leitfrage Hand-Tracking:** Ohne Controller entfällt die Haptik. Ersatz: sichtbare Rückmeldung an der Hand oder Kugel (Hervorhebung, Hand-Pose) und ein kurzes Greifgeräusch.

### Erwartete Skizzen für den Einstieg

- Hinweistext über der Ablage: „Greife die Kugel und rolle sie auf die Pins.“
- Hervorgehobene Kugel (Glow oder leichtes Pulsieren), solange noch nicht geworfen wurde.
- Markierung der Wurfposition auf dem Anlauf (z. B. Fußabdrücke).
- Sichtbare Neustart-Schaltfläche neben der Ablage statt einer Taste.

## Block 3 – Räumliches Audio

### Pin-Geräusche untersuchen (`PinAudio.cs`)

| Frage | Erwartete Antwort |
|---|---|
| Welches Ereignis löst das Geräusch aus? | `OnCollisionEnter` am Pin |
| Wie wird die Lautstärke berechnet? | `relativeVelocity.magnitude / fullVolumeSpeed`, auf 0–1 begrenzt |
| Wozu `minImpactSpeed` und `cooldown`? | Keine Geräusche beim Aufstellen bzw. bei Kleinstkollisionen; kein „Knattern“, wenn ein Pin mehrfach kurz hintereinander anstößt. Bei 0/0 ist beim Start und beim Liegenbleiben der Pins ein unruhiges Klicken zu hören. |
| Warum zufällige Tonhöhe? | Gleiche Clips klingen bei Wiederholung künstlich („Maschinengewehr-Effekt“); kleine Variation wirkt natürlicher. |

**Vergleich Spatial Blend 0 gegenüber 1:**

- Bei 0 klingen die Pins, als stünden sie direkt neben einem; die Entfernung ist nicht hörbar.
- Bei 1 kommt das Geräusch hörbar von vorn und aus der Ferne; beim Drehen des Kopfes wandert es mit.
- Erwartetes Problem: Mit *Logarithmic Rolloff* und kleiner *Min Distance* sind die Pins in 18 m Entfernung sehr leise. Lösung: *Min Distance* erhöhen (im Muster-Projekt 3 m) oder *Max Distance* anpassen.

### Geräusche der Kugel (`BallAudio.cs` aus dem Paket von Woche 3)

Das Skript wird unverändert verwendet, eingerichtet und untersucht. Typische Fehler beim Einrichten:

- `NullReferenceException` in jedem Frame: `Roll Source` nicht zugewiesen. Bei Treffern: `Hit Source` fehlt. Fehlt nur der `Strike Clip`, bleibt der Treffer stumm (Warnung statt Ausnahme).
- Rollgeräusch schon beim Start kurz hörbar: *Volume* von `RollAudio` nicht auf 0 gesetzt.
- Kein Rollgeräusch: *Play On Awake* oder *Loop* aus, oder `BallAudio` hängt an einem Objekt ohne die Kollisionen des Rigidbody.

**Aufprall (`OnCollisionEnter`):**

| Frage | Erwartete Antwort |
|---|---|
| Pin oder Bahn? | Objekte ohne Rigidbody (`collision.rigidbody == null`) gelten als Boden; ein Treffer zählt nur, wenn das andere Objekt die Komponente `Pin` trägt. |
| Lautstärke, `+ 0.3f`? | `relativeVelocity.magnitude / fullVolumeSpeed + 0.3`, auf 0–1 begrenzt. Der Zusatz sorgt für eine Mindestlautstärke: Auch ein langsamer Treffer ist in 18 m Entfernung noch hörbar. Volle Lautstärke wird dadurch schon bei 5,6 m/s erreicht. |
| Ohne `strikeCooldown`? | Mehrere Strike-Sounds direkt hintereinander, weil die Kugel in kurzer Folge mehrere Pins berührt. Der Clip ist ein kompletter Strike und soll pro Wurf nur einmal kommen; die einzelnen Anschläge übernimmt `PinAudio`. Deshalb 2 s statt 0,05 s. |
| Fehlende Absicherung? | Es gibt keine Mindestgeschwindigkeit. Auch wenn die Kugel einen Pin nur ganz langsam berührt (z. B. ausrollend), erklingt der Strike-Sound mit mindestens 30 % Lautstärke. |
| Prüfung in `PinAudio`? | Ohne sie spielt der getroffene Pin zusätzlich seinen eigenen Anschlag; der Treffer der Kugel klingt doppelt. |

**Rollen (`Update`, `OnCollisionStay`):**

| Frage | Erwartete Antwort |
|---|---|
| Warum Loop mit *Volume* 0? | Rollen ist ein Zustand. Ein dauerhaft laufender Loop lässt sich weich ein- und ausblenden; Starten und Stoppen würde knacken und bei jedem Hüpfer von vorn beginnen. |
| Bodenkontakt, Zeitfenster? | `OnCollisionStay` und `OnCollisionEnter` merken sich den Zeitpunkt des letzten Kontakts mit einem Objekt ohne Rigidbody. Die Physik läuft in festen Schritten und nicht in jedem Frame; außerdem hebt die Kugel bei kleinen Unebenheiten kurz ab. Das Zeitfenster von 0,1 s überbrückt beides. Ein „Verlassen“-Ereignis muss nicht behandelt werden. |
| `!body.isKinematic`? | Eine gegriffene Kugel ist kinematisch. Ohne die Prüfung könnte es rollen, wenn die gehaltene Kugel über die Ablage oder den Boden geführt wird. |
| Nur horizontale Geschwindigkeit? | Rollen ist Bewegung entlang der Bahn. Der vertikale Anteil (Fallen, Hüpfen) soll kein Rollgeräusch erzeugen. |
| Parameter? | Unter `minRollSpeed` ist nichts zu hören; bei `fullVolumeSpeed` ist `maxRollVolume` erreicht; die Tonhöhe steigt mit der Geschwindigkeit von `minPitch` auf `maxPitch`. |
| `Mathf.MoveTowards`? | Die Lautstärke wird weich nachgeführt (hier in höchstens 0,25 s von 0 auf 1), damit es bei kurzen Hüpfern und beim Aufsetzen nicht knackt. |

**Ausprobieren:**

- `strikeCooldown` = 0: mehrere überlagerte Strike-Sounds, klingt unnatürlich laut und verschmiert.
- Gleiche Werte für `minPitch` und `maxPitch`: Die Geschwindigkeit ist nur noch an der Lautstärke zu erkennen; das Rollen wirkt gleichförmiger.
- `fullVolumeSpeed` = 2: Rollen und Aufprall fast immer mit voller Lautstärke, kaum Unterschied zwischen Würfen. Bei 20: Rollen sehr leise; der Aufprall bleibt wegen `+ 0.3` hörbar.
- *Spatial Blend* 0 an `RollAudio`: Das Rollen klingt, als bliebe die Kugel neben einem; die Entfernung ist nur noch über die Geschwindigkeit zu erahnen.

### Optional – `BallAudio` erweitern

Erwartet von wenigen schnellen Personen:

- Mindestgeschwindigkeit: zusätzliches Feld und Abbruch in `OnCollisionEnter`, analog zu `PinAudio`.
- Zuweisungen: Prüfung in `Awake` mit `Debug.LogWarning` und `enabled = false`, oder Null-Prüfungen vor der Verwendung.
- Rinne: meist nur als Idee (Tag oder Physics Material des Kollisionspartners abfragen); eine Umsetzung ist nicht zu erwarten.

## Block 4 – Haptik und räumliches UI

### Haptischer Impuls beim Greifen

| Stärke / Dauer | Erwarteter Eindruck |
|---|---|
| 0,3 / 0,05 s | kaum spürbar, wird leicht übersehen |
| 0,6 / 0,08–0,1 s | sehr kurzes „Klick“-Gefühl; beim Greifen schwer zu bemerken (Test am Musterprojekt) |
| 0,6 / 0,2 s | deutlich spürbar, ohne zu stören; Standardwert im Musterprojekt |
| 1,0 / 0,3 s | zu stark und zu lang, wirkt wie ein Fehlersignal |

- Erwartete Empfehlung: mittlere Stärke, kurze Dauer (um 0,2 s). Kürzere Impulse sind beim Zugreifen schwer zu bemerken.
- Leitfrage „Vibriert nur der greifende Controller?“: Ja, wenn der Controller über `ControllerRef` korrekt ermittelt wird. Typischer Fehler: beide Controller vibrieren (`OVRInput.Controller.Touch` bzw. `Active` statt `LTouch`/`RTouch`).
- Mit Hand-Tracking: kein Impuls, kein Fehler.

### Punktestand

Erwartete Befunde:

- Anzeige über den Pins ist aus der Wurfposition lesbar, wenn die Schrift groß genug ist (in ca. 19 m Entfernung mindestens etwa 30 cm Schrifthöhe).
- Score aktualisiert sich teils schon, während Pins noch fallen; besser erst nach der Auswertung.
- Anzeige neben der Bahn verdeckt bei manchen die Sicht auf die Pins.

### Neustart

- Erwartete Platzierung: Schaltfläche neben bzw. vor der Kugelablage in Greifhöhe (ca. 0,9–1,1 m), leicht zur Person geneigt.
- Erwartete Schwierigkeiten: fehlendes `PointableCanvasModule` (Assistent meldet es, **Fix** behebt es), Schaltfläche zu klein oder zu weit weg, versehentliches Auslösen beim Greifen der Kugel.
- Leitfrage Variante: Zum Einstieg ohne Erklärung passt die **sichtbare Schaltfläche** am besten; die Controller-Taste kann zusätzlich bleiben.

## BS – A/B-Vergleich und Überarbeitung

### Erwartete Ergebnisse des A/B-Vergleichs

| Variante | Verständlichkeit | Plausibilität | Subjektives Erleben | Erwarteter Kommentar |
|---|---|---|---|---|
| A: mit räumlichem Audio | 4–5 | 4–5 | 4–5 | „Man hört, wo die Pins fallen.“ |
| B: ohne räumliches Audio (Spatial Blend 0) | 4 | 2–3 | 3 | „Klingt, als wäre alles direkt neben mir.“ |
| A: mit Haptik beim Greifen | 4–5 | 4–5 | 4 | „Ich merke sofort, dass ich die Kugel habe.“ |
| B: ohne Haptik | 3–4 | 3–4 | 3 | Unterschied fällt manchen erst im direkten Vergleich auf. |

**Erwartete Schlussfolgerung zur Leitfrage „Mehr Feedback = mehr Immersion?“:**

- Gezieltes Feedback an den richtigen Momenten verbessert Verständlichkeit und Erleben deutlich.
- Zu viel Feedback stört: Überlagerte Geräusche, zu lange Vibration, zu viel Text oder Jubel nach jedem Wurf wirken aufdringlich.
- Unterschiede zwischen A und B werden oft erst bemerkt, wenn die Rückmeldung **fehlt**.

### Erwartete Überarbeitungen

- Schaltfläche größer oder näher an die Ablage gesetzt.
- Hinweistext gekürzt oder nach dem ersten Wurf ausgeblendet.
- Lautstärke der Pins bzw. *Min Distance* angepasst.
- Vibration kürzer gestellt.

### Optional – Meta XR Audio Spatializer

Erwartet von wenigen schnellen Personen:

- **Hörbarer Unterschied:** Mit eingeschaltetem Spatializer (**Enable Spatialization**) ist besser zu hören, ob Geräusche vorn oder hinten liegen. Mit dem Rücken zur Bahn ist der Unterschied am deutlichsten; beim normalen Wurf mit Blick auf die Pins fällt er kaum auf.
- **Typische Stolpersteine:** Stereo-Clips nicht auf Mono umgestellt (dann wirkt der Spatializer nicht wie erwartet); Meta XR Audio Source nur im Prefab ergänzt, aber die getestete Szenen-Instanz ist kein Prefab mehr; zum Vergleich das Häkchen **Spatialize** an der AudioSource umgeschaltet (wirkungslos, die Meta-Komponente überschreibt es zur Laufzeit); Pins nach dem Umstellen zu leise (Abhilfe: *Gain Boost* an der Meta XR Audio Source).
- **Erwartete Einschätzung:** Für das Bowling ein geringer Gewinn, weil die Geräusche fast immer von vorn kommen. Wichtiger ist er bei Projekten, in denen Geräusche von allen Seiten kommen, z. B. Orientierung im Raum, Gegner oder Hinweise hinter der Person.

## Nachbereitung (optional)

### Erwartete Ergebnisse des Onboarding-Tests

Der Test ist optional und findet außerhalb der Präsenz statt; Ergebnisse sind nur von einem Teil der Gruppe zu erwarten.

| Beobachtung | Erwartung ohne Hinweis | Erwartung mit Hinweis |
|---|---|---|
| Zeit bis zum ersten Wurf | ca. 20–60 s, teils Nachfragen | unter 20 s |
| Zögern bzw. Nachfragen | Wo ist die Kugel? Wie greife ich? | Wie wird neu gestartet? |
| Neustart ohne Hilfe gefunden | selten (Taste unbekannt) | meist, wenn die Schaltfläche sichtbar ist |

Zeigt der Test, dass der Einstieg nicht ohne Erklärung funktioniert, passen die Teilnehmenden meist den Hinweistext oder die Platzierung der Schaltfläche an (vgl. „Erwartete Überarbeitungen“).

## Stand im Muster-Projekt

Das [Muster-Projekt](https://github.com/fschmidmichels/HSBI-xReality-Technologien-Unity) enthält zu dieser Woche:

| Bestandteil | Umsetzung |
|---|---|
| Räumliches Audio | `PinAudio` an allen Pins; `BallAudio` (dieselbe Fassung wie im Paket von Woche 3, mit Treffer- und Rollgeräusch); `EvaluationAudio` mit Bestätigung, Jubel, Klatschen und „Aww“ |
| Haptik | `GrabHaptics` an der Kugel: kurzer Impuls am greifenden Controller |
| Punktestand | `ScoreBoard` über den Pins, `ScoreHistoryBoard` mit den bisherigen Frames |
| Neustart | `RestartButton` vor der Kugelablage (Poke) sowie X-Taste |
| Einstieg | `OnboardingHint` über der Ablage, blendet beim ersten Wurf aus |
