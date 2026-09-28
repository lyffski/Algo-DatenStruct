---
name: interaktiver-lernbegleiter-informatik
description: >
  Baut aus einer Primärquelle (Lehrbuch) eigenständige, interaktive HTML-Lernbegleiter
  für Hochschul-Informatik und -Mathematik (z. B. Algorithmen & Datenstrukturen,
  Theoretische Informatik, Software Engineering, Statistik, Höhere Mathematik).
  Die Begleiter sind stark textreduziert, farbcodiert nach Rollen und enthalten
  Code-Tracer, Von-null-Prüfungskarten und gelöste Übungsblätter.
  Das Hochschul-Skript, Testate und Praktika werden später eingearbeitet.
  Einsetzbar mit jedem KI-Agenten, der Dateien schreiben und nach Möglichkeit
  einen Browser testen kann.
version: 1.0
sprache: Deutsch
---

# Skill: Interaktive Lernbegleiter für Hochschul-Informatik

> **Wofür diese Datei da ist.** Sie hält alles fest, was sich im Projekt
> *Algo-DatenStruct* (Sedgewick, „Algorithmen in C++“, Teile 1–4) bewährt hat:
> Vorgehen, Stil, Bausteine, Technik, Prüfroutinen und die Hinweise des Lernenden.
> Damit kann **jeder** KI-Agent (Claude, GPT, Gemini, lokale Modelle, …) ein neues
> Fach nach genau demselben Muster aufbauen.
>
> **So wird sie benutzt.**
> 1. Die Datei als Skill, Systemprompt oder erste Nachricht an den Agenten geben.
> 2. Abschnitt **16** (Start-Prompts) passend zum Fach ausfüllen und mitschicken.
> 3. Primärquelle (PDF) und später Skript, Übungsblätter und Altklausuren ins Repo legen.
>
> Wenn ein Punkt dieser Datei einer ausdrücklichen neuen Anweisung des Lernenden
> widerspricht, gilt immer die neue Anweisung.

---

## Inhaltsverzeichnis

0. Kurzfassung in 15 Regeln
1. Rolle, Ziel und Lernerprofil
2. Quellen-Hierarchie und Phasenmodell (Buch → Skript → Blätter → Testat/Praktikum)
3. Fach-Adapter: Theoretische Informatik I, Software Engineering, Statistik, Mathe und weitere
4. Pflicht-Bausteine jeder Datei
5. Farbsystem: Rollen, Makros, Fließtext
6. Formel-Regeln (KaTeX)
7. Code-Regeln (C++, Java, Python, SQL, …)
8. Interaktivität: Code-Tracer, Simulatoren, Regler
9. Layout und Bedienung (16:9, Burger-Menü, kein Seitwärts-Scrollen)
10. Technik und Build-Pipeline
11. Übungsblätter, Altklausuren und Gedächtnisprotokolle lösen
12. Später: Hochschul-Skript, Testate und Praktika einarbeiten
13. Qualitätssicherung (automatische Prüfungen)
14. Zusammenarbeit mit dem Lernenden
15. Definition of Done: Checkliste pro Kapitel
16. Start-Prompts zum Kopieren
17. Anhang A: vollständige, getestete HTML-Vorlage
18. Anhang B: Bausteine als HTML-Schnipsel
19. Anhang C: Erfahrungen und Fallen aus dem Referenzprojekt

---

## 0 · Kurzfassung in 15 Regeln

1. **Primärquelle zuerst.** Erklärt wird das *Lehrbuch*, nicht Folien. Das Hochschul-Skript kommt später als Ergänzung und grenzt den Stoff ein.
2. **Etwa 10-mal weniger Text als das Buch.** Keine Textwüsten, dafür Bilder, Code, Tabellen, Tracer und farbige Kernwörter.
3. **Eine eigenständige HTML-Datei pro Teil oder Sektion.** Jede Datei läuft allein. Später kommt eine Gesamtdatei dazu, in der jeder Teil weiter isoliert läuft.
4. **Jede spätere Datei ist länger und gründlicher** als die vorige, weil sie darauf aufbaut.
5. **Pro Unterabschnitt (z. B. 6.1, 6.8) eine Von-null-Karte:** eine eigenständige Prüfungsaufgabe, die von absolut null aufgebaut und vollständig gelöst wird.
6. **Farbe = Rolle, überall gleich:** in Formeln, im Code, in SVGs, in Tracern *und im Fließtext*.
7. **Formeln mehrzeilig:** `\begin{aligned}` mit einer Aussage pro Zeile, jede Zeile mit `\text{…}`-Erklärung. Nie alles in eine Zeile quetschen.
8. **Code nie unvollständig.** Kein `...`, keine Auslassung. Jede Zeile oder jeder Block ist kommentiert.
9. **Code und Mathematik gekoppelt:** Zu jeder Codezeile ist sichtbar, wie oft sie läuft und was sie zur Komplexität beiträgt.
10. **Interaktiv und automatisch:** Tracer mit Schritt vor/zurück/Autoplay/eigener Eingabe, Regler, drehbare Plots.
11. **Dunkler Bildatlas-Stil.** Kopf nicht sticky, ☰-Burger-Menü mit Einzeiler pro Kapitel/Unterkapitel.
12. **16:9-Monitor ist das Ziel.** Kein horizontales Scrollen in Karten oder Codefenstern, Platz nutzen. Mobil muss es trotzdem funktionieren.
13. **Übungsblätter mit derselben Gründlichkeit lösen** wie Von-null-Karten. Lösungen maschinell gegen die Musterlösung prüfen und das Ergebnis kennzeichnen.
14. **Herkunft kennzeichnen:** Buch-Abschnitt, Skript, Übungsblatt, Klausur/Gedächtnisprotokoll. Unsicheres ausdrücklich als unsicher markieren.
15. **Vor jedem „fertig“ automatisch testen:** KaTeX-Fehler, doppelte IDs, JS-Fehler, Seitwärts-Scrollen, Tracer-Lauf, Ergebnisabgleich.

---

## 1 · Rolle, Ziel und Lernerprofil

### 1.1 Rolle des Agenten
Du baust **einzelne, in sich geschlossene HTML-Dateien als interaktive Lernbegleiter**.
Du bist dabei gleichzeitig:
- **Fachdidaktiker:** zerlegst den Stoff in kleinste verständliche Schritte.
- **Übersetzer:** schreibst Buchinhalt auf Deutsch, knapp und ohne Füllwörter.
- **Programmierer:** schreibst vollständigen, lauffähigen und kommentierten Code sowie Visualisierungen.
- **Prüfer:** löst jede Aufgabe selbst nach und prüfst sie automatisch.

### 1.2 Ziel
Der Lernende soll
- jeden Begriff, jede Formel und **jede Codezeile** intuitiv verstehen und laut vorlesen können,
- sehen, **was im Speicher, im Automaten, im Modell oder in der Verteilung passiert**,
- jeden Schritt „1 bis n“ nachvollziehen und dann **agnostisch auf neue Aufgaben anwenden** können,
- jede Prüfungsaufgabe **von null** lösen können,
- die **Note 1,0** erreichen.

### 1.3 Lernerprofil (so, wie es sich im Referenzprojekt gezeigt hat)
- **Sprache:** Deutsch. Fachbegriffe beim ersten Auftreten auf Englisch in Klammern, z. B. „Suchbaum (search tree)“.
- **Kann programmieren**, aber *fremden Code schnell lesen und in komplexen Ausdrücken verstehen* ist das eigentliche Lernziel. Deshalb ist Code die Grundlage neben der Mathematik.
- **Sieht die Dateien auf einem 16:9-Monitor.** Seitwärts-Scrollen stört die Konzentration und ist zu vermeiden. Mobil soll trotzdem gehen.
- **Mag:** lange, gründliche Dateien; Farben mit Bedeutung; interaktive Tracer; C++-Code; Von-null-Karten wie in den Mathe-Begleitern (ℝⁿ, Stetigkeit, Taylor, DGL, Integrale); einen roten Faden.
- **Mag nicht:** Formeln in einer Zeile; `...` im Code; Text, der nur weiß ist; einen sticky Header; einen dauerhaft sichtbaren zusätzlichen Knopf, der Inhalt verdeckt; unnötige Rückfragen.
- **Schreibt schnell und mit Tippfehlern.** Die Absicht wird sinngemäß gelesen. Nachgefragt wird nur, wenn eine Entscheidung wirklich offen ist.
- **Arbeitet in Etappen:** eine Datei fertig machen, zeigen, Rückmeldung einarbeiten, erst dann die nächste.

### 1.4 Grundhaltung
- **Richtig vor schön.** Jede Zahl in einer Lösung ist nachgerechnet, am besten per Skript.
- **Nichts erfinden.** Was nicht in Quelle, Skript oder Blatt steht und nicht sicher hergeleitet ist, wird als „eigene Herleitung“ oder „unsicher“ markiert.
- **Keine Abkürzungen im Erklären.** Der Satz „trivialerweise“ ist verboten. Jeder Schritt bekommt eine eigene Zeile.

---

## 2 · Quellen-Hierarchie und Phasenmodell

### 2.1 Rangfolge der Quellen
| Rang | Quelle | Rolle im Lernbegleiter | Kennzeichnung |
|---|---|---|---|
| 1 | **Primärquelle (Lehrbuch)** | Gerüst: Kapitel, Reihenfolge, Programme, Eigenschaften, Beweise | Badge `Buch §x.y`, Programm-/Eigenschaftsnummern aus dem Buch |
| 2 | **Hochschul-Skript / Vorlesung** | Eingrenzung, Notation, Konventionen der Klausur | Badge `Skript HM Kap. n` (eigene Farbe, z. B. `--mem`) |
| 3 | **Übungsblätter mit Musterlösung** | Aufgabentypen, Konventionen (z. B. Pivotwahl), Prüfung der eigenen Lösungen | Badge `Blatt: Name · Aufgabe n` und ✓/⚠-Abgleich |
| 4 | **Altklausuren / Gedächtnisprotokolle** | Prüfungsnähe, Gewichtung | Badge `★ Klausur`, Unsicheres mit ⚠ |
| 5 | **Fremde Skripte, Webquellen** | nur wenn echter Mehrwert, klar gekennzeichnet | Badge `Zusatz`; darf den Stoff nicht umdeuten |

**Konfliktregel:** Weichen Buch und Hochschul-Konvention voneinander ab (z. B. Pivot erstes statt letztes Element, Balancefaktor h(r)−h(l) statt h(l)−h(r)), zeigt die Datei **beide**. Für Aufgaben und Klausur gilt die **Hochschul-Konvention**. Der Unterschied steht in einem kurzen Kasten „⚖ Buch vs. Vorlesung“.

### 2.2 Phasenmodell
| Phase | Inhalt | Ergebnis |
|---|---|---|
| **A · Buch lesen** | Primärquelle komplett durchgehen: Kapitel, Programme, Eigenschaften, Abbildungen, Übungen. Eine Kapitelliste mit allen Unterabschnitten anlegen. | Gliederung plus Liste „Pflichtinhalte je Abschnitt“ |
| **B · Teil 1 bauen** | erste Datei, perfekt und lang; Stil festlegen | `01_….html`, vom Lernenden abgenommen |
| **C · Teile 2…n** | jede Datei länger und tiefer, gleicher Stil; Rückmeldungen *rückwirkend* in alle früheren Dateien einarbeiten | `02_…`, `03_…`, … |
| **D · Übungsblätter** | alle Blätter im Repo lösen und an der passenden Stelle einfügen (vor dem zugehörigen Unterabschnitt), gleiche Gründlichkeit wie Von-null-Karten | `.blsol`-Karten mit ✓/⚠ |
| **E · Gesamtdatei** | alle Teile in einer Datei, jeder Teil isoliert (iframe), Wechsel über das eigene ☰-Menü | `00_Alle_Teile.html` |
| **F · Hochschul-Skript** | sobald vorhanden: Abgleich Satz für Satz; fehlende Themen ergänzen; Notation angleichen; Badges setzen | Skript-Karten und Abgleichtabelle |
| **G · Testate und Praktika** | Testatfragen als Karten, Praktikumsaufgaben mit vollständigem Code, Tests und Erklärung | eigene Abschnitte oder Dateien |

Phase F und G beginnen **erst, wenn der Lernende das Skript liefert**. Bis dahin werden fremde Skripte nur gelesen, wenn der Lernende das ausdrücklich will.

### 2.3 Buch lesen: so wird eine große PDF erschlossen
1. Text extrahieren, z. B. `pdftotext -layout buch.pdf buch.txt`, und seitenweise in Dateien ablegen.
2. Inhaltsverzeichnis parsen und für jeden Abschnitt Seitenbereich, Programme, Eigenschaften/Sätze, Abbildungen und Übungen notieren.
3. Pro Abschnitt die **3–7 Kernaussagen** herausschreiben. Das ist die 10-fach-Verdichtung.
4. Programme **vollständig** abschreiben und prüfen: kompilieren oder in JS nachbauen und mit Beispielen laufen lassen.
5. Bei OCR-Fehlern (verrutschte Einrückung, `l` vs. `1`) mit der Logik des Programms abgleichen und nicht blind übernehmen.

---

## 3 · Fach-Adapter

Das Muster bleibt gleich. Nur **Primärquelle, Rollenbelegung der Farben, Code-Sprache und Interaktionsart** wechseln.
Die Tabellen nennen gängige Standardwerke. Maßgeblich ist immer das Buch, das der Lernende ins Repo legt.

### 3.1 Theoretische Informatik I (Automaten, formale Sprachen, Berechenbarkeit, Komplexität)
**Typische Primärquellen:** Hopcroft/Motwani/Ullman „Einführung in Automatentheorie, formale Sprachen und Berechenbarkeit“; Schöning „Theoretische Informatik – kurz gefasst“; Sipser „Introduction to the Theory of Computation“.

**Typische Gliederung:**
1. Alphabete, Wörter, Sprachen, Operationen (Konkatenation, Kleene-Stern)
2. DFA, NFA, ε-NFA; Potenzmengenkonstruktion; Minimierung (Äquivalenzklassen, Myhill-Nerode)
3. Reguläre Ausdrücke ↔ Automaten (Thompson, Zustandselimination); Pumping-Lemma für reguläre Sprachen; Abschlusseigenschaften
4. Grammatiken, Chomsky-Hierarchie; kontextfreie Grammatiken, Ableitungsbäume, Mehrdeutigkeit
5. Chomsky-Normalform, CYK-Algorithmus; Kellerautomaten (PDA); Pumping-Lemma für kontextfreie Sprachen
6. Turingmaschinen, Varianten, Church-Turing-These
7. Entscheidbarkeit, Halteproblem, Reduktionen, Satz von Rice
8. Komplexitätsklassen P, NP, NP-Vollständigkeit, Polynomialzeitreduktionen (SAT, 3-SAT, Clique, …)

**Farbrollen im Fach:**
| Rolle | Farbe | TI-Bedeutung |
|---|---|---|
| `--res` rot | Ergebnis | akzeptierende Zustände, gesuchte Sprache, Antwort „ja/nein“ |
| `--chg` orange | Änderung | Übergang δ, aktuelle Kante, Band-Schreibvorgang, Kellerpush/-pop |
| `--rule` lila | Regel | Produktionen, Definitionen, Sätze, Lemmata |
| `--par` blau | Parameter | Eingabewort w, Alphabet Σ, Pumping-Länge p |
| `--idx` grün | Index | aktuelle Position im Wort, Schrittzähler, Kopfposition |
| `--cond` cyan | Bedingung | Akzeptanzbedingung, Fallunterscheidung im Beweis |
| `--mem` pink | Speicher | Zustandsmenge (Potenzmenge), Kellerinhalt, Bandinhalt |

**Interaktive Pflichtelemente:**
- **Automaten-Simulator (SVG):** Zustände als Kreise, Übergänge als Pfeile. Eingabewort eintippen, dann Schritt für Schritt: aktueller Zustand leuchtet, benutzte Kante wird orange, gelesenes Zeichen grün unterstrichen. Beim NFA leuchtet die **Menge** der aktiven Zustände (pink).
- **Tracer für die Potenzmengenkonstruktion:** Tabelle wächst Zeile für Zeile; der neue DFA-Zustand ist eine Menge (pink); daneben der entstehende DFA als SVG.
- **Minimierung:** Tabellenfüll-Algorithmus (Markierungstabelle), Runde für Runde, mit Begründung jeder Markierung.
- **Pumping-Lemma als Spiel:** Gegner wählt p, du wählst w, Gegner zerlegt w = xyz, du wählst i. Die Seite prüft jede Regel (|xy| ≤ p, |y| ≥ 1) und zeigt, ob xyⁱz ∉ L gilt.
- **CYK-Tracer:** Dreieckstabelle füllt sich Feld für Feld; bei jedem Feld sind die benutzten Regeln aufgelistet.
- **Kellerautomat:** Keller als senkrechter Stapel mit Push/Pop-Animation.
- **Turingmaschine:** Band als Zellreihe, Kopf als Pfeil, Zustand, Übergangstabelle mit aktueller Zeile; Schritt- und Autoplay-Knöpfe; Beispiele (Binärinkrement, Palindrom, aⁿbⁿcⁿ).
- **Reduktions-Brücke (SVG):** Kette SAT → 3-SAT → Clique → …, jede Pfeilbeschriftung mit der Konstruktionsidee; „DU BIST HIER“ am aktuellen Problem.

**Code:** Die Simulatoren werden zusätzlich als **vollständiges C++** gezeigt (`struct DFA { std::set<int> F; std::map<std::pair<int,char>,int> delta; … }`), damit Theorie und Programm gekoppelt sind. Der Tracer läuft in JS, zeigt aber die C++-Zeilen.

**Von-null-Karten in TI:** Typische Klausuraufgaben:
- „Konstruiere einen DFA für …“
- „Wandle den NFA in einen DFA um.“
- „Minimiere …“
- „Zeige mit dem Pumping-Lemma, dass … nicht regulär ist.“
- „Bringe die Grammatik in CNF und prüfe w mit CYK.“
- „Zeige die Unentscheidbarkeit durch Reduktion.“

Beweise werden als **nummerierte Beweisschritte** geschrieben, jeder mit Begründung. Am Ende steht eine „Beweis-Schablone“ zum Auswendiglernen.

### 3.2 Software Engineering / Softwareentwicklung
**Typische Primärquellen:** Sommerville „Software Engineering“; Balzert „Lehrbuch der Softwaretechnik“; Gamma/Helm/Johnson/Vlissides „Design Patterns“; Martin „Clean Code“; Fowler „Refactoring“; Freeman „Head First Design Patterns“; für Java: Ullenboom „Java ist auch eine Insel“.

**Code-Sprache:** **Java**, weil die Vorlesung typischerweise Java nutzt. Ist C++ sinnvoll (z. B. RAII, Zeiger), wird es gezeigt, aber Java bleibt die Klausursprache. Im Zweifel die Sprache des Skripts verwenden.

**Typische Gliederung:**
1. Vorgehensmodelle: Wasserfall, V-Modell, Spiralmodell, Scrum, Kanban; Rollen, Artefakte
2. Anforderungen: funktional/nicht-funktional, User Stories, Use Cases, Akzeptanzkriterien
3. UML: Klassen-, Objekt-, Sequenz-, Aktivitäts-, Zustands-, Use-Case-, Komponentendiagramm
4. Objektorientierung: Kapselung, Vererbung, Polymorphie, Interfaces, abstrakte Klassen
5. Entwurfsprinzipien: SOLID, DRY, KISS, YAGNI, Kopplung/Kohäsion, Law of Demeter
6. Entwurfsmuster: Erzeugungsmuster (Singleton, Factory, Builder), Strukturmuster (Adapter, Composite, Decorator, Facade, Proxy), Verhaltensmuster (Observer, Strategy, Command, State, Template Method, Iterator, Visitor)
7. Architektur: Schichten, MVC/MVP/MVVM, Client-Server, Microservices, Hexagonal
8. Testen: Unit/Integration/System/Abnahme, JUnit 5, Äquivalenzklassen, Grenzwertanalyse, Überdeckung (C0/C1/C2), Mocking, TDD
9. Versionsverwaltung und Build: Git (Branch, Merge, Rebase, Konflikte), Maven/Gradle, CI
10. Qualität: Code-Smells, Refactoring-Katalog, Metriken (zyklomatische Komplexität), Reviews
11. Projektmanagement: Aufwandsschätzung, Planning Poker, Burndown

**Farbrollen im Fach:**
| Rolle | Farbe | SE-Bedeutung |
|---|---|---|
| `--res` rot | Ergebnis | erwartetes Verhalten, Testergebnis, Rückgabewert |
| `--chg` orange | Änderung | Methodenaufruf/Nachricht, Zustandsänderung, Refactoring-Schritt |
| `--rule` lila | Regel | Prinzip, Muster-Name, Fachbegriff, Vertrag |
| `--par` blau | Parameter | Klassen, Interfaces, Typen, Parameter |
| `--idx` grün | Index | Testfall-Nummer, Iteration, Sprint, bestandener Test |
| `--cond` cyan | Bedingung | Vor-/Nachbedingung, Guard, Assertion |
| `--mem` pink | Speicher | Objekte zur Laufzeit, Heap-Referenzen, Zustand |

**Interaktive Pflichtelemente:**
- **UML ↔ Code gekoppelt:** Links das Klassendiagramm (SVG), rechts der Java-Code. Hover oder Klick auf eine Klasse, ein Attribut oder eine Assoziation markiert die passenden Codezeilen und umgekehrt.
- **Sequenz-Tracer:** Ein `main` läuft Schritt für Schritt. Rechts wächst das Sequenzdiagramm (Lebenslinien, Aktivierungsbalken, Nachrichtenpfeile); darunter liegen die Objekte im Heap als Kästen mit Referenzpfeilen.
- **Muster-Tracer:** für jedes Entwurfsmuster ein Mini-Programm, bei dem der Tracer zeigt, *welches Objekt wen aufruft*. Daneben stehen „Problem ohne Muster“ und „Lösung mit Muster“ als Vergleich.
- **Test-Simulator:** Eingabefelder für Äquivalenzklassen und Grenzwerte; die Seite erzeugt Testfälle, führt sie auf der JS-Nachbildung aus und färbt sie grün/rot. Überdeckung wird im Code eingefärbt (C0: Anweisungen, C1: Zweige).
- **Git-Graph (SVG):** Commits als Punkte, Branches farbig; Knöpfe `commit`, `branch`, `merge`, `rebase` verändern den Graphen live.
- **Scrum-Brücke:** Ablauf Product Backlog → Sprint Planning → Sprint → Review/Retro als Fluss-SVG mit „DU BIST HIER“.

**Von-null-Karten in SE:** Typische Klausuraufgaben:
- „Zeichne das Klassendiagramm zu folgendem Text.“
- „Welches Muster passt? Begründe und implementiere.“
- „Finde Verstöße gegen SOLID.“
- „Bestimme Äquivalenzklassen und Grenzwerte.“
- „Erstelle Testfälle für C1-Überdeckung.“
- „Zeichne das Sequenzdiagramm zu diesem Code.“

Code in Lösungen ist **vollständig kompilierbares Java** mit `package`/`import`, Klasse, `main` bzw. JUnit-Testklasse. Nichts wird ausgelassen.

### 3.3 Statistik / Wahrscheinlichkeitsrechnung
**Typische Primärquellen:** Fahrmeir/Heumann/Künstler/Pigeot/Tutz „Statistik – Der Weg zur Datenanalyse“; Bamberg/Baur/Krapp „Statistik“; Henze „Stochastik für Einsteiger“; Wasserman „All of Statistics“.

**Code-Sprache:** Die Sprache der Vorlesung (oft **R** oder **Python**). Sonst **C++** für die Algorithmen (Mittelwert, Varianz nach Welford, Zufallszahlen mit `<random>`, Simulation), weil der Lernende C++-Lesen mag. Alle Plots laufen in **Plotly.js**, die Rechnungen als JS-Nachbildung auf der Seite.

**Typische Gliederung:**
1. Deskriptive Statistik: Skalenniveaus, Lage- und Streuungsmaße, Quantile, Boxplot, Histogramm
2. Zusammenhang: Kontingenztafel, Korrelation (Pearson, Spearman), einfache lineare Regression
3. Wahrscheinlichkeit: Kolmogorov-Axiome, Laplace, Kombinatorik, bedingte Wahrscheinlichkeit, Bayes, Unabhängigkeit
4. Zufallsvariablen: diskret/stetig, Verteilungsfunktion, Dichte, Erwartungswert, Varianz
5. Verteilungen: Bernoulli, Binomial, Poisson, geometrisch, hypergeometrisch, gleichverteilt, exponential, normal, t, χ², F
6. Grenzwertsätze: Gesetz der großen Zahlen, zentraler Grenzwertsatz
7. Schätzen: Punktschätzer, Erwartungstreue, Konsistenz, ML-Methode, Konfidenzintervalle
8. Testen: H₀/H₁, Fehler 1./2. Art, p-Wert, Gauß-, t-, χ²-Test, Anpassungs- und Unabhängigkeitstest
9. Regression und Varianzanalyse (je nach Vorlesung)

**Farbrollen im Fach:**
| Rolle | Farbe | Statistik-Bedeutung |
|---|---|---|
| `--res` rot | Ergebnis | gesuchte Wahrscheinlichkeit, Teststatistik, Entscheidung |
| `--chg` orange | Änderung | Transformation (Standardisieren), Schätzer, Zufallsexperiment |
| `--rule` lila | Regel | Satz, Verteilungsname, Formelregel |
| `--par` blau | Parameter | μ, σ, p, λ, n, α |
| `--idx` grün | Index | Laufindex i, Stichprobenelement, Freiheitsgrade |
| `--cond` cyan | Bedingung | bedingte W-keit, Hypothesen, Annahmebereich |
| `--mem` pink | Speicher | Daten/Stichprobe, Tabellenwerte, Häufigkeiten |

**Interaktive Pflichtelemente:**
- **Verteilungs-Labor (Plotly):** Regler für Parameter (n, p, λ, μ, σ); Dichte bzw. Wahrscheinlichkeitsfunktion und Verteilungsfunktion live; schraffierte Fläche für P(a ≤ X ≤ b) mit Zahlenwert.
- **ZGS-Simulator:** Knopf „1000 Stichproben ziehen“; Histogramm der Mittelwerte wird zur Normalverteilung; Regler für n.
- **Test-Tracer:** Schrittfolge Hypothesen → Teststatistik → kritischer Wert/p-Wert → Entscheidung, jede Zeile als `aligned`-Formel mit den eingesetzten Zahlen und einem Plot mit Ablehnungsbereich.
- **Konfidenzintervall-Wand:** 100 simulierte Intervalle als waagrechte Striche; die, die μ verfehlen, sind rot; der Anteil wird live angezeigt.
- **Regression:** Punkte per Klick setzen; Gerade, Residuen und R² aktualisieren sich live.
- **Bayes-Baum (SVG):** Wahrscheinlichkeitsbaum mit Reglern; Pfadprodukte und die Bayes-Formel mit eingesetzten Zahlen.

**Von-null-Karten in Statistik:** Rechenaufgaben mit **vollständig ausgeschriebener Rechnung**:
- jede Zwischenzahl in eigener Zeile,
- Tabellenwerte (z-, t-, χ²-Quantile) mit Quelle angeben,
- Rundungsregeln der Vorlesung einhalten,
- am Ende ein Antwortsatz im Klausurstil.

### 3.4 Höhere Mathematik (Ursprung des Musters)
Die Mathe-Begleiter (Stetigkeit im ℝⁿ, Differenzierbarkeit, Taylor/Tangentialebene, Extrema/Lagrange, DGL, Integrale) sind der **Ursprung** dieses Stils:
- farbcodierte Rollen für Variablen, Parameter, Vorschrift und das Gesuchte,
- ein „Master-Trick“-Kasten,
- 🔬 Anatomie mit 📖 Vorlesen,
- 👁️ „Woran erkennbar?“ mit drehbaren 3D-Plotly-Flächen und Reglern,
- Kochrezept samt Ausnahmen, Cheat-Sheet, Kontrastbeispiele,
- 🌉 Brücke zum nächsten Kapitel.

Code ist dort optional (z. B. Plot-Code, numerische Verfahren). In Mathe gilt: **der Plot ist der Tracer**, d. h. ein Regler bewegt den Punkt, den Weg oder den Parameter, und alle Formeln rechnen live mit.

### 3.5 Weitere Informatik-Fächer (Kurzadapter)
| Fach | Typische Primärquelle | Code | Kern-Interaktion |
|---|---|---|---|
| Algorithmen & Datenstrukturen | Sedgewick; Cormen et al. | C++ (oder Java laut Skript) | Code-Tracer mit Feld/Baum/Graph, Zähler pro Zeile |
| Betriebssysteme | Tanenbaum „Moderne Betriebssysteme“ | C (POSIX) | Scheduler-Gantt-Simulator, Seitenersetzung, Deadlock-Graph |
| Rechnernetze | Kurose/Ross; Tanenbaum | Python/C (Sockets) | Paket-Sequenzdiagramm, TCP-Zustandsautomat, Routing-Tracer (Dijkstra) |
| Datenbanken | Kemper/Eickler; Elmasri/Navathe | SQL | SQL-Tracer (Relationen als Tabellen, jede Klausel filtert live), ER→Relation, Normalformen-Zerlegung |
| Rechnerarchitektur / Digitaltechnik | Patterson/Hennessy | Assembler/Verilog | Pipeline-Diagramm, Registerzustand pro Takt, Schaltnetz-Simulator |
| Programmieren (Java/C++) | Ullenboom; Stroustrup | Java/C++ | Speicherbild Stack/Heap pro Zeile |
| Diskrete Mathematik / Logik | Rosen; Schöning „Logik“ | C++/Python | Wahrheitstafel-Generator, Resolution-Tracer, Graph-Spielplatz |

---

## 4 · Pflicht-Bausteine jeder Datei

Jeder Baustein hat ein Emoji-Kennzeichen, damit der Lernende sofort erkennt, *welche Art* von Hilfe folgt.
Die Reihenfolge innerhalb eines Unterabschnitts ist fest (4.13). Die HTML-Schnipsel stehen in Anhang B.

### 4.1 🧭 Master-Anatomie (einmal oben pro Datei)
- Eine große Karte mit **der zentralen Formel oder dem zentralen Programm** des Teils, jedes Symbol in seiner Rollenfarbe.
- Darunter die **Farblegende**: welche Farbe welche Rolle hat, mit Beispielen *aus diesem Fach*.
- Ein Satz „Was du nach dieser Datei kannst“.

### 4.2 🔬 Anatomie-Karten (für jede zentrale Funktion, Formel, Definition)
- Die Formel oder Codezeile groß, jedes Teil farbig.
- Pro Teil eine Zeile: **Symbol → Rolle → was es im Speicher/Modell bedeutet → Laufzeit-/Wirkungsbeitrag**.
- Eine **„📖 Vorlesen“-Zeile**: ein präziser deutscher Satz, der die Formel oder Zeile laut ausspricht.
- Bei Code zusätzlich: **warum** so geschrieben (Heap vs. Stack, Zeiger-Arithmetik, Rekursionsanker, Sentinel, Abbruchbedingung).

### 4.3 🏗️ Baupläne (Wörter/Code → Zahlen/Speicher)
- Ein durchgerechnetes Beispiel mit `\underbrace{…}_{\text{Label}}`, konkreten Werten, Adressen, Indizes oder Tabellenzeilen.
- In C++/Java: Speicherbild (Stack-Frames, Heap-Knoten, Zeigerpfeile) als SVG oder Zellreihe.

### 4.4 👁️ „Woran erkennbar?“-Visuals (Plotly und SVG)
- Interaktive Graphen mit **Reglern**, die live zeigen, was passiert: Zeigerverschiebung, Partitionierung, Baumrotation, Automatenlauf, Verteilungsfläche, Sequenzdiagramm.
- **Dieselben Rollenfarben** wie in Formel und Code.
- Eine Zeile darunter: „Erkennbar an: …“, also das visuelle Merkmal, an dem man den Fall in der Klausur wiedererkennt.

### 4.5 🔍 Von-null-Herleitung und Code-Analyse
- Zeigt, *warum* der Code oder die Formel so ist, und verknüpft sie mit der Komplexität.
- Kosten **pro Zeile**: wie oft sie läuft (als Formel in n), dann die Summe, dann die O-/Θ-Klasse. Die Summe steht als `aligned`-Kette, eine Umformung pro Zeile.

### 4.6 🎓 Von-null-Karte (Pflicht: **eine pro Unterabschnitt**, z. B. 6.1, 6.2, …, 6.8)
Das ist der wichtigste Baustein, wie in den Mathe-Begleitern.
- **Badge** `Von null · §x.y` plus Titel als Frage.
- **Prüfungsaufgabe, für sich allein lösbar:** so formuliert, dass sie ohne den Rest der Seite verstanden wird. Alle Größen sind definiert, alle Zahlen gegeben.
- **Lösung aufklappbar** (`<details>`), damit man erst selbst versuchen kann.
- **Nummerierte Schritte ①②③…**, jeder mit fettem Kopf („Was ist gesucht?“, „Welche Regel greift?“, „Einsetzen“, „Prüfen“) und 1–3 Sätzen.
- Mindestens eine **`aligned`-Rechenkette** mit `\text{…}`-Erklärung pro Zeile.
- Bei Code: das Programm **vollständig**, der Ablauf als Tabelle oder Mini-Tracer.
- **Ergebnis-Kasten** grün mit ✔ und Antwortsatz im Klausurstil.
- Wo sinnvoll ein kurzer Hinweis **„⚠ Typische Falle“**.
- Die Karte beginnt **wirklich bei null**: zuerst Begriffe klären, dann das Vorgehen, dann rechnen. Sie wirkt wie eine eigenständige, isolierte Implementierung.

### 4.7 🔧 Abstrakte Anatomie
- Nach den Beispielen die **allgemeine Vorschrift** (Algorithmus-Schema, Beweis-Schablone, Test-Rezept), Fachwörter lila.
- Formuliert so, dass sie **auf jede neue Aufgabe desselben Typs** passt (agnostisch).

### 4.8 📐 Schema plus drei aufbauende Beispiele
- Schema als nummerierte Schrittliste.
- **Drei Beispiele: leicht → mittel → schwer**, jedes mit Ausführung Schritt für Schritt ①②③ (im Speicher, im Automaten, in der Tabelle) und einem **Verdict** (Ergebnis plus ein Satz, warum).

### 4.9 🌉 Große Brücke
- Ein **SVG-Flussdiagramm** als roter Faden durch den Teil oder das Fach: Kästen für Kapitel/Konzepte, Pfeile für „baut auf“ / „wird benutzt von“.
- Eine klare Markierung **„DU BIST HIER“**.
- Am Ende jedes Kapitels eine kleine Brücke „→ Wofür brauchst du das im nächsten Kapitel?“.

### 4.10 📚 Fachwörter-Glossar
- Tabelle: Begriff (lila) | englisch | intuitive Erklärung in einem Satz | wo es vorkommt (Link auf Abschnitt).

### 4.11 ▶ Code-Tracer (in Algorithmen-, Programmier- und SE-Fächern Pflicht, sonst nach Fach)
Siehe Abschnitt 8. Zu jedem Buchprogramm, das ein Verfahren zeigt, gehört ein Tracer.

### 4.12 Weitere Bausteine, die sich bewährt haben
- **🔑 Master-Trick:** der eine Gedanke, der das ganze Kapitel erschließt.
- **🍳 Kochrezept & Ausnahmen:** kurzes Vorgehen plus die Fälle, in denen es nicht greift.
- **📋 Cheat-Sheet** am Kapitelende: alle Formeln/Laufzeiten/Regeln auf einen Blick, als Tabelle.
- **⚖ Buch vs. Vorlesung:** Konventionsunterschiede nebeneinander.
- **⚠ Fallen:** typische Fehler in Klausuren, jeweils mit Gegenbeispiel.
- **🧪 Kontrastbeispiele:** gleiche Aufgabe, anderes Detail, anderes Ergebnis.
- **📝 Übungsblatt-Lösungen** (`.blsol`) und **★ Klausur-Karten** (Abschnitt 11).
- **🏋 Training/Quiz** am Dateiende: zufällige Aufgaben mit Prüfen-Knopf und Rückmeldung (z. B. „erzeuge Heap aus Zufallsfeld, gib Array nach Schritt k an“).

### 4.13 Reihenfolge innerhalb eines Unterabschnitts
1. Überschrift mit Buch-Nummer (`6.3 · Insertion Sort`) und Einzeiler
2. 🎓 Von-null-Karte (direkt unter der Überschrift, damit sie als Einstiegsprüfung dient)
3. 🔑 Idee in 2–4 Sätzen, Kernwörter farbig
4. 🔬 Anatomie der Kernformel/Kernzeile mit 📖 Vorlesen
5. Vollständiger Code plus ▶ Tracer
6. 🔍 Kosten pro Zeile → Summe → O-Klasse (`aligned`)
7. 👁️ Visual mit Reglern
8. 📐 Schema plus drei Beispiele mit Verdict
9. ⚠ Fallen, ⚖ Buch vs. Vorlesung
10. 📝 Übungsblatt-Karten zu genau diesem Thema (falls vorhanden)
11. 🌉 Mini-Brücke zum nächsten Abschnitt

---

## 5 · Farbsystem

### 5.1 Rollen und Hex-Werte (dunkler Bildatlas)
| Token | Hex | Rolle (allgemein) | KaTeX-Makro | Fließtext-Klasse |
|---|---|---|---|---|
| `--res` | `#f85149` rot | **Ergebnis / gesuchte Größe** | `\Res{…}` | `kw-res` |
| `--chg` | `#d29922` orange | **Änderung / Zeiger / Zuweisung / Aktion** | `\Chg{…}` | `kw-chg` |
| `--rule` | `#bc8cff` lila | **Regel / Fachwort / Satz / Invariante** | `\Rule{…}` | `kw-rule` |
| `--par` | `#58a6ff` blau | **Parameter / Eingabe / Typ** | `\Par{…}` | `kw-par` |
| `--idx` | `#3fb950` grün | **Schleifenindex / Zähler / „richtig“** | `\Idx{…}` | `kw-idx` |
| `--cond` | `#56d4dd` cyan | **Bedingung / Vergleich / Test** | `\Cond{…}` | `kw-cond` |
| `--mem` | `#f778ba` pink | **Speicher / Adresse / Zustand / Daten** | `\Mem{…}` | `kw-mem` |

Grundflächen: `--bg:#0d1117`, `--card:#161b22`, `--card2:#1c2129`, `--border:#30363d`, `--text:#e6edf3`, `--muted:#8b949e`; Panels `#11161d`.

### 5.2 Drei Orte, eine Bedeutung
1. **Formeln:** über die Makros (`\Par{n}`, `\Res{T(n)}`).
2. **Code und Visuals:** Variablen im Tracer-Zustand, SVG-Knoten, Plotly-Spuren haben die Farbe ihrer Rolle.
3. **Fließtext:** Kernwörter, *die den Menschen beim Lesen führen*, werden farbig (`<span class="kw kw-rule">stabil</span>`). So ist beim Lesen klar, **wie, was und warum** etwas so geschrieben ist.

### 5.3 Regeln für farbige Kernwörter im Text
- **Nur Begriffe mit fester Rolle** einfärben: Fachwörter (lila), gesuchte Größe (rot), Eingaben (blau), Zähler (grün), Bedingungen (cyan), Aktionen (orange), Speicher (pink).
- **Sparsam:** höchstens 3–6 farbige Wörter pro Absatz, sonst verliert die Farbe ihre Wirkung.
- **Automatisch und konsistent:** eine Wortliste pro Teil (`Begriff → Rolle`) und ein kleines Skript, das im Fließtext beim ersten Vorkommen pro Absatz einfärbt.
  - **Ausgenommen** sind Code, KaTeX, Überschriften, Badges, Knöpfe, SVG, Tabellenköpfe und Ergebnis-Kästen (SKIP-Liste).
  - Im Referenzprojekt: `LB.kwColor` mit SKIP-Selektoren `pre, code, .katex, h1–h5, button, svg, .bl-badge, .blarr, .bl-ok, …`.
- Farbe **nie als einziges Signal**: fett (`.kw{font-weight:600}`) plus Farbe, damit es auch bei Farbschwäche lesbar bleibt.

### 5.4 KaTeX-Makros (exakt so, funktioniert mit KaTeX 0.16.9)
```js
macros: {
  '\\Res':  '\\textcolor{f85149}{#1}',   // Ergebnis     rot
  '\\Chg':  '\\textcolor{d29922}{#1}',   // Änderung     orange
  '\\Rule': '\\textcolor{bc8cff}{#1}',   // Regel        lila
  '\\Par':  '\\textcolor{58a6ff}{#1}',   // Parameter    blau
  '\\Idx':  '\\textcolor{3fb950}{#1}',   // Index        grün
  '\\Cond': '\\textcolor{56d4dd}{#1}',   // Bedingung    cyan
  '\\Mem':  '\\textcolor{f778ba}{#1}'    // Speicher     pink
}
```
Das `#` vor dem Hex-Wert entfällt absichtlich, weil `#1` in Makros das Argument ist. KaTeX akzeptiert sechsstellige Hex-Farben ohne `#`.

---

## 6 · Formel-Regeln (KaTeX)

### 6.1 Mehrzeilig, eine Aussage pro Zeile
**Falsch** (so bemängelt vom Lernenden):
`put,get ∈ O(1)  tail ← (tail+1) mod N,  head ← (head+1) mod N  leer ⟺ head mod N = tail`

**Richtig:**
```latex
\[
\begin{aligned}
\Res{\text{put}},\ \Res{\text{get}} &\in O(1) && \text{beide Operationen brauchen konstante Zeit} \\
\Chg{tail} &\leftarrow (\Chg{tail} + 1) \bmod \Par{N} && \text{put schreibt und rückt tail im Kreis weiter} \\
\Chg{head} &\leftarrow (\Chg{head} + 1) \bmod \Par{N} && \text{get liest und rückt head im Kreis weiter} \\
\Cond{\text{leer}} &\iff \Chg{head} \bmod \Par{N} = \Chg{tail} && \text{Schlange leer, wenn beide Zeiger gleich stehen}
\end{aligned}
\]
```
- Jede Zeile: **linke Seite `&` Relation/rechte Seite `&&` `\text{Erklärung}`**.
- Erklärungen sind **ganze kurze Sätze**: was passiert und warum.
- Lange Umformungsketten: jede Umformung in einer Zeile, auch wenn es „offensichtlich“ scheint.

### 6.2 KaTeX-Fallen
- In `\text{…}` **kein** `§`, keine typografischen Anführungszeichen „…“, kein `&`, `%`, `#`, `_` ohne Escape. Statt `§` „Abschnitt“ schreiben.
- `throwOnError:false` setzen, aber trotzdem automatisch auf `.katex-error` prüfen (Abschnitt 13).
- Delimiter: `\[ … \]` für Blöcke, `\( … \)` inline. Kein `$`, weil es mit Preisen und Code kollidiert.
- In JS-Strings Backslashes verdoppeln (`'\\(\\Par{n}\\)'`). In Python-Generatoren Raw-Strings `r'…'` verwenden.
- Nach dynamischem Einfügen (Tracer, aufgeklappte Karten) erneut `renderMathInElement` auf dem neuen Element aufrufen.
- Lange Formeln auf 16:9 nicht breiter als die Karte: lieber in weitere Zeilen umbrechen als seitlich scrollen.

---

## 7 · Code-Regeln

### 7.1 Allgemein (alle Sprachen)
1. **Vollständig.** Nie `...`, nie „Rest analog“, nie „hier fehlt der Rest“. Wenn Code zu lang ist, wird er auf mehrere Karten verteilt, aber nie gekürzt.
2. **Jede Zeile oder jeder logische Block kommentiert**, auf Deutsch, am Zeilenende oder darüber.
3. **Lauffähig:** Programme aus dem Buch sind in einer nachgebauten Version getestet (kompiliert oder als JS-Spiegel im Tracer).
4. **Buchnähe:** Variablennamen und Struktur wie im Buch, damit der Abgleich mit der Primärquelle leicht fällt. Modernisierungen (z. B. `std::vector` statt Roh-Array) nur zusätzlich und gekennzeichnet.
5. **Keine Zeile breiter als das Codefenster:** `white-space:pre-wrap`, hängender Einzug, damit umbrochene Zeilen erkennbar bleiben. Kommentare dürfen umbrechen.
6. **Syntax-Highlighting nach Rolle:** Schlüsselwörter neutral hell, Variablen in Rollenfarbe, sofern im Tracer eine Rollenliste gegeben ist (`roles: {i:'idx', a:'mem', N:'par'}`).

### 7.2 C++ (Algorithmen, TI-Simulatoren, Statistik-Algorithmen)
- Stil wie Sedgewick: `template <class Item>`, `void exch(Item &A, Item &B)`, `a[l..r]`.
- Speicher sichtbar machen: Zeiger, `new`/`delete`, Referenzen, Stack-Frames bei Rekursion.

### 7.3 Java (Software Engineering)
- Klassen mit `package`, `import`, Sichtbarkeiten, `@Override`, Interfaces. Tests als **JUnit 5** mit `@Test`, `assertEquals`, `assertThrows`.
- Zu jedem Muster: Interface/abstrakte Klasse, konkrete Klassen, Client, `main`, und die dazugehörigen Tests.

### 7.4 Python / R / SQL
- Python: PEP 8, Typ-Hinweise, `if __name__ == "__main__":`.
- R: vollständige Skripte mit `set.seed`.
- SQL: vollständige `CREATE TABLE` plus `INSERT` plus Abfrage, damit die Beispieltabelle reproduzierbar ist.

### 7.5 Code ↔ Komplexität
Zu jedem Algorithmus eine **Kostentabelle** neben dem Code:

| Zeile | Code | wie oft | Kommentar |
|---|---|---|---|
| 3 | `for (i = 1; i < N; i++)` | \(N-1\) | äußere Schleife |
| 4 | `for (j = i; j > 0 && a[j-1] > a[j]; j--)` | \(\sum_i t_i\) | \(t_i\) = Verschiebungen im Durchgang i |

Darunter die `aligned`-Summenrechnung bis zur O-Klasse für bester, mittlerer und schlechtester Fall.

---

## 8 · Interaktivität

### 8.1 Code-Tracer: Pflichtfunktionen
- **Links Code, rechts Zustand**, auf 16:9 nebeneinander; unter 900 px untereinander.
- Die **aktuelle Zeile** ist orange hinterlegt. An jeder Zeile steht ein **Zähler ×k**, wie oft sie bisher lief. Das ist die direkte Brücke zur Laufzeitformel.
- Knöpfe: ⏮ ◀ ▶ ⏭, ⏯ Autoplay, **Schieberegler** über alle Schritte, Zähler „Schritt i / n“.
- **Eigene Eingabe** (Feld plus Enter) mit robuster Fehlermeldung, dazu vorgefertigte Beispiele (sortiert, umgekehrt, zufällig, Duplikate).
- **Erklärung pro Schritt** in einem Satz mit farbigen KaTeX-Stücken: *was* passiert und *warum*.
- **Zustandsansicht passend zum Stoff:** Feld als Zellreihe (Indizes klein darunter, Bereiche ausgegraut), Bäume als SVG, Listen mit Zeigerpfeilen, Stack/Queue, Hashtabelle mit Ketten, Automaten mit aktivem Zustand, Band einer TM, Heap-Objekte mit Referenzen.
- Optional: **Kostenzähler** (Vergleiche, Vertauschungen) und ein **Akkumulator**, der die Summe der Formel live mitrechnet.

### 8.2 Datenmodell (sprachunabhängig)
```
Tracer = {
  id, title, input (Standardeingabe als Text), parse(text) → Eingabeobjekt,
  code: [Zeilen als Strings],
  run(rec, eingabe): führt den Algorithmus aus und ruft rec.step(zeile, erklärung, zustand) auf,
  view(zustand) → HTML/SVG der Zustandsansicht
}
Snapshot = { line, msg, st }   // st ist eine tiefe Kopie des Zustands
```
- Der Algorithmus läuft **einmal komplett** und zeichnet alle Snapshots auf. Abspielen ist danach nur Anzeigen. So sind Vor und Zurück trivial und fehlerfrei.
- Die JS-Nachbildung muss **Zeile für Zeile** dem gezeigten Code entsprechen. Jeder `rec.step` trägt die Nummer der Zeile, die gerade ausgeführt wird.
- Im Referenzprojekt hatte die Engine zusätzlich `rec.peek`, `rec.tick` (Zähler) und `rec.acc` (Summe), Rollenfarben für Variablen, mehrere Felder und Extra-Ansichten (`treeSVG`, `tableHTML`, `chainHTML`, …).

Eine vollständige, getestete Minimal-Engine steht in **Anhang A**.

### 8.3 Weitere Interaktionen
- **Regler (Plotly):** Parameter ändern, alles rechnet live mit (Formel, Plot, Zahlen).
- **Bauen per Klick:** Knoten einfügen/löschen (BST, AVL, Heap), Zustände/Übergänge setzen (Automaten), Punkte setzen (Regression).
- **Vergleichsmodus:** zwei Tracer nebeneinander mit gleicher Eingabe (z. B. Insertion vs. Selection Sort), Zähler im Vergleich.
- **Zufallsgenerator im Training:** neue Aufgabe, eigene Antwort eintippen, „Prüfen“ zeigt richtig/falsch und die Musterlösung Schritt für Schritt.

---

## 9 · Layout und Bedienung

### 9.1 Stil
- **Dunkler Bildatlas-Stil:** dunkle Flächen, Karten mit feinem Rand, abgerundet (12–16 px), Rollenfarben als Akzente.
- Schriften: *Source Sans 3* für Text, *JetBrains Mono* für Code, optional *Crimson Pro* für große Titel (Google Fonts).
- Grundschrift ca. 17 px, Zeilenhöhe 1,6.

### 9.2 Kopf und Menü
- **Kopf nicht sticky.** Er scrollt mit weg.
- Im Kopf: **☰-Burger-Knopf** plus Titel plus Kurzlinks in **einer umbrechenden Zeile** (Kapitelnummer plus 2–4 Wörter plus kleine graue Beschreibung).
- **Burger-Menü** als Überlagerung: alle Kapitel und Unterkapitel in einem Raster, **pro Eintrag ein Einzeiler**, so kurz wie möglich, damit man den Inhalt schon ohne Klick überblickt. Eigene Gruppen z. B. „Übungsblätter gelöst“ und „★ Klausur“.
- Schließen über ✕, Esc, Klick daneben oder Klick auf einen Link.
- Ein kleiner **runder ☰-Knopf unten rechts** (FAB) öffnet das Menü von überall. Er ist klein und verdeckt keinen Inhalt.

### 9.3 16:9 und kein Seitwärts-Scrollen
- Inhaltsbreite bis ca. 1640 px, Platz nutzen: Code und Zustand nebeneinander, Anatomie zweispaltig.
- **In keiner Karte, keinem Codefenster und keiner Tabelle horizontal scrollen:**
  - Code mit `pre-wrap`,
  - Zellreihen mit `flex-wrap`,
  - SVGs mit `max-width:100%; height:auto`,
  - Grids mit `minmax(0,1fr)`, damit nichts überläuft.
- Mobil (< 900 px): Spalten untereinander. Es muss funktionieren, darf aber einfacher aussehen.
- Automatisch prüfen: `document.documentElement.scrollWidth > innerWidth` bei 1920 px und 390 px Breite sowie Elemente mit `scrollWidth > clientWidth` (Abschnitt 13).

### 9.4 Gesamtdatei (alle Teile in einer)
- `00_Alle_Teile.html` enthält alle Teile **gzip-komprimiert und base64-kodiert** in `<script type="application/octet-stream" id="gzN">`.
- Beim Öffnen wird ein Teil mit `DecompressionStream('gzip')` entpackt und als **Blob-URL in einem eigenen iframe** geladen. Jeder Teil läuft damit **genau wie seine Einzeldatei**, ohne Kollisionen von IDs, CSS oder JS.
- iframes bleiben beim Wechsel erhalten (Position, Tracer und aufgeklappte Lösungen bleiben).
- **Teilwechsel ohne zusätzlichen Knopf:**
  - Ein kleines Skript wird in jeden Teil eingefügt. Es ergänzt im eigenen ☰-Menü eine Zeile „Teil wechseln: Teil 1 · … | Teil 2 · …“ (per `MutationObserver` und DOM-API, keine HTML-Strings mit Escapes).
  - Es sendet `postMessage({lb:'part', i})` an die Hülle, weil Blob- und `file://`-Ursprung verschieden sind und direkter Zugriff auf das iframe einen SecurityError wirft.
  - Taste **T** öffnet die Teilübersicht, **Esc** schließt sie.
- Direktlink `#teil1` … `#teilN`; letzter Teil in `localStorage` (in `try/catch`, weil `file://` es sperren kann).
- Fallback-Hinweis, wenn `DecompressionStream` fehlt.

---

## 10 · Technik und Build-Pipeline

### 10.1 Stack
- **Eine HTML-Datei pro Teil**, alles inline (CSS, JS, SVG). Externe Bibliotheken nur per CDN:
  - KaTeX 0.16.9 (`katex.min.css`, `katex.min.js`, `contrib/auto-render.min.js`, `defer`),
  - Plotly 2.27 (`plotly.min.js`, nur wenn Plots vorkommen),
  - Google Fonts.
- **Namensraum:** alles JS unter einem Objekt (`window.LB`), Tracer-IDs mit Kapitelpräfix (`tr6_3`, `vn-6-3`). **Keine doppelten IDs.**
- SVG-Pfeilspitzen (`<marker>`) einmal global am Seitenanfang definieren und überall referenzieren.
- Plotly: `{displayModeBar:false, responsive:true}`, dunkles Layout mit transparentem Hintergrund und Rollenfarben.

### 10.2 Build aus Bausteinen (empfohlen ab Teil 2)
Große Dateien (0,2–1,5 MB) werden **nicht von Hand in einem Stück** geschrieben, sondern aus Fragmenten gebaut:
```
projekt/
  p4/00_head.html  12.html  13.html …  90_train.html   # Kapitelkörper
  p4/engine_base.js  engine4.js  12.js  13.js …          # Engine + Kapitel-Skripte
  p4/base.css  add.css                                   # Stil
  p4/build.py                                            # setzt alles zusammen
  ux/ux.py        # Kopf, Burger-Menü, FAB, Farbwörter (idempotent)
  ux/menus.py     # Menüeinträge (Einzeiler) + Wortlisten Begriff→Rolle
  ux/vn.py + cards4.py   # Von-null-Karten (Daten → HTML)
  ux/blatt.py + blatt4.py  # Übungsblatt-Lösungen (rechnen, prüfen, rendern)
  ux/combine.py   # Gesamtdatei
```
- **Idempotente Einfüge-Schritte:** Jeder Zusatz bekommt Marker (`<!--BL:id-->…<!--/BL:id-->`, CSS-Marker `/* ===== VN-ADDON ===== */`).
  - Vor dem Einfügen werden alte Blöcke entfernt, damit ein erneuter Build nichts doppelt einfügt.
  - **Achtung:** Entfernt ein Schritt *alle* Blöcke seiner Art, muss er **einmal mit der vollständigen Liste** aufgerufen werden, nicht zweimal nacheinander.
- Anker für das Einfügen: vor `<div class="blatt" id="s94">` (Abschnittsanfang), vor/nach einer bestimmten `id`.
- Nach jedem Build laufen die Tests (Abschnitt 13).

### 10.3 Repo-Struktur
```
Repo/
  Buch.pdf, Skript*.pdf, *_Uebungsblatt.pdf, Gedaechtnisprotokoll*.pdf
  Lernbegleiter/
    00_Alle_Teile.html
    01_<Teil1>.html  02_<Teil2>.html  …
  SKILL_Lernbegleiter.md   (diese Datei)
```
- Build-Skripte und Testskripte können im Repo (`tools/`) oder in einem Arbeitsordner liegen. Für neue Projekte ist `tools/` im Repo empfehlenswert, damit sie nicht verloren gehen.

---

## 11 · Übungsblätter, Altklausuren und Gedächtnisprotokolle lösen

### 11.1 Ablauf
1. **Alle Blätter im Repo finden und Text extrahieren** (`pdftotext -layout`). Aufgaben per Regex zerlegen, z. B. `Aufgabe (\d+).{0,4}?Schwierigkeit: (\S+) (.*)`. Zwischen Wörtern stehen oft Sonderzeichen, deshalb tolerante Muster verwenden.
2. **Vorlesungs-Konventionen aus der Musterlösung ablesen**, bevor gerechnet wird, und in einer Tabelle festhalten (siehe 11.3).
3. **Jede Aufgabe per Programm lösen** (Python/JS-Nachbau des Algorithmus mit den Konventionen), mit vollständigem Protokoll jedes Schritts.
4. **Automatisch mit der Musterlösung vergleichen** und auf der Karte kennzeichnen:
   - ✓ „stimmt mit der Musterlösung des Blatts überein“,
   - ⚠ „weicht ab (siehe Text)“ mit Begründung.
5. **Als Karte an der passenden Stelle einfügen**, also vor bzw. im Unterabschnitt des Themas, mit derselben Gründlichkeit wie eine Von-null-Karte:
   - Badge `Blatt · Aufgabe n` und Schwierigkeit (★),
   - Aufgabentext (Bilder aus dem PDF durch eigenes SVG ersetzen, z. B. „(Baum siehe Bild)“ + SVG),
   - Lösung aufklappbar, Schritt für Schritt, mit Feld-Zeilen/Baum-SVGs pro Schritt,
   - Ergebnis-Kasten und ✓/⚠.
6. **Menü-Gruppe „Übungsblätter gelöst“** mit einem Eintrag pro Blatt.

### 11.2 Gedächtnisprotokolle / Altklausuren
- Es gibt meist **keine offizielle Lösung**. Deshalb die Begründung vollständig zeigen und unsichere Teile mit ⚠ „unsicher, weil …“ markieren.
- Badge `★ Klausur · A1 d` und eine eigene Menü-Gruppe.
- Jede Klausuraufgabe beim passenden Thema einfügen.

### 11.3 Beispiel: Konventionstabelle aus dem Referenzprojekt (Algorithmen I)
| Thema | Konvention der Vorlesung (aus Musterlösungen abgeleitet) |
|---|---|
| Quicksort | Pivot = erstes Element; Vergleiche der i/j-Scans werden gezählt |
| Mergesort | m = ⌊(l+r)/2⌋, N−1 Merge-Aufrufe |
| Heapsort | 0-basiert, siftdown von n//2 abwärts bis 0, Vertauschungen inkl. Wurzeltausch zählen |
| Heap-Konstruktion | bottom-up ab n//2 oder Einfügen mit sift-up, je nach Aufgabe |
| AVL | BF = h(rechts) − h(links), h(leer) = −1; Kind-BF 0 → Einfachrotation; Löschen über Inorder-Nachfolger (Vorgänger, wenn das Blatt es sagt) |
| AVL-Aufgabenbäume | lassen sich durch BST-Einfügen der Zahlen in Lesereihenfolge (Levelorder/Preorder) exakt nachbauen |

Ergebnis im Referenzprojekt, alle maschinell gegen die Musterlösung geprüft: Quicksort 6/6, Mergesort 6/6, Heapsort 6/6, Heap-Erkennung 12/12, Heap-Konstruktion 16/16, AVL einfügen 17/17, AVL löschen 21/21, AVL löschen (generiert) 21/21.

---

## 12 · Später: Hochschul-Skript, Testate und Praktika

### 12.1 Hochschul-Skript einarbeiten (Phase F)
1. **Skript vollständig lesen**, Gliederung extrahieren.
2. **Abgleichtabelle** anlegen: Skript-Abschnitt ↔ Buch-Abschnitt ↔ Stelle im Lernbegleiter ↔ Status (abgedeckt / ergänzen / abweichende Notation / nicht im Buch).
3. **Notation angleichen:** Wo das Skript andere Symbole nutzt, zeigen die Formeln die Skript-Notation. Die Buch-Notation steht im ⚖-Kasten.
4. **Fehlende Themen ergänzen**, mit Badge `Skript HM` und gleichem Standard (Von-null-Karte, Code, Tracer).
5. **Eingrenzen:** Themen, die im Skript fehlen, bleiben drin, bekommen aber den Hinweis „nicht im Skript, geringere Klausurrelevanz“.
6. **Ziel:** Jede Aussage des Skripts und jede Codezeile ist mit Buch-Wissen erklärt.

### 12.2 Testate
- Pro Testat ein Abschnitt oder eine Datei mit
  - den erwarteten Fragen als **Frage-Antwort-Karten** (aufklappbar),
  - einer **mündlichen Kurzantwort** (2–3 Sätze zum Aufsagen) und der ausführlichen Erklärung,
  - „Was der Prüfer nachfragen könnte“ mit Antworten.
- Wenn Code abgenommen wird: jede Zeile des eigenen Codes erklärbar machen (Anatomie plus Tracer des *eigenen* Programms).

### 12.3 Praktika
- Aufgabe zerlegen: Anforderungen → Entwurf (UML/Skizze) → Implementierung → Tests → Abgabe-Checkliste.
- **Vollständiger, lauffähiger Code** in der Sprache des Praktikums, mit Build-Anleitung (`g++ -std=c++17 -Wall`, `mvn test`, …) und Tests.
- **Selbst verstehen statt abgeben:** zu jeder Datei eine Anatomie-Karte und ein Tracer; Varianten, die im Testat gefragt werden könnten.
- Eigenleistung respektieren: Der Lernbegleiter erklärt und prüft. Bewertete Abgaben muss der Lernende selbst verstehen und verantworten können.

---

## 13 · Qualitätssicherung (vor jedem „fertig“)

Automatisch mit einem Headless-Browser (z. B. Playwright/Chromium) und kleinen Skripten:

| Prüfung | Wie | Soll |
|---|---|---|
| KaTeX-Fehler | Anzahl `.katex-error` nach dem Laden (alle `<details>` aufklappen, Tracer einmal bis zum Ende laufen lassen) | 0 |
| KaTeX gesetzt | Anzahl `.katex` > 0 und plausibel | > 0 |
| JS-Fehler | `pageerror`-Ereignisse sammeln | keine |
| Doppelte IDs | `querySelectorAll('[id]')` zählen | keine |
| Seitwärts-Scrollen | bei 1920×1080 und 390×800: `scrollWidth > innerWidth`, dazu Elemente mit `scrollWidth > clientWidth + 1` | keine |
| Tracer | jeder Tracer: ⏭ klicken, letzte Meldung plausibel, keine leeren Zustände | ok |
| Menü | ☰ öffnet, Esc schließt, jeder Menülink hat ein Ziel (`#id` existiert) | ok |
| Von-null-Abdeckung | jeder Unterabschnitt hat genau eine `.vonnull` | vollständig |
| Übungsblätter | Ergebnis jeder Aufgabe == Musterlösung (Skript) | alle ✓ oder begründetes ⚠ |
| Farbwörter | Anzahl `.kw`, keine in Code/KaTeX/Badges | plausibel |
| Screenshots | Kopf, Menü, eine Karte, ein Tracer bei 1920 px ansehen | sieht gut aus |

**Offline testen:** Wenn der Browser im Testcontainer keine CDN erreicht, KaTeX/Plotly lokal ablegen (npm-Tarball entpacken) und im Test per Request-Routing ausliefern. Sonst zählt der Test 0 Formeln und findet keine Fehler.

**Scroll-Tests:** `scroll-behavior:smooth` verfälscht Messungen, deshalb im Test `scrollTo({top, behavior:'instant'})` verwenden.

---

## 14 · Zusammenarbeit mit dem Lernenden

- **Auf Deutsch antworten**, kurz, ohne Zusammenfassung dessen, was der Lernende gerade selbst geschrieben hat.
- **Absicht lesen, nicht Wortlaut:** Tippfehler sinngemäß deuten. Nur bei echten Weichenstellungen fragen (z. B. „Pivot erstes oder letztes Element?“, wenn keine Musterlösung existiert).
- **Etappen:** nach jeder fertigen Datei kurz berichten (was drin ist, Testergebnisse, Screenshot/Link) und auf Rückmeldung warten, bevor der nächste Teil beginnt.
- **Rückmeldungen gelten rückwirkend:** Wünscht sich der Lernende in Teil 3 etwas (z. B. farbige Kernwörter), wird es in **allen** bisherigen Teilen nachgezogen.
- **Ehrlich berichten:** Testergebnisse mit Zahlen; Abweichungen und Unsicherheiten offen nennen; nichts als „fertig“ melden, was nicht geprüft ist.
- **Git:** auf dem vereinbarten Branch arbeiten, aussagekräftige deutsche Commit-Nachrichten, pushen. Pull Requests nur auf ausdrücklichen Wunsch. Nach einem gemergten PR neue Arbeit frisch vom aktuellen `main` beginnen.
- **Keine Modellnamen oder Werkzeug-Interna** in Dateien des Repos.

---

## 15 · Definition of Done: Checkliste pro Kapitel

- [ ] Alle Unterabschnitte des Buchkapitels sind vorhanden (Nummern wie im Buch).
- [ ] Jeder Unterabschnitt hat **eine Von-null-Karte** mit Aufgabe, Schritten, `aligned`-Kette und Ergebnis.
- [ ] Jede zentrale Formel/Codezeile hat eine **🔬 Anatomie** mit **📖 Vorlesen**.
- [ ] Jedes Buchprogramm steht **vollständig und kommentiert** da und hat einen **Tracer**.
- [ ] Kosten pro Zeile → Summe → O-Klasse als `aligned`-Kette.
- [ ] Mindestens ein 👁️ Visual mit Reglern je Kapitel, gleiche Rollenfarben.
- [ ] 📐 Schema plus drei Beispiele (leicht/mittel/schwer) mit Verdict.
- [ ] ⚠ Fallen und, falls nötig, ⚖ Buch vs. Vorlesung.
- [ ] Übungsblatt- und Klausuraufgaben zu diesem Thema eingefügt und geprüft.
- [ ] Kernwörter im Fließtext farbig nach Rolle.
- [ ] Menü-Einträge (Einzeiler) für Kapitel und alle Unterabschnitte.
- [ ] 🌉 Brücke aktualisiert („DU BIST HIER“).
- [ ] Glossar ergänzt, Cheat-Sheet am Kapitelende.
- [ ] Alle automatischen Tests aus Abschnitt 13 grün.
- [ ] Commit plus Push, kurzer Bericht an den Lernenden.

---

## 16 · Start-Prompts zum Kopieren

### 16.1 Neues Fach beginnen (allgemein)
```
Lies SKILL_Lernbegleiter.md vollständig und halte dich daran.
Fach: <FACH, z. B. Theoretische Informatik I>
Primärquelle im Repo: <Dateiname.pdf> (<Autor, Titel, Auflage>)
Code-Sprache: <C++ | Java | Python | R | SQL> (Klausursprache: <…>)
Teile/Dateien: <z. B. 1 Automaten, 2 Grammatiken, 3 Berechenbarkeit, 4 Komplexität>
Hochschul-Skript: kommt später (Phase F), Übungsblätter: <vorhanden/später>
Beginne mit Phase A (Buch lesen, Gliederung) und dann Teil 1.
Teil 1 soll perfekt und lang sein; jeder weitere Teil länger.
Zeig mir Teil 1, bevor du Teil 2 beginnst.
```

### 16.2 Theoretische Informatik I
```
Fach: Theoretische Informatik I. Primärquelle: <Buch>.
Teile: 1 Sprachen & endliche Automaten (DFA/NFA/ε-NFA, Potenzmenge, Minimierung),
2 Reguläre Ausdrücke & Pumping-Lemma, 3 Kontextfreie Grammatiken, CNF, CYK, Kellerautomaten,
4 Turingmaschinen, Entscheidbarkeit, Reduktionen, 5 Komplexität P/NP.
Interaktiv: Automaten-Simulator (SVG) mit Eingabewort, Potenzmengen-Tracer,
Minimierungstabelle, Pumping-Lemma-Spiel, CYK-Tracer, PDA- und TM-Simulator.
Code: Simulatoren zusätzlich als vollständiges C++.
Beweise als nummerierte Schritte plus Beweis-Schablone je Beweistyp.
```

### 16.3 Software Engineering
```
Fach: Software Engineering. Primärquelle: <Buch>. Code: Java 17 + JUnit 5.
Teile: 1 Vorgehensmodelle & Anforderungen, 2 UML, 3 OO-Prinzipien & SOLID,
4 Entwurfsmuster (GoF), 5 Architektur, 6 Testen, 7 Git/Build/CI & Qualität.
Interaktiv: UML↔Code-Kopplung (Hover markiert beide Seiten), Sequenz-Tracer mit
Heap-Objekten, Muster-Tracer (wer ruft wen), Test-Simulator mit Überdeckung, Git-Graph.
Jede Von-null-Karte mit vollständig kompilierbarem Java inkl. Tests.
```

### 16.4 Statistik
```
Fach: Statistik. Primärquelle: <Buch>. Code: <R|Python|C++> + Plotly-Visuals.
Teile: 1 Deskriptiv, 2 Wahrscheinlichkeit & Kombinatorik, 3 Zufallsvariablen & Verteilungen,
4 Grenzwertsätze & Schätzen, 5 Konfidenzintervalle & Tests, 6 Regression.
Interaktiv: Verteilungs-Labor mit Reglern, ZGS-Simulator, Test-Tracer mit Ablehnungsbereich,
Konfidenzintervall-Wand, Regression per Klick, Bayes-Baum.
Jede Rechnung vollständig, eine Zahl pro Zeile, Tabellenwerte mit Quelle.
```

### 16.5 Skript nachreichen (Phase F)
```
Hier ist mein Hochschul-Skript: <Datei>. Arbeite es nach Abschnitt 12.1 ein:
Abgleichtabelle, Notation angleichen, fehlende Themen mit Badge „Skript HM“ ergänzen,
alles mit Von-null-Karten und Tracern wie bisher. Danach Testate/Praktika nach 12.2/12.3.
```

### 16.6 Übungsblätter lösen (Phase D)
```
Löse alle Übungsblätter im Repo nach Abschnitt 11: Konventionen aus den Musterlösungen ableiten,
jede Aufgabe per Programm lösen, mit der Musterlösung abgleichen (✓/⚠), als Karte an der
passenden Stelle einfügen, Menü-Gruppe „Übungsblätter gelöst“ ergänzen, Tests laufen lassen.
```

---

## 17 · Anhang A: vollständige, getestete HTML-Vorlage

Die folgende Datei ist **komplett** und wurde in Chromium getestet: 0 KaTeX-Fehler, 0 JS-Fehler, kein Seitwärts-Scrollen bei 1920 px und 390 px, Tracer läuft vorwärts und rückwärts, eigene Eingabe funktioniert, Menü öffnet und schließt.

Sie enthält:
- Farb-Token,
- nicht-stickigen Kopf mit ☰-Menü und FAB,
- farbige Kernwörter,
- eine Von-null-Karte mit `aligned`-Kette,
- eine Anatomie-Karte mit 📖 Vorlesen,
- eine Minimal-Tracer-Engine mit Zeilenzählern samt Instanz (binäre Suche).

Als Startpunkt für jedes neue Fach kopieren.

```html
<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Teil 1 · Binäre Suche — Lernbegleiter</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Source+Sans+3:wght@400;600;700&family=JetBrains+Mono:wght@400;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.16.9/katex.min.css">
<script defer src="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.16.9/katex.min.js"></script>
<script defer src="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.16.9/contrib/auto-render.min.js"></script>
<style>
/* ---------- Farb-Token: Grundflächen (Bildatlas dunkel) + Rollenfarben ---------- */
:root{
  --bg:#0d1117; --card:#161b22; --card2:#1c2129; --border:#30363d; --text:#e6edf3; --muted:#8b949e;
  --res:#f85149;  /* Ergebnis / gesuchte Größe     rot    */
  --chg:#d29922;  /* Änderung / Zeiger / Zuweisung orange */
  --rule:#bc8cff; /* Regel / Fachwort / Invariante lila   */
  --par:#58a6ff;  /* Parameter / Eingabe           blau   */
  --idx:#3fb950;  /* Schleifenindex / Zähler / OK  grün   */
  --cond:#56d4dd; /* Bedingung / Vergleich         cyan   */
  --mem:#f778ba;  /* Speicher / Adresse / Zustand  pink   */
}
*{box-sizing:border-box}
html{scroll-behavior:smooth}
body{margin:0;background:var(--bg);color:var(--text);font-family:'Source Sans 3','Segoe UI',system-ui,sans-serif;font-size:17px;line-height:1.6}
.wrap{max-width:1640px;margin:0 auto;padding:1.2rem 1.6rem 5rem}
h2{font-size:1.5rem;margin:2.2rem 0 .6rem;border-bottom:1px solid var(--border);padding-bottom:.3rem}
code{font-family:'JetBrains Mono',Consolas,monospace;font-size:.88em;background:var(--card2);border:1px solid var(--border);border-radius:5px;padding:0 .3rem}
.card{background:var(--card);border:1px solid var(--border);border-radius:14px;padding:1rem 1.2rem;margin:1rem 0}
/* ---------- Farbige Schlüsselwörter im Fließtext (gleiche Rolle = gleiche Farbe wie in KaTeX) ---------- */
.kw{font-weight:600}
.kw-res{color:var(--res)} .kw-chg{color:var(--chg)} .kw-rule{color:var(--rule)} .kw-par{color:var(--par)}
.kw-idx{color:var(--idx)} .kw-cond{color:var(--cond)} .kw-mem{color:var(--mem)}
/* ---------- Kopf: NICHT sticky, Burger + Kurzlinks in einer umbrechenden Zeile ---------- */
.lbtop{display:flex;flex-wrap:wrap;align-items:center;gap:.5rem 1.1rem;padding:.8rem 1.4rem;border-bottom:1px solid var(--border);background:var(--card)}
.lbtop .lbt b{font-size:1.05rem;margin-right:.6rem}
.lbtop .lbt span{color:var(--muted);font-size:.85rem}
.lbq{display:flex;flex-wrap:wrap;gap:.3rem .45rem;flex:1 1 520px}
.lbq a{font-size:.8rem;text-decoration:none;color:var(--text);border:1px solid var(--border);border-radius:7px;padding:.12rem .5rem;white-space:nowrap}
.lbq a:hover{border-color:var(--rule);color:var(--rule)}
.lbq a small{color:var(--muted);margin-left:.3rem}
.lbburger,.lbfab{font:inherit;font-weight:700;cursor:pointer;border-radius:9px;border:1px solid var(--border);background:var(--card2);color:var(--text);padding:.3rem .75rem}
.lbfab{position:fixed;right:18px;bottom:18px;z-index:300;border-radius:50%;width:48px;height:48px;padding:0;box-shadow:0 4px 14px rgba(0,0,0,.35)}
.lbmenu{position:fixed;inset:0;z-index:400;background:rgba(0,0,0,.55);display:none;overflow:auto;padding:2.2rem 2rem}
body.menu-open .lbmenu{display:block}
.lbm-in{max-width:1640px;margin:0 auto;background:var(--card);border:1px solid var(--border);border-radius:14px;padding:1.1rem 1.4rem 1.4rem}
.lbm-head{display:flex;align-items:center;gap:1rem;margin-bottom:.6rem}
.lbm-head button{margin-left:auto}
.lbm-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(290px,1fr));gap:.9rem 1.4rem}
.lbm-grid h5{margin:.2rem 0 .3rem;font-size:.82rem;letter-spacing:.06em;text-transform:uppercase;color:var(--rule)}
.lbm-grid a{display:flex;gap:.5rem;align-items:baseline;text-decoration:none;color:var(--text);padding:.16rem .35rem;border-radius:6px;font-size:.9rem}
.lbm-grid a:hover{background:var(--card2)}
.lbm-grid a span{color:var(--muted);font-size:.8rem}
/* ---------- Anatomie-Karte ---------- */
.anat{display:grid;grid-template-columns:minmax(0,1fr) minmax(0,1fr);gap:.4rem 1.2rem}
.anat .sym{font-family:'JetBrains Mono',Consolas,monospace;font-weight:600}
.read{border-left:4px solid var(--rule);background:var(--card2);border-radius:8px;padding:.45rem .8rem;margin-top:.6rem}
/* ---------- Von-null-Karte ---------- */
.vonnull{border:1.6px solid var(--rule);border-radius:14px;background:#11161d;padding:1rem 1.2rem .8rem;margin:1.1rem 0 1.4rem}
.vonnull .vn-ttl{display:flex;flex-wrap:wrap;align-items:baseline;gap:.6rem;font-size:1.2rem;font-weight:700}
.vonnull .vn-badge{font-family:'JetBrains Mono',monospace;font-size:.74rem;color:#fff;background:var(--rule);border-radius:6px;padding:.16rem .55rem}
.vonnull .vn-task{border-left:4px solid var(--res);background:var(--card2);border-radius:8px;padding:.6rem .9rem;margin:.4rem 0 .6rem}
.vonnull summary{cursor:pointer;font-weight:700;color:var(--rule);padding:.35rem 0}
.vn-step{display:grid;grid-template-columns:2rem minmax(0,1fr);gap:.65rem;align-items:start;margin:.55rem 0}
.vnn{display:inline-flex;align-items:center;justify-content:center;width:1.75rem;height:1.75rem;border-radius:50%;background:var(--rule);color:#fff;font-weight:700;font-size:.85rem}
.vn-res{display:inline-block;margin:.5rem 0 .2rem;padding:.35rem .8rem;border-radius:8px;font-weight:700;border:1.5px solid var(--idx);color:var(--idx)}
/* ---------- Code-Tracer: links Code, rechts Zustand, nie seitwärts scrollen ---------- */
.tracer{background:var(--card);border:1px solid var(--border);border-radius:14px;padding:.9rem 1.1rem;margin:1rem 0}
.tr-head{display:flex;flex-wrap:wrap;gap:.6rem 1rem;align-items:center;margin-bottom:.6rem}
.tr-head input{font:inherit;font-family:'JetBrains Mono',monospace;background:var(--card2);color:var(--text);border:1px solid var(--border);border-radius:7px;padding:.2rem .5rem;width:min(26rem,100%)}
.tracer button{font:inherit;cursor:pointer;background:var(--card2);color:var(--text);border:1px solid var(--border);border-radius:8px;padding:.2rem .6rem}
.tr-grid{display:grid;grid-template-columns:minmax(0,1.15fr) minmax(0,1fr);gap:1rem}
.tr-code{margin:0;font-family:'JetBrains Mono',Consolas,monospace;font-size:.84rem;background:#0b0f14;border:1px solid var(--border);border-radius:10px;padding:.5rem 0}
.tr-code .row{white-space:pre-wrap;word-break:break-word;padding:0 .7rem 0 3.4em;text-indent:-2.7em}
.tr-code .row.cur{background:color-mix(in srgb,var(--chg) 22%,transparent);outline:1px solid var(--chg)}
.tr-code .ln{display:inline-block;width:2.2em;text-indent:0;color:var(--muted);margin-right:.5em;text-align:right}
.tr-code .hits{color:var(--idx);margin-left:.6em;font-size:.75rem}
.tr-state{background:var(--card2);border:1px solid var(--border);border-radius:10px;padding:.6rem .8rem}
.tr-ctl{display:flex;flex-wrap:wrap;gap:.4rem;align-items:center;margin-top:.6rem}
.tr-ctl input[type=range]{flex:1 1 200px}
.tr-cnt{color:var(--muted);font-size:.85rem}
.tr-msg{margin-top:.5rem;border-left:4px solid var(--chg);padding:.3rem .8rem;background:var(--card2);border-radius:6px}
.cells{display:flex;flex-wrap:wrap;gap:3px}
.cells .c{min-width:2.3rem;text-align:center;border:1px solid var(--border);border-radius:5px;padding:.1rem .25rem;font-family:'JetBrains Mono',monospace;font-weight:700}
.cells .c i{display:block;font-style:normal;font-size:.62rem;font-weight:400;color:var(--muted)}
.cells .c.m{border-color:var(--idx);background:color-mix(in srgb,var(--idx) 22%,transparent)}
.cells .c.out{opacity:.3}
@media(max-width:900px){.tr-grid,.anat{grid-template-columns:minmax(0,1fr)}}
</style>
</head>
<body>

<header class="lbtop">
  <button class="lbburger" id="lbOpen" aria-label="Inhalt öffnen">☰ Inhalt</button>
  <div class="lbt"><b>Teil 1 · Binäre Suche</b><span>Buch Abschnitt 2.6 · später ergänzt um Skript HM</span></div>
  <nav class="lbq">
    <a href="#s1">1 Idee<small>Bereich halbieren</small></a>
    <a href="#s2">2 Code<small>Tracer Zeile für Zeile</small></a>
  </nav>
</header>

<div class="lbmenu" id="lbMenu"><div class="lbm-in">
  <div class="lbm-head"><b>Inhalt</b><button class="lbburger" data-close>✕ schließen</button></div>
  <div class="lbm-grid">
    <div>
      <h5>Kapitel 2 · Analyse</h5>
      <a href="#s1"><b>2.6 Idee</b><span>jeder Vergleich halbiert den Suchbereich</span></a>
      <a href="#s2"><b>2.6 Code</b><span>Tracer, Kosten pro Zeile, O(log N)</span></a>
    </div>
  </div>
</div></div>
<button class="lbfab" id="lbFab" aria-label="Inhalt öffnen">☰</button>

<main class="wrap">

<section id="s1">
<h2>1 · Idee: Suchbereich halbieren</h2>

<div class="vonnull" id="vn-2-6">
  <div class="vn-ttl"><span class="vn-badge">Von null · Abschnitt 2.6</span>Wie oft vergleicht die binäre Suche höchstens?</div>
  <div class="vn-task"><b>Prüfungsaufgabe (für sich allein lösbar).</b> Ein sortiertes Feld hat \(\Par{N}=1000\) Einträge. Wie viele Vergleiche braucht die <span class="kw kw-rule">binäre Suche</span> im <span class="kw kw-res">schlechtesten Fall</span>?</div>
  <details><summary>Lösung von null, Schritt für Schritt</summary>
    <div class="vn-step"><span class="vnn">1</span><div><b class="h">Was passiert pro Vergleich?</b> Der Suchbereich mit <span class="kw kw-par">N</span> Einträgen schrumpft auf höchstens die Hälfte.</div></div>
    <div class="vn-step"><span class="vnn">2</span><div><b class="h">Wann ist Schluss?</b> Wenn der Bereich leer ist, also nach so vielen Halbierungen, dass weniger als ein Eintrag übrig bleibt.</div></div>
    <div class="vn-step"><span class="vnn">3</span><div><b class="h">Rechnen.</b> Die Kette unten zeigt jede Umformung in einer eigenen Zeile.</div></div>
    <div class="step">\[
      \begin{aligned}
      \Res{C_N} &= \Res{C_{\lfloor N/2 \rfloor}} + 1 && \text{ein Vergleich, dann die Hälfte weiter} \\
      \Res{C_1} &= 1 && \text{Rekursionsanker: ein Eintrag, ein Vergleich} \\
      \Res{C_N} &= \lfloor \log_2 \Par{N} \rfloor + 1 && \text{so oft kann man halbieren, plus der letzte Vergleich} \\
      \Res{C_{1000}} &= 9 + 1 = 10 && \text{denn } 2^9 = 512 \le 1000 < 1024 = 2^{10}
      \end{aligned}
    \]</div>
    <div class="vn-res">✔ höchstens 10 Vergleiche</div>
  </details>
</div>

<div class="card">
  <b>🔬 Anatomie: \(\Idx{m} = \lfloor (\Par{l} + \Par{r}) / 2 \rfloor\)</b>
  <div class="anat">
    <div><span class="sym kw-par">l, r</span>: linker und rechter Rand des noch möglichen Bereichs (Eingabe jeder Runde)</div>
    <div><span class="sym kw-idx">m</span>: Index der Mitte, an dem verglichen wird</div>
    <div><span class="sym kw-cond">v == a[m]</span>: die Bedingung, die über Treffer entscheidet</div>
    <div><span class="sym kw-chg">r = m − 1 / l = m + 1</span>: die Änderung, die den Bereich halbiert</div>
  </div>
  <div class="read">📖 Vorlesen: „m ist die abgerundete Mitte zwischen l und r; dort wird verglichen, danach wird eine Hälfte weggeworfen.“</div>
</div>
</section>

<section id="s2">
<h2>2 · Code Zeile für Zeile</h2>
<div id="trBin"></div>
</section>

</main>

<script>
/* ---------- Namensraum, KaTeX, Burger-Menü ---------- */
const LB = window.LB = {};
LB.KOPT = {                                                   // eine Konfiguration für alle KaTeX-Aufrufe
  delimiters: [{left: '\\[', right: '\\]', display: true}, {left: '\\(', right: '\\)', display: false}],
  throwOnError: false,                                        // Fehler rot anzeigen statt Seite abbrechen
  macros: {                                                   // Rollenfarben als Makros, identisch zu :root
    '\\Res': '\\textcolor{f85149}{#1}', '\\Chg': '\\textcolor{d29922}{#1}', '\\Rule': '\\textcolor{bc8cff}{#1}',
    '\\Par': '\\textcolor{58a6ff}{#1}', '\\Idx': '\\textcolor{3fb950}{#1}', '\\Cond': '\\textcolor{56d4dd}{#1}',
    '\\Mem': '\\textcolor{f778ba}{#1}'
  }
};
LB.tex = el => { if (window.renderMathInElement && el) window.renderMathInElement(el, LB.KOPT); };  // Formeln in el setzen
LB.esc = s => String(s).replace(/[&<>"]/g, c => ({'&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;'}[c]));  // HTML-sicher machen

LB.menu = open => document.body.classList.toggle('menu-open', open);             // Menü an/aus
document.getElementById('lbOpen').addEventListener('click', () => LB.menu(true));  // Kopf-Knopf öffnet
document.getElementById('lbFab').addEventListener('click', () => LB.menu(true));   // runder Knopf unten rechts öffnet
document.getElementById('lbMenu').addEventListener('click', e => {                 // schließen bei ✕, Link oder Klick daneben
  if (e.target.closest('[data-close]') || e.target.closest('a') || e.target.id === 'lbMenu') LB.menu(false);
});
document.addEventListener('keydown', e => { if (e.key === 'Escape') LB.menu(false); });  // Esc schließt

/* ---------- Code-Tracer: zeichnet Schritte auf und spielt sie ab ---------- */
LB.Tracer = class {
  constructor(o) {
    this.o = o;                                               // Optionen (id, title, input, parse, code, run, view)
    this.root = document.getElementById(o.id);                // Container im HTML
    this.root.classList.add('tracer');
    this.root.innerHTML =
      '<div class="tr-head"><b>' + LB.esc(o.title) + '</b>' +
      '<label>Eingabe <input class="tr-in" value="' + LB.esc(o.input) + '"></label>' +
      '<button data-a="run">▶ neu starten</button></div>' +
      '<div class="tr-grid"><pre class="tr-code"></pre><div class="tr-state"></div></div>' +
      '<div class="tr-ctl"><button data-a="first">⏮</button><button data-a="prev">◀</button>' +
      '<input type="range" class="tr-pos" min="0" value="0">' +
      '<button data-a="next">▶</button><button data-a="last">⏭</button>' +
      '<button data-a="auto">⏯ auto</button><span class="tr-cnt"></span></div>' +
      '<div class="tr-msg"></div>';
    this.q = s => this.root.querySelector(s);                 // Kurzform für Suche im eigenen Container
    this.q('.tr-code').innerHTML = o.code.map((l, i) =>       // jede Codezeile mit Nummer und Trefferzähler
      '<div class="row" data-l="' + (i + 1) + '"><span class="ln">' + (i + 1) + '</span>' +
      LB.esc(l) + '<span class="hits"></span></div>').join('');
    this.root.addEventListener('click', e => {                // alle Knöpfe über data-a
      const b = e.target.closest('[data-a]'); if (b) this.act(b.dataset.a);
    });
    this.q('.tr-pos').addEventListener('input', e => this.show(+e.target.value));  // Schieberegler springt
    this.q('.tr-in').addEventListener('keydown', e => { if (e.key === 'Enter') this.run(); });  // Enter startet neu
    this.run();
  }
  run() {
    const snaps = [];                                         // aufgezeichnete Schritte
    const rec = { step: (line, msg, st) => snaps.push({ line, msg, st: JSON.parse(JSON.stringify(st)) }) };  // Kopie des Zustands
    let inp;
    try { inp = this.o.parse(this.q('.tr-in').value); }      // Eingabe lesen
    catch (e) { snaps.push({ line: 0, msg: 'Eingabe nicht lesbar: ' + LB.esc(e.message), st: null }); }
    if (inp !== undefined) this.o.run(rec, inp);              // Algorithmus einmal komplett durchlaufen
    this.snaps = snaps.length ? snaps : [{ line: 0, msg: '(keine Schritte)', st: null }];
    this.q('.tr-pos').max = this.snaps.length - 1;
    if (this.timer) { clearInterval(this.timer); this.timer = null; }
    this.show(0);
  }
  act(a) {
    const n = this.snaps.length - 1;                          // Index des letzten Schritts
    if (a === 'run') this.run();
    else if (a === 'first') this.show(0);
    else if (a === 'prev') this.show(Math.max(0, this.i - 1));
    else if (a === 'next') this.show(Math.min(n, this.i + 1));
    else if (a === 'last') this.show(n);
    else if (a === 'auto') {                                  // Autoplay an/aus
      if (this.timer) { clearInterval(this.timer); this.timer = null; return; }
      this.timer = setInterval(() => {
        if (this.i >= n) { clearInterval(this.timer); this.timer = null; } else this.show(this.i + 1);
      }, 700);
    }
  }
  show(i) {
    this.i = i; const s = this.snaps[i];                      // aktueller Schritt
    this.q('.tr-pos').value = i;
    const hits = {};                                          // wie oft wurde jede Zeile bis hier ausgeführt?
    for (let k = 0; k <= i; k++) hits[this.snaps[k].line] = (hits[this.snaps[k].line] || 0) + 1;
    this.root.querySelectorAll('.tr-code .row').forEach(r => {
      const l = +r.dataset.l;
      r.classList.toggle('cur', l === s.line);                // aktuelle Zeile markieren
      r.querySelector('.hits').textContent = hits[l] ? '×' + hits[l] : '';
    });
    this.q('.tr-state').innerHTML = s.st ? this.o.view(s.st) : '';  // Zustand zeichnen
    this.q('.tr-msg').innerHTML = s.msg;                      // Erklärung dieses Schritts
    this.q('.tr-cnt').textContent = 'Schritt ' + i + ' / ' + (this.snaps.length - 1);
    LB.tex(this.q('.tr-msg'));                                // Formeln in der Erklärung setzen
  }
};

/* ---------- Tracer-Instanz: binäre Suche ---------- */
document.addEventListener('DOMContentLoaded', () => {
  LB.tex(document.body);                                      // alle statischen Formeln setzen
  new LB.Tracer({
    id: 'trBin',
    title: 'Binäre Suche (Buch Programm 2.2)',
    input: '10 13 17 21 28 33 40 52 ; 33',
    parse: s => {                                             // "Feld ; Schlüssel" zerlegen
      const [a, k] = s.split(';');
      if (k === undefined) throw new Error('Format: Zahlen ; Schlüssel');
      return { a: a.trim().split(/\s+/).map(Number).sort((x, y) => x - y), v: Number(k) };
    },
    code: [
      'int search(int a[], int v, int l, int r)',
      '{ while (r >= l)                     // solange der Bereich nicht leer ist',
      '    { int m = (l + r) / 2;           // Mitte des Bereichs (abgerundet)',
      '      if (v == a[m]) return m;       // Treffer: Position zurückgeben',
      '      if (v < a[m]) r = m - 1;       // Schlüssel kleiner: links weitersuchen',
      '      else l = m + 1;                // Schlüssel größer: rechts weitersuchen',
      '    }',
      '  return -1;                         // Bereich leer: nicht gefunden',
      '}'
    ],
    run(rec, { a, v }) {
      let l = 0, r = a.length - 1, m = -1;                    // Startbereich = ganzes Feld
      const st = () => ({ a, v, l, r, m });                   // Momentaufnahme für die Anzeige
      rec.step(1, 'Start: \\(\\Par{l}=' + l + '\\), \\(\\Par{r}=' + r + '\\), gesucht \\(\\Res{v}=' + v + '\\)', st());
      while (true) {
        const ok = r >= l;
        rec.step(2, 'Test \\(\\Cond{r \\ge l}\\): ' + r + ' ≥ ' + l + (ok ? ' → weiter' : ' → Bereich leer'), st());
        if (!ok) break;
        m = Math.floor((l + r) / 2);
        rec.step(3, '\\(\\Idx{m}=\\lfloor(' + l + '+' + r + ')/2\\rfloor=' + m + '\\)', st());
        if (v === a[m]) { rec.step(4, '\\(\\Cond{v = a[m]}\\): ' + v + ' = ' + a[m] + ' → \\(\\Res{\\text{Treffer bei } m=' + m + '}\\)', st()); return; }
        rec.step(4, '\\(\\Cond{v = a[m]}\\)? ' + v + ' ≠ ' + a[m], st());
        if (v < a[m]) { r = m - 1; rec.step(5, v + ' < ' + a[m] + ' → \\(\\Chg{r}=' + r + '\\)', st()); }
        else { l = m + 1; rec.step(6, v + ' > ' + a[m] + ' → \\(\\Chg{l}=' + l + '\\)', st()); }
      }
      rec.step(8, 'Rückgabe \\(\\Res{-1}\\): nicht gefunden', st());
    },
    view: ({ a, v, l, r, m }) =>
      '<div class="cells">' + a.map((x, k) =>
        '<span class="c' + (k === m ? ' m' : '') + (k < l || k > r ? ' out' : '') + '">' + x + '<i>' + k + '</i></span>').join('') +
      '</div><p><span class="kw kw-par">l</span> = ' + l + ' · <span class="kw kw-par">r</span> = ' + r +
      ' · <span class="kw kw-idx">m</span> = ' + m + ' · <span class="kw kw-res">v</span> = ' + v + '</p>'
  });
});
</script>
</body>
</html>
```

---

## 18 · Anhang B: Bausteine als HTML-Schnipsel

Alle Schnipsel setzen die CSS-Klassen der Vorlage aus Anhang A voraus.

### 18.1 Von-null-Karte
```html
<div class="vonnull" id="vn-6-3">
  <div class="vn-ttl"><span class="vn-badge">Von null · Abschnitt 6.3</span>Wie viele Vergleiche macht Insertion Sort auf 5 4 3 2 1?</div>
  <div class="vn-task"><b>Prüfungsaufgabe (für sich allein lösbar).</b> Gegeben ist das Feld \(\Mem{a}=[5,4,3,2,1]\) mit \(\Par{N}=5\). Zähle alle Schlüsselvergleiche von <span class="kw kw-rule">Insertion Sort</span>.</div>
  <details><summary>Lösung von null, Schritt für Schritt</summary>
    <div class="vn-step"><span class="vnn">1</span><div><b class="h">Was ist gesucht?</b> Die Anzahl der Vergleiche <span class="kw kw-res">C</span> im umgekehrt sortierten Feld, also im schlechtesten Fall.</div></div>
    <div class="vn-step"><span class="vnn">2</span><div><b class="h">Welche Regel greift?</b> Im Durchgang <span class="kw kw-idx">i</span> wandert das neue Element an allen <span class="kw kw-idx">i</span> Vorgängern vorbei.</div></div>
    <div class="vn-step"><span class="vnn">3</span><div><b class="h">Einsetzen.</b> Jede Summe steht in einer eigenen Zeile.</div></div>
    <div class="step">\[
      \begin{aligned}
      \Res{C} &= \sum_{\Idx{i}=1}^{\Par{N}-1} \Idx{i} && \text{Durchgang i vergleicht i-mal} \\
      &= 1 + 2 + 3 + 4 && \text{für N = 5 ausgeschrieben} \\
      &= 10 && \text{addiert} \\
      &= \tfrac{\Par{N}(\Par{N}-1)}{2} && \text{allgemeine Formel, hier 5 mal 4 durch 2}
      \end{aligned}
    \]</div>
    <div class="vn-res">✔ 10 Vergleiche, allgemein N(N−1)/2 im schlechtesten Fall</div>
  </details>
</div>
```

### 18.2 Anatomie-Karte
```html
<div class="card">
  <b>🔬 Anatomie: \(\Chg{a[j]} = \Chg{a[j-1]}\)</b>
  <div class="anat">
    <div><span class="sym kw-chg">a[j] = a[j-1]</span>: schiebt das größere Element eine Zelle nach rechts</div>
    <div><span class="sym kw-idx">j</span>: läuft von i abwärts, Position der Lücke</div>
    <div><span class="sym kw-cond">v &lt; a[j-1]</span>: solange der Vorgänger größer ist, weiter schieben</div>
    <div><span class="sym kw-mem">a</span>: das Feld im Speicher, wird an Ort und Stelle verändert</div>
  </div>
  <div class="read">📖 Vorlesen: „Solange der linke Nachbar größer ist, rückt er eine Zelle nach rechts, und die Lücke wandert nach links.“</div>
</div>
```

### 18.3 Übungsblatt-Lösungskarte
```html
<div class="vonnull" id="bl-quick-3" style="border-color:var(--par)">
  <div class="vn-ttl"><span class="vn-badge" style="background:var(--par)">Blatt Quicksort · Aufgabe 3</span>Vergleiche beim Partitionieren <span style="color:var(--chg)">★★</span></div>
  <div class="vn-task">Aufgabentext wörtlich aus dem Blatt; Abbildungen durch eigenes SVG ersetzt.</div>
  <details><summary>Lösung Schritt für Schritt</summary>
    <div class="vn-step"><span class="vnn">1</span><div><b class="h">Konvention.</b> Pivot ist das erste Element (wie in der Musterlösung).</div></div>
    <div class="vn-step"><span class="vnn">2</span><div><b class="h">Partition 1.</b> Feldzeile vor und nach dem Tausch, Pivot orange, Tauschpartner rot.</div></div>
    <div class="vn-res">✔ Ergebnis im Klausurstil</div>
    <span style="color:var(--idx);font-size:.85rem">✓ stimmt mit der Musterlösung des Blatts überein</span>
  </details>
</div>
```

### 18.4 Brücke mit „DU BIST HIER“ (SVG)
```html
<svg viewBox="0 0 760 120" style="max-width:100%;height:auto" role="img" aria-label="Brücke der Kapitel">
  <defs><marker id="arr" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#8b949e"/></marker></defs>
  <g font-family="Source Sans 3, sans-serif" font-size="14" fill="#e6edf3" text-anchor="middle">
    <rect x="10"  y="40" width="160" height="44" rx="10" fill="#1c2129" stroke="#30363d"/><text x="90"  y="67">Kap. 6 Elementar</text>
    <rect x="200" y="40" width="160" height="44" rx="10" fill="#1c2129" stroke="#bc8cff" stroke-width="2.5"/><text x="280" y="67">Kap. 7 Quicksort</text>
    <rect x="390" y="40" width="160" height="44" rx="10" fill="#1c2129" stroke="#30363d"/><text x="470" y="67">Kap. 8 Mergesort</text>
    <rect x="580" y="40" width="160" height="44" rx="10" fill="#1c2129" stroke="#30363d"/><text x="660" y="67">Kap. 9 Heaps</text>
    <line x1="170" y1="62" x2="198" y2="62" stroke="#8b949e" stroke-width="1.6" marker-end="url(#arr)"/>
    <line x1="360" y1="62" x2="388" y2="62" stroke="#8b949e" stroke-width="1.6" marker-end="url(#arr)"/>
    <line x1="550" y1="62" x2="578" y2="62" stroke="#8b949e" stroke-width="1.6" marker-end="url(#arr)"/>
    <text x="280" y="28" fill="#bc8cff" font-weight="700">▼ DU BIST HIER</text>
  </g>
</svg>
```

### 18.5 Glossar-Zeile
```html
<table>
  <tr><th>Begriff</th><th>englisch</th><th>intuitiv</th><th>wo</th></tr>
  <tr><td><span class="kw kw-rule">stabil</span></td><td>stable</td><td>gleiche Schlüssel behalten ihre Reihenfolge</td><td><a href="#s6-1">6.1</a></td></tr>
</table>
```

---

## 19 · Anhang C: Erfahrungen und Fallen aus dem Referenzprojekt

| Problem | Ursache | Lösung |
|---|---|---|
| Formeln in einer Zeile unleserlich | alles in eine Zeile gepackt | `aligned`, eine Aussage pro Zeile plus `\text{}` |
| Code mit `...` | Kürzen aus Platzgründen | nie kürzen, auf mehrere Karten verteilen |
| Text nur weiß | nur KaTeX war farbig | Wortliste Begriff→Rolle und automatisches Einfärben im Fließtext |
| Codefenster scrollt seitwärts | `white-space:pre` | `pre-wrap` plus hängender Einzug, `minmax(0,1fr)` in Grids |
| Riesige SVGs in Lösungen | feste `width` im SVG | `svg{width:auto!important;max-width:100%;height:auto}` |
| Farbwort-Skript färbt Badges/Knöpfe | fehlende Ausnahmen | SKIP-Selektorliste pflegen |
| KaTeX-Fehler bei `§` und „“ in `\text` | nicht unterstützte Zeichen | „Abschnitt“ schreiben, gerade Anführungszeichen weglassen |
| Regex findet Aufgabenkopf nicht | Sonderzeichen zwischen Wörtern im PDF-Text | tolerante Muster `.{0,4}?` |
| falsche Antwort aus Musterlösung gelesen | Regex traf „Balancefaktor der Wurzel“ statt „Wurzel“ | präzise Muster mit Lookbehind, Ergebnis stichprobenartig prüfen |
| Einfügeschritt löscht vorherige Blöcke | Aufräumen entfernt alle Blöcke einer Art | einmal mit der Gesamtliste aufrufen |
| Gesamtdatei: SecurityError | `file://` (Ursprung „null“) und Blob-iframe sind verschiedene Ursprünge | `postMessage` in beide Richtungen, eingefügtes Mini-Skript |
| Eingefügtes Skript kaputt | Escapes in HTML-Strings | DOM-API (`createElement`) statt HTML-Strings |
| Zusatzknopf verdeckt Inhalt | fester Knopf oben | entfernt; Teilwechsel als Zeile im vorhandenen ☰-Menü |
| Test zeigt 0 Formeln | CDN im Testbrowser nicht erreichbar | Bibliotheken lokal ausliefern (Request-Routing) |
| Scroll-Test misst falsch | `scroll-behavior:smooth` | `behavior:'instant'` im Test |
| Sticky Header nimmt Platz weg | Wunsch des Lernenden | Kopf scrollt weg, Menü über ☰ und FAB |

---

*Ende des Skills. Viel Erfolg bei der 1,0.*
