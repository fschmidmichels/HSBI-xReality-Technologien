# Woche 6: Animationen und Performance

<!-- ENTWURF – aus der Planung abgeleitet; weitere Blöcke folgen noch. Code und Menüpfade vor Kursbeginn im Musterprojekt prüfen. -->

## Überblick

In der sechsten Woche wird die Bowlingszene in zwei Richtungen weiterentwickelt. Zuerst werden die bisher schlagartigen Zustandswechsel durch Animationen ersetzt: Die Pins werden sichtbar neu aufgestellt, und die Kugel kommt nachvollziehbar zur Ablage zurück. Dabei geht es vor allem um das Zusammenspiel von Animation und Physik. Danach wird gemessen, wie gut die Szene auf der Quest 3 läuft, und eine gezielte Verbesserung vorgenommen. In der BS-Phase wird das Bowling in eine Mixed-Reality-Variante im realen Raum übertragen.

Als gemeinsames Ergebnis entsteht ein Bowling-Prototyp mit animiertem Zurücksetzen, gemessener und verbesserter Performance sowie eine erste MR-Variante.

**Umfang:** 4 h Praktikum + 2 h Betreutes Selbststudium.

Grundlage ist das eigene Bowlingprojekt mit Spielablauf, Audio, Haptik und UI aus Woche 5. Vor Beginn wird der aktuelle Stand gesichert, z.B. **mit Git committet**. Andernfalls wird ein Klon des Musterprojekts benutzt (siehe README).

## Lernziele

Im Verlauf der Veranstaltung werden folgende Kompetenzen aufgebaut:

- Gestaltung nachvollziehbarer Zustandswechsel durch Animationen statt schlagartiger Sprünge;
- Abstimmung von Animation und Physiksimulation über kinematische Rigidbodies;
- gezielter Einsatz und kritische Prüfung von KI-Werkzeugen bei der Umsetzung;
- Messung von Framezeiten auf dem Zielgerät und Einordnung in das Framebudget;
- Identifikation einfacher Engpässe und Überprüfung einer Optimierung durch erneute Messung;
- Übertragung einer VR-Szene in Mixed Reality mit Passthrough und Bezug zum realen Raum.

## Block 1 – Animationen

### Warum animieren?

Bisher setzen `PinGroup.ResetPositions()` und `LaneControls.ReturnBall()` Pins und Kugel in einem einzigen Frame an ihre Ausgangsposition. Technisch ist das korrekt, für die Person im Headset aber schwer nachzuvollziehen: Die Kugel ist plötzlich wieder da, die Pins stehen ohne Übergang. Eine kurze Animation zeigt, **dass** und **wie** sich der Zustand ändert, und gibt der Person Zeit, den Wechsel wahrzunehmen. Sie ist damit auch eine Form von Feedback (vgl. Woche 5).

Leitfragen:

- Wie lang darf eine Animation sein, bevor sie als Wartezeit empfunden wird?
- Was soll passieren, wenn die Person während der Animation nach der Kugel greift?

### KI-Unterstützung ausdrücklich erwünscht

In diesem Block **soll** mit KI-Werkzeugen gearbeitet werden. Verwendet werden die in [Woche 4](../Woche_4/Woche_4.md#block-2--unity-mit-ki-verwenden) eingerichteten Werkzeuge: Coding-Assistent in VS Code (GitHub Copilot oder KI:connect.nrw mit Continue), Unity-MCP-Server und Meta-MCP-Server. Das Code-Gerüst und die Hinweise in diesem Block dienen als Orientierung und zum Prüfen der KI-Ergebnisse, nicht als Abschreibvorlage.

Vorgehen wie in Woche 4:

1. **Kontext geben:** Die betroffenen Skripte (`PinGroup.cs`, `LaneControls.cs`, bei Bedarf `BallReturn.cs` und `GameManager.cs`) anhängen und das Ziel genau beschreiben.
2. **Anforderungen nennen:** Rigidbody während der Animation kinematisch, Bewegung über `MovePosition`/`MoveRotation`, Geschwindigkeiten vor dem Freigeben auf null, vorhandene Aufrufer unverändert lassen.
3. **Ergebnis lesen und erklären:** Den erzeugten Code der Nachbarperson erklären. Was nicht erklärt werden kann, wird nicht übernommen.
4. **Im Headset testen:** insbesondere die Fallstricke unter [Kugel zurückholen](#kugel-zurückholen), z. B. Greifen während der Animation.
5. **Fehler zurückmelden** und Änderungen per Git-Diff prüfen.

Beispiel-Prompt:

> Unity 6000.3, Meta XR Interaction SDK. Angehängt sind `PinGroup.cs` und `LaneControls.cs`. Ändere `ResetPositions()` so, dass sich die Pins innerhalb von 1 s aus 1 m Höhe auf ihre gespeicherten Positionen absenken. Die Rigidbodies sollen während der Animation kinematisch sein und mit `MovePosition` bewegt werden. Danach werden sie wieder freigegeben. Erkläre kurz, welche Stellen in `BallReturn` und `GameManager` von der Änderung betroffen sein könnten.

Zum Abschluss wird kurz festgehalten: Was hat die KI richtig gemacht, wo lag sie falsch (z. B. Transform statt Rigidbody bewegt, Physik nicht abgeschaltet), und wie wurde das bemerkt? Der KI-Einsatz wird wie gewohnt im README vermerkt.

### Animation und Physik

Pins und Kugel sind **dynamische** Rigidbodies: Ihre Position wird von der Physik berechnet. Bewegt man sie zusätzlich per Animation, arbeiten zwei Systeme gegeneinander. Die Folgen sind Zittern, Herausschießen beim Loslassen oder Kollisionen mit anderen Pins während der Bewegung.

Lösung: Während der Animation wird der Rigidbody **kinematisch** geschaltet. Er wird dann nicht mehr von Kräften und Schwerkraft bewegt, schiebt aber weiterhin andere Objekte weg. Nach der Animation wird er wieder freigegeben.

| Phase | `isKinematic` | Bewegung durch |
|---|---|---|
| Spiel | `false` | Physik (Kräfte, Kollisionen) |
| Animation | `true` | Skript bzw. Animator |
| nach der Animation | `false` | Physik; Geschwindigkeiten vorher auf null |

Kinematische Rigidbodies werden in `FixedUpdate` bzw. nach `WaitForFixedUpdate` mit `Rigidbody.MovePosition` und `Rigidbody.MoveRotation` bewegt. So bleibt die Interpolation der Physik erhalten, und die Bewegung wirkt im Headset flüssig.

### Pins neu aufstellen

Die Pins werden nicht mehr an ihre Position gesetzt, sondern senken sich von oben auf die Bahn ab, ähnlich wie bei einer Pinsetter-Maschine.

Ansatz für eine Coroutine in `PinGroup`:

```csharp
// In PinGroup ergänzen. Ersetzt das sofortige Setzen in ResetPositions().
[Tooltip("Höhe in Metern, aus der sich die Pins absenken.")]
public float dropHeight = 1.0f;

[Tooltip("Dauer der Absenkung in Sekunden.")]
public float resetDuration = 1.0f;

public bool IsResetting { get; private set; }

private IEnumerator AnimateReset()
{
    IsResetting = true;
    Rigidbody[] bodies = new Rigidbody[pins.Length];

    for (int index = 0; index < pins.Length; index++)
    {
        pins[index].gameObject.SetActive(true);
        pins[index].CancelToppleCheck();
        bodies[index] = pins[index].GetComponent<Rigidbody>();
        bodies[index].linearVelocity = Vector3.zero;
        bodies[index].angularVelocity = Vector3.zero;
        bodies[index].isKinematic = true;
        Vector3 start = positions[index] + Vector3.up * dropHeight;
        bodies[index].position = start;
        bodies[index].rotation = rotations[index];
        pins[index].transform.SetPositionAndRotation(start, rotations[index]);
    }

    float time = 0f;
    while (time < resetDuration)
    {
        yield return new WaitForFixedUpdate();
        time += Time.fixedDeltaTime;
        // SmoothStep: langsam anfahren, langsam abbremsen.
        float t = Mathf.SmoothStep(0f, 1f, time / resetDuration);
        for (int index = 0; index < pins.Length; index++)
        {
            Vector3 start = positions[index] + Vector3.up * dropHeight;
            bodies[index].MovePosition(Vector3.Lerp(start, positions[index], t));
        }
    }

    foreach (Rigidbody body in bodies)
    {
        body.isKinematic = false;
    }
    IsResetting = false;
}
```

Aufgaben:

1. `ResetPositions()` so umbauen, dass es die Coroutine startet (`StartCoroutine(AnimateReset())`). Läuft bereits eine Animation, wird sie vorher gestoppt.
2. Im Headset prüfen: Stehen die Pins nach der Animation still, oder kippen oder springen sie beim Freigeben?
3. Prüfen, ob die Auswertung in `BallReturn` (`ArePinsSettled`) während der Animation fälschlich auslöst. Bei Bedarf `IsResetting` dort berücksichtigen.

Varianten zum Ausprobieren: alle Pins gleichzeitig oder kurz nacheinander (Pin 1 zuerst); stehen gebliebene Pins vor dem Neuaufstellen anheben und wieder absenken; ein einfacher „Rechen“ (Quader), der umgefallene Pins in den Pit schiebt.

### Kugel zurückholen

Statt auf der Ablage zu erscheinen, rollt die Kugel aus einem Rückführschacht auf die Ablage. Der Ansatz ist derselbe wie bei den Pins:

1. Kugel kinematisch schalten, Geschwindigkeiten auf null setzen.
2. Kugel an einen Startpunkt setzen, der für die Person nicht direkt sichtbar ist, z. B. ein leeres GameObject `BallReturnExit` hinter oder unter der Ablage.
3. Kugel per Coroutine über `MovePosition` vom Startpunkt zur Ablage bewegen, optional mit einer passenden Drehung (`MoveRotation`), damit sie sichtbar rollt.
4. Am Ziel `isKinematic = false` setzen.

Der Umbau erfolgt in `LaneControls.ReturnBall()`. `BallReturn` und `GameManager` rufen diese Methode bereits auf und müssen nicht verändert werden.

Zu beachten:

- **Greifen während der Animation:** Das Interaction SDK verändert beim Greifen und Loslassen selbst den kinematischen Zustand. Wird die Kugel mitten in der Animation gegriffen, kann sie nach dem Loslassen kinematisch bleiben und in der Luft hängen. Entweder wird das Greifen während der Animation verhindert, oder die Animation wird beim Greifen abgebrochen. Welche Lösung passt besser zum Spielablauf?
- **`IsBallAtStart`:** Der `GameManager` erkennt einen neuen Wurf daran, dass die Kugel die Ablage verlässt. Während die Kugel zur Ablage fährt, darf das keinen Wurf auslösen.
- **Audio:** Das Rollgeräusch aus `BallAudio` hört auf Bodenkontakt und `!body.isKinematic`. Ist während der Animation etwas zu hören, und sollte es das?

### Alternative: Animator und Animation Clips

Statt per Skript lassen sich Bewegungen auch als **Animation Clip** im Animation-Fenster (**Window → Animation → Animation**) aufzeichnen und über einen **Animator** abspielen. Das eignet sich für feste Abläufe, etwa einen Pinsetter-Balken, der herunterfährt. Für Rigidbodies gilt dasselbe: Während der Clip läuft, ist der Rigidbody kinematisch, und am Animator wird **Update Mode** = *Animate Physics* gesetzt. Für Bewegungen mit wechselnden Start- und Zielpunkten, wie bei der Kugel, ist eine Coroutine meist einfacher.

| Ansatz | Vorteil | Nachteil |
|---|---|---|
| **Coroutine** (`MovePosition`, `Lerp`) | Start und Ziel zur Laufzeit; Ablauf im Code nachvollziehbar | Kurvenverlauf nur über Formeln |
| **Animator** (Animation Clip) | Verlauf im Editor sichtbar und bearbeitbar | Feste Positionen; Zusammenspiel mit Physik und Skripten aufwendiger |

## Block 2 – Performance auf der Quest 3

### Framebudget

Die Quest 3 zeigt in der Standardeinstellung 72 Bilder pro Sekunde an; Apps können 90 oder 120 Hz anfordern. Für jedes Bild steht damit nur eine feste Zeit zur Verfügung:

| Bildwiederholrate | Framebudget |
|---|---|
| 72 Hz | 13,9 ms |
| 90 Hz | 11,1 ms |
| 120 Hz | 8,3 ms |

Wird das Budget überschritten, fällt ein Bild aus. Das System füllt die Lücke durch Reprojektion, was als Ruckeln oder Doppelbilder bei Kopfbewegungen wahrgenommen wird und Unwohlsein fördern kann. Performance ist in XR deshalb auch eine Frage des Komforts, nicht nur der Technik.

#### Bildwiederholrate einstellen

Wie eine App eine andere Rate anfordert, beschreibt die Meta-Dokumentation [Set Display Refresh Rates](https://developers.meta.com/vr/documentation/unity/unity-set-disp-freq/). Das Vorgehen:

1. Die Liste der Raten abfragen, die das Gerät anbietet (`OVRPlugin.systemDisplayFrequenciesAvailable`). Die Laufzeitumgebung lehnt Raten ab, die nicht in dieser Liste stehen.
2. Eine Rate aus der Liste setzen (`OVRPlugin.systemDisplayFrequency = 90f;`).
3. Unter `com.oculus.supportedDevices` im Android-Manifest muss die Quest 3 eingetragen sein (`quest3`).

Im Musterprojekt übernimmt das das Skript `DisplayRefreshRate` am GameManager. Es fragt die Liste ab und stellt die gewünschte Rate ein (Standard 90 Hz), oder die nächst niedrigere, falls die gewünschte fehlt. Im Editor ohne Headset ist die Liste leer, dann bleibt die Rate unverändert.

Zu beachten:

- **Budget:** Eine höhere Rate lohnt sich nur, wenn die App das kürzere Budget dauerhaft einhält. Sonst ruckelt es stärker als bei 72 Hz.
- **Überhitzung:** Wird die Quest zu warm, setzt das System die Rate selbst auf 72 Hz zurück. Das Ereignis `OVRManager.DisplayRefreshRateChanged` meldet solche Wechsel, das Skript gibt sie in der Console aus.
- **Vergleichen:** Eine gute Übung ist, dieselbe Situation bei 72 Hz und bei 90 Hz zu messen und zu prüfen, ob das Budget von 11,1 ms noch reicht.

CPU und GPU arbeiten parallel. Entscheidend ist, welche der beiden länger braucht:

- **CPU-gebunden:** Skripte, Physik, viele Objekte bzw. Draw Calls.
- **GPU-gebunden:** Auflösung, Schatten, transparente Flächen, aufwendige Shader und Post-Processing.

### Messen

Gemessen wird **auf der Quest**, nicht im Editor. Der Editor läuft auf einem viel stärkeren Rechner und sagt über das Zielgerät wenig aus.

1. **OVR Metrics Tool:** Overlay im Headset mit Bildrate, CPU- und GPU-Auslastung und GPU-Framezeit. Am schnellsten wird es direkt auf der Quest im Meta Horizon Store gesucht und von dort installiert. Anschließend wird es in der App selbst als Overlay eingeschaltet. Geeignet für einen schnellen Überblick während des Spielens.
2. **Unity Profiler:** In den **Build Profiles** werden *Development Build* und *Autoconnect Profiler* aktiviert. Nach dem Start der APK verbindet sich der Profiler (**Window → Analysis → Profiler**) mit der Quest. Im CPU-Modul ist zu sehen, welche Skripte und Systeme wie viel Zeit brauchen.
3. **Performance Analyzer im Meta VR Developer Hub (MVDH):** Die Quest ist per USB (ADB) verbunden. Auf dem Tab **Performance Analyzer** startet die Play-Schaltfläche oben die Aufzeichnung. Die Messwerte laufen dann als Zeitleiste am Rechner mit, z. B. Bildrate, Frame-Timings, CPU, GPU und Speicher. Die Module lassen sich über **Modules** ein- und ausblenden. Über das Zahnrad (**Performance Settings**) werden Schwellwerte gesetzt, z. B. Bildrate unter 72 fps. Wird ein Schwellwert über- oder unterschritten, erscheint eine Markierung (**Flag**), die zur passenden Stelle in der Zeitleiste springt. Für genauere Werte sollte das Casting mit dem Schalter **Casting** abgeschaltet werden. Geeignet, um einen ganzen Ablauf aufzuzeichnen und Einbrüche im Nachhinein zu finden, ohne dass ein Overlay im Bild stört.

> **Hinweis: Der Unity Profiler misst auf der Quest nur die CPU.** Das GPU-Modul (*GPU Usage*) unterstützt laut Unity-Dokumentation kein Vulkan unter Android. Die Quest verwendet Vulkan. Mit OpenGL ES funktioniert es unter Android nur auf NVIDIA- und Intel-GPUs, die Quest hat aber eine Qualcomm-Adreno-GPU. Außerdem ist GPU-Profiling ausgeschlossen, solange in den Player Settings *Graphics Jobs* aktiv ist, wie im Musterprojekt. Der Unity Profiler zeigt deshalb, **welche CPU-Arbeit** teuer ist (Skripte, Physik, Vorbereitung des Renderings). **Ob die GPU** der Engpass ist, zeigen das OVR Metrics Tool und der Performance Analyzer.

Für die Messung werden feste Situationen festgelegt, damit vorher und nachher vergleichbar sind, z. B.:

- Stehen an der Wurfposition, Blick auf die Bahn;
- Wurf in die vollen Pins (viele gleichzeitige Kollisionen);
- Neuaufstellen der Pins (neu aus Block 1).

| Situation | Bildrate | CPU-Zeit bzw. -Auslastung | GPU-Zeit bzw. -Auslastung | Auffälligkeiten |
|---|---|---|---|---|
| Stehen, Blick auf die Bahn | … | … | … | … |
| Wurf in die Pins | … | … | … | … |
| Neuaufstellen | … | … | … | … |

### Mögliche Ursachen untersuchen

Jede Person wählt **eine** Vermutung und prüft sie durch eine einzelne Änderung:

| Vermutung | Änderung zum Testen |
|---|---|
| Echtzeitschatten kosten GPU-Zeit | Schatten am Licht ausschalten oder Schattenauflösung und -distanz im URP Asset verringern |
| Viele dynamische Physikobjekte | Anzahl der Pins vorübergehend reduzieren; *Fixed Timestep* vergleichen |
| Aufwendige Effekte | Post-Processing, transparente Materialien oder Partikel abschalten |
| Hohe Auflösung | *Render Scale* im URP Asset verringern |
| Spatializer aus Woche 5 | Meta XR Audio Source an allen Pins ab- und wieder einschalten |
| Eigene Skripte | Im Profiler prüfen, ob eigene `Update`-Methoden auffallen, z. B. `GetComponent` in jedem Frame |

### Ändern und erneut messen

1. Ausgangswerte festhalten (Tabelle oben).
2. **Eine** Änderung vornehmen und committen.
3. Dieselben Situationen erneut messen.
4. Bewerten: Hat die Änderung messbar geholfen? Wie verändert sie das Aussehen oder das Spielgefühl? Lohnt sich der Kompromiss?

Leitfrage: Läuft die Szene bereits im Budget? Wenn ja, welche Reserve braucht ein späteres Gruppenprojekt mit mehr Inhalten?

## Zentrale Begriffe

| Begriff | Arbeitsdefinition |
|---|---|
| **Kinematischer Rigidbody** | Rigidbody, der nicht von der Physik bewegt wird, sondern per Skript oder Animation, aber weiterhin andere Objekte anstößt. |
| **Interpolation (Lerp, SmoothStep)** | Berechnung von Zwischenwerten zwischen Start und Ziel in Abhängigkeit von einem Fortschritt zwischen 0 und 1. |
| **Coroutine** | Methode, die über mehrere Frames hinweg abläuft und mit `yield return` pausiert. |
| **Animator / Animation Clip** | Unity-Komponente bzw. aufgezeichneter Bewegungsablauf für feste Animationen. |
| **Framebudget** | Zeit, die für die Berechnung eines Bildes zur Verfügung steht (bei 72 Hz 13,9 ms). |
| **CPU- bzw. GPU-gebunden** | Die Framezeit wird vor allem durch die CPU bzw. durch die GPU begrenzt. |
| **Reprojektion** | Ersatzbild des Systems, wenn ein Bild nicht rechtzeitig fertig wird. |
| **Passthrough** | Live-Bild der realen Umgebung durch die Kameras des Headsets. |
| **Mixed Reality (MR)** | Virtuelle Objekte werden in den realen Raum eingebettet und können sich auf dessen Geometrie beziehen. |

## 2 h BS – Bowling auf MR übertragen

In der BS-Phase wird die Bowlingszene in eine Mixed-Reality-Variante überführt. Die virtuelle Umgebung entfällt; Bahn, Pins und Kugel stehen per Passthrough im realen Raum.

Vor Beginn wird der Stand committet. Gearbeitet wird in einer **Kopie der Szene** (z. B. `Bowling_MR`), die VR-Fassung bleibt lauffähig.

### Passthrough einrichten

1. Den aus [Woche 2](../Woche_2/Woche_2.md) bekannten Building Block **Passthrough** hinzufügen.
2. Virtuelle Umgebung ausblenden: Boden, Wände, Halle und Skybox deaktivieren. Die Bahn selbst bleibt.
3. Prüfen, ob der Hintergrund der Kamera transparent ist. Sonst verdeckt er das Passthrough-Bild.
4. APK bauen und prüfen: Erscheint die Bahn im realen Raum?

### Bahn im Raum platzieren

Die Bahn muss auf dem realen Boden liegen und in den verfügbaren Platz passen. Dafür gibt es zwei Wege:

| Weg | Umsetzung | Voraussetzung |
|---|---|---|
| **Platzierung durch die Person** | Position und Ausrichtung der Bahn zur Laufzeit festlegen, z. B. per Controller an der aktuellen Position auf Bodenhöhe | Tracking Origin *Floor Level* (seit Woche 2) |
| **Raumerkennung** | Bahn über den Building Block **Anchor Prefab Spawner** bzw. das Mixed Reality Utility Kit (MRUK) am erkannten Boden ausrichten | Raum ist in den Einstellungen der Quest eingescannt; Berechtigung für Raumdaten |

Anschließend werden Maßstab und Bodenhöhe im Headset geprüft. Eine Bahn von rund 18 m passt in keinen Seminarraum. Mögliche Anpassungen sind eine verkürzte Bahn oder ein verkleinerter Aufbau. Wird skaliert, ändert sich auch das physikalische Verhalten. Leitfrage: Was muss bei einer verkleinerten Bahn außer der Größe angepasst werden (Masse, Geschwindigkeit, Wartezeiten in `BallReturn`)?

### Bezug zur realen Umgebung

- **Reale Wände und Möbel:** Ohne weitere Maßnahmen rollt die Kugel durch reale Gegenstände hindurch. Mit der Raumerkennung (z. B. Building Block **Effect Mesh** mit Collidern) prallt sie an der erkannten Geometrie ab. Ohne Raumerkennung wird sie spätestens nach Verlassen der Bahn zurückgeholt.
- **Sichtbarkeit:** Heben sich Kugel, Pins, Punktestand und Hinweise vor dem wechselnden realen Hintergrund ausreichend ab? Feedback und UI aus Woche 5 werden bei Bedarf angepasst.
- **Sicherheit:** Die Wurfrichtung zeigt nicht auf reale Personen oder zerbrechliche Gegenstände. Rund um die Wurfposition bleibt ausreichend freie Fläche.

### Optional für Schnelle

- Reale Hindernisse, z. B. einen Tisch, als Bande in das Spiel einbeziehen.
- Verdeckung virtueller Objekte durch reale Gegenstände ausprobieren (Depth API bzw. Occlusion): Verschwindet eine Kugel hinter einem realen Stuhl?

### Vergleich VR und MR

Zum Abschluss werden die Varianten kurz im Plenum verglichen:

- Was wirkt in MR überzeugender, was störender?
- Wo stößt die Bowling-Idee im realen Raum an Grenzen?
- Für welche Projektideen wäre MR die bessere Wahl, für welche VR?

## Vor- und Nachbereitung außerhalb der Präsenz (optional)

Die MR-Variante wird im eigenen Raum ausprobiert. Festgehalten werden Beobachtungen zu Platzbedarf, Tracking und Raumerkennung sowie die Messwerte und die Bewertung der Optimierung aus Block 2.

Eingesetzte KI-Werkzeuge werden wie gewohnt im README vermerkt.

## Ergebnis der Woche

Zum Abschluss werden folgende Ergebnisse zusammengeführt:

- animiertes Neuaufstellen der Pins und animierte Rückholung der Kugel, abgestimmt mit der Physik;
- Messwerte zur Performance auf der Quest 3 vor und nach einer gezielten Änderung;
- erste MR-Variante der Bowlingszene im realen Raum;
- eigenständig auf der Quest 3 laufende APK.
