# Spielemagazine-Datenbank / Game magazine database

[English version below](#overview)

## Überblick
Diese kleine Datenbank katalogisiert die Inhalte von Zeitschriften die sich, ausschliesslich oder am Rande, mit Computer- und Videospielen befassen. Eingetragen werden sowohl die Inhalte der gedruckten Magazine als auch der beigelegten Disketten, CDs und DVDs. So können schnell alle Artikel und Dateien gefunden werden, die sich mit einem bestimmten Spiel oder einer bestimmten Applikation befassen. Auch Inhalte wie alte Gerätetreiber lassen sich so einfacher finden.

Die Datenbank enthält nur Metadaten. Die eigentlichen Artikel und Dateien müssen woanders aufgetrieben werden. Der Fokus liegt vorerst auf deutschen Zeitschriften, sollte aber irgendwann auch auf englische und U.S.-amerikanische ausgeweitet werden.

## Format
Die Datenbank wird derzeit im `.accdb`-Format für Microsoft Access geführt. Warum? Weil es mir hauptsächlich um möglichst schnelle und komfortable Dateneingabe geht. Access ist hier leider immer noch ungeschlagen. LibreOffice Base sieht vielversprechend aus, ich stosse aber noch auf zu viele Instabilitäten um damit produktiv arbeiten zu können.

Sobald die Datenbank eine anständige Grösse erreicht hat, werde ich sie auch in einem offenen Format zur Verfügung stellen. Access bietet glücklicherweise zahlreiche Wege, die Daten in andere Systeme zu exportieren.

## Lizenz
Vorerst stelle ich die Datenbank unter der Lizenz *CC BY-NC-ND 4.0* zur Verfügung. Dies kann sich in Zukunft noch ändern. Wichtigstes Detail dieser Lizenz: das Trainieren von LLMs oder ähnlichen Systemen auf diesen Daten ist verboten.

## Schema
Die Tabellen sind in drei Kategorien unterteilt:

| Kategorie         | Beschreibung |
|-------------------|--------------|
| Haupttabellen     | Erfassung der eigentlichen Inhalte: Artikel und Dateien, sowie die Zeitschriftenausgaben und Datenträger in oder auf welchen sie gefunden werden können; ebenfalls die Verlage und Magazinreihen. |
| Themen            | Die Spiele, Anwendungen und Personen, für die Suche nach Artikeln und Dateien die sich einem bestimmten Thema widmen. Andere Themen, wie etwa Unternehmen, könnten später ebenfalls noch in separaten Tabellen geführt werden. |
| Nachschlagelisten | Zur Kategorisierung der anderen Einträge, beispielsweise Spielegenres oder Betriebssysteme. |

### Haupttabellen für Verlage, Magazine, Ausgaben und Datenträger
In diesen Tabellen werden die eigentlichen Zeitschriften und Datenträger (Heft-CDs, usw.) verwaltet. Datenträger gehören immer zu einer bestimmten Zeitschriftenausgabe. Ausgaben gehören zu einer bestimmten Zeitschrift (Tabelle *Magazine*), welche wiederum zu einem Verlag gehören.

Das Feld *Vorgänger* in der Tabelle Magazine verweist auf Einträge in derselben Tabelle. Beispielsweise ist die Zeitschrift *ASM* die Vorgängerin der *PC Spiel*. In anderen Fällen (beispielsweise die verschiedenen Reihen der *Bestseller Games*) verweisen mehrere "Untermagazine" auf denselben Vorgänger.

Die Numerierung ("Nr.") der Ausgaben basiert nicht auf offiziellen Angaben sondern dem Versuch, sie strikt chronologisch anzuordnen, inklusive allfälliger Sonderhefte. Das Datum von Datenträgern wird, wann immer möglich, anhand der Informationen auf dem eigentlichen Datenträger eruiert. Bei CD-ROMs kann dieses zum Beispiel aus dem ISO 9660 Volume Descriptor stammen, bei Disketten vom Zeitstempel der zuletzt modifizierten Datei.

Die Datenbank enthält auch Angaben zu allfällig vorhandenen Scans und Imagedateien der Magazine und Datenträger. Oft sind Quellenangaben enthalten, die einen Hinweis darauf geben können, wo diese zu finden sind. Selbst stelle ich aber keine Dateien zur Verfügung.

### Haupttabellen für Artikel und Inhalte
Inhalte können verschiedenen Themen zugewiesen sein. Die Art, wie auf diese verwiesen wird, gefällt mir nicht ganz, ist für den Moment aber zweckdienlich.

Jeder Inhalt kann (muss aber nicht) einem *Inhaltstyp* zugewiesen werden. Das kann ein Spiel oder eine Anwendung sein, aber auch eine Person (z.B. Interviews), ein Entwicklungsstudio (z.B. Hintergrundberichte), ein Genre (z.B. Bestenlisten) und so weiter. In den Fällen *Spiel*, *Anwendung* oder *Person* stehen dabei zusätzlich Felder für Fremdschlüssel zur Verfügung, die auf einen entsprechenden Eintrag in den Thementabellen (siehe unten) verweisen. Es handelt sich um 1:n-Beziehungen – Artikel, die mehrere Themen behandeln, werden in der Artikeltabelle mehrfach erfasst. Dabei dienen die Felder *Heft*, *Seite* und *Titel* als Identifikation, dass es sich um denselben Artikel handelt. Die Anzahl *Seiten* und *Bilder* dieser Artikel wird anteilsmässig auf die einzelnen Themen aufgeteilt. Für Inhalte die nicht vom Typ *Spiel*, *Anwendung* oder *Person* sind dient das Freitextfeld *Thema* der Beschreibung des Themas.

Für (Datenträger-)*Inhalte* wird das erforderliche Betriebssystem aus den offiziellen Angaben entnommen. Gibt es keine Angabe, versuche ich meistens, anhand verschiedener Emulatoren und virtueller Maschinen herauszufinden, welche Betriebssystemversion effektiv die älteste ist, welche das Programm ausführen kann. Das Datum ergibt sich aus dem Änderungsdatum der zuletzt geänderten Datei, so, wie sie auf dem Datenträger abgespeichert wurde.

*Repack* bedeutet, dass die Datei auf dem Datenträger definitiv oder vermutlich vom Zeitschriftenverlag erstellt wurde, beispielsweise durch erneutes Packen der Dateien oder durch Hinzufügen eines eigenen Installationsprogramms. Das bedeutet, dass die Datei nicht von einer offiziellen Quelle (im Sinne des Herstellers des Spiels oder der Anwendung) stammt, und sich Dateiname, Dateigrösse und Prüfsumme vermutlich von anderen Kopien desselben Inhalts unterscheidet.

### Thementabellen
In diesen Tabellen werden die Spiele, Anwendungen und Personen verwaltet, auf die sich Artikel und Datenträgerinhalte beziehen können. So lassen sich Inhalte, die zum selben Spiel oder Programm gehören, leicht auffinden.

Bei der *Spiele*-Tabelle handelt es sich nicht um eine eigentliche Spieledatenbank, sondern lediglich um die Auflistung einiger Grunddaten, um Spiele eindeutig zu identifizieren. Es geht hauptsächlich darum auch Spiele unterscheiden zu können, die denselben Titel haben.

Die Genres und Spieltypen sind grob an mein eigentliches Spieledatenbankprojekt angelehnt, mit dem Unterschied, dass die Hierarchie nicht forciert wird und jedes Spiel nur einem Hauptgenre und -spieltyp zugewiesen werden kann. Es ist ein Kompromiss. Spiele können hier nicht so detailliert kategorisiert werden, wie in meiner anderen Datenbank. Trotzdem kann man beispielsweise alle Zeitschriftenartikel zu Rennspielen mit dem Spieltyp "Motorrad" zusammensuchen. Das Feld *DBID* verweist auf die ID des entsprechenden Spiels in meiner Spieledatenbank, welche derzeit aber noch nicht öffentlich zugänglich ist. Diese Datenbank beschreibt auch narrative Themen, Schauplätze, und vieles mehr.

### Nachschlagelisten

#### Anwendungsarten
Ähnlich den Genres für Spiele erlauben es die Anwendungsarten, Applikationen gemäss ihrem Zweck einzordnen. Beispiele sind etwa *Bildbearbeitung*, *Buchhaltung* oder *Nachschlagewerk*. Ein Sonderfall stellt das *Lernprogramm* dar, da es hier einen ziemlich fliessenden Übergang zu Lernspielen gibt. Die Einteilung, ob ein Programm eine Anwendung vom Typ *Lernprogramm* oder ein Spiel vom Typ *Lernspiel* ist, erfolgt nach Gefühl. Je weniger die Interaktivität an ein Spiel erinnert, das auch der Unterhaltung dient, desto grösser die Wahrscheinlichkeit, dass es in der Kategorie der Anwendungen landet.

#### Artikelarten

| Artikelart           | Beschreibung |
|----------------------|--------------|
| Editorial            | Beiträge aus der Redaktion, direkt an die Leserinnen und Leser gerichtet, ohne spezifischen Themenfokus. |
| Neuigkeit            | Aktuelle Informationen aus der Branche, inklusive Verkaufszahlen. |
| Kommentar            | Meinungsartikel. |
| Portrait             | Profile von Entwicklern und Entwicklerinnen, Studios, Verlagshäusern, usw. |
| Programmbeschreibung | Im Gegensatz zu kritischen Rezensionen; häufig für Programme, die sich auf den beigelegten Datenträgern befinden. |
| Anleitung            | Benutzeranleitungen für Programme, oder Abdrucke ganzer Handbücher. |
| Technik              | Tiefergehende Technikartikel, zum Beispiel zu Hardware, Programmierung, Grafikfeatures, usw. |
| Zuschriften          | Von Lesern beigesteuerte Inhalte, zum Beispiel Leserbriefe oder Kleinanzeigen. |
| Service              | Informationen vom Verlag, zum Beispiel das Impressum oder Angaben zur Abonnierung. |
| Humor                | Cartoons, Satire, usw. |
| Werbung              | Anzeigen halt… auch die sind inzwischen historisch oder nostalgisch interessant :) |

#### Betriebssysteme
Selbsterklärend, oder?

#### Inhaltstypen

| Inhaltstyp  | Beschreibung |
|-------------|--------------|
| Shareware   | Frei verteilbare Versionen von Spielen und Anwendungen, üblicherweise mit eingeschränktem Funktionsumfang. |
| Entwicklung | Inhalte für Entwickler, wie Beispielcode oder Bibliotheken. |

#### Lizenzen

| Lizenz    | Beschreibung |
|-----------|--------------|
| Shareware | Software, die weitergegeben werden darf. In Abgrenzung zur Freeware und gemeinfreien Software verwende ich den Begriff hier insbesondere für Software mit erwünschter oder verpflichtender, üblicherweise kostenpflichtiger Registrierung. |

#### Medien
Datenträgerformate (z.B. *5¼″ Floppy* oder *DVD-ROM DL*; "DL" steht hier für Dual-Layer).

#### Spielegenres und Spieltypen

| Genre     | Spieltyp      | Beschreibung |
|-----------|---------------|--------------|
| Action    |               | Actionspiele stellen die Reflexe und Geschicklichkeit von Spielerinnen und Spielern auf die Probe. Häufig stehen sie dabei auch unter Zeitdruck. Es geht um das flinke, geschickte und präzise Bedienen der Steuerung. Siehe auch: Shooter. |
|           | Ausweichen    | Actionspiele, in welchen hauptsächlich Hindernissen, Gegnern oder Ähnlichem ausgewichen werden muss, im Gegensatz zur direkten Konfrontation. |
|           | Ball & Paddel | Spiele wie Pong oder Breakout. |
|           | Flipper       | Simulationen von Flippertischen (Pinball). |
| Denkspiel |               | Neudeutsch auch Puzzles genannt. Denksportaufgaben, die durch logisches Überlegen gelöst werden müssen. |
|           | Mahjongg      | |
|           | Schieberätsel | Nicht die traditionellen Schiebepuzzles wie "15", sondern Denkspiele, in welchen Gegenstände verschoben werden müssen. Ein populäres Beispiel ist Sōkoban. |
| Lernspiel |               | Programme, die unterhaltsam und lehrreich zugleich sein möchten (vgl. Anwendungen vom Typ Lernprogramm). |
|           | Mathematik    | |
|           | Verkehr       | Verkehrstheorie oder Fahrschulsimulationen. |
| Shooter   |               | Um im Spiel erfolgreich zu sein, müssen Ziele mit Projektilwaffen getroffen werden. |
|           | Shoot 'em Up  | |

#### Thementypen

Art des Themas, mit welchem sich ein Artikel oder ein Datenträgerinhalt befasst.

- __Software:__ Spiel, Anwendung
- __Unternehmen:__ Studio, Verlag, Vertrieb
- __Person__
- __Genre__
- __Plattform__ (z.B. Spielekonsolen oder Betriebssysteme)
- __Hardware__

---

## Overview

The English translation will follow :)
