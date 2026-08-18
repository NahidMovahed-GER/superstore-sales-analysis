# Superstore Sales Analysis

## Was macht dieses Projekt?

In diesem Projekt analysiere ich die Verkaufsdaten eines Superstores und
entwickle die Datenverarbeitung schrittweise weiter -- von der
klassischen Datenanalyse über PostgreSQL und Reporting bis zu einem
ersten Prototyp eines AI Data Agents.

Der bisherige Projektablauf:

``` text
CSV
 ↓
Pandas
 ↓
PostgreSQL
 ├──→ Apache Superset → Dashboard
 ↓
LLM → SQL-Sicherheitsprüfung → PostgreSQL → verständliche Antwort
```

## Phase 1: Datenanalyse mit Pandas

In der ersten Phase analysiere ich die Verkaufsdaten mit Python und
Pandas.

Das Ziel ist herauszufinden:

-   Wie entwickeln sich Umsatz und Gewinn?
-   Welche Kategorien und Produkte sind profitabel?
-   Welche Produkte verursachen Verluste?
-   Welchen Einfluss haben Rabatte auf den Gewinn?
-   Welche Regionen und Kundensegmente sind besonders erfolgreich?
-   In welchen Monaten ist das Geschäft stark oder schwach?

### Welche Daten werden verwendet?

Der Datensatz enthält unter anderem:

-   Bestelldatum
-   Produktname und Produktkategorie
-   Umsatz (`Sales`)
-   Gewinn (`Profit`)
-   Rabatt (`Discount`)
-   verkaufte Menge (`Quantity`)
-   Region
-   Kundensegment

### Was habe ich gemacht?

Zuerst habe ich die CSV-Datei mit Pandas geladen und die Daten
kontrolliert. Danach habe ich:

1.  Umsatz und Gewinn nach Jahr analysiert.
2.  Kategorien und Unterkategorien verglichen.
3.  Gewinnmargen berechnet.
4.  Produkte mit dem höchsten Umsatz und Gewinn gefunden.
5.  Produkte und Unterkategorien mit Verlust identifiziert.
6.  den Zusammenhang zwischen Rabatt und Gewinn untersucht.
7.  Regionen und Kundensegmente verglichen.
8.  die monatliche Entwicklung von Umsatz und Gewinn analysiert.
9.  Ergebnisse mit Tabellen und Diagrammen dargestellt.

### Wichtigste Ergebnisse

-   Umsatz und Gewinn sind insgesamt gestiegen.
-   2014 war das erfolgreichste Jahr.
-   Technology ist die profitabelste Kategorie.
-   Furniture erzielt viel Umsatz, aber nur wenig Gewinn.
-   Tables und Bookcases verursachen Verluste.
-   Rabatte über 20 % sind insgesamt nicht rentabel.
-   West ist die profitabelste Region.
-   Central hat die niedrigste Gewinnmarge.
-   Consumer bringt den höchsten Gesamtgewinn.
-   Home Office hat die höchste Gewinnmarge.
-   November und Dezember sind besonders starke Monate.
-   Juli 2011 und Januar 2012 waren die einzigen Verlustmonate.

### Fazit der Datenanalyse

Die Analyse zeigt, dass ein hoher Umsatz nicht automatisch einen hohen
Gewinn bedeutet. Besonders hohe Rabatte sowie einzelne Produkte und
Unterkategorien reduzieren die Rentabilität. Das Unternehmen sollte hohe
Rabatte überprüfen und verlustreiche Produkte genauer untersuchen.

------------------------------------------------------------------------

## Phase 2: PostgreSQL-Integration

Nach der Datenanalyse habe ich den Datensatz in einer
PostgreSQL-Datenbank gespeichert.

Dafür habe ich:

1.  eine PostgreSQL-Datenbank namens `superstore_db` erstellt,
2.  Python mithilfe von SQLAlchemy mit PostgreSQL verbunden,
3.  den Pandas-DataFrame als Tabelle `superstore_sales` gespeichert,
4.  die erfolgreiche Übertragung mit einer SQL-Abfrage kontrolliert.

Verwendete SQL-Abfrage:

``` sql
SELECT *
FROM superstore_sales
LIMIT 10;
```

Diese Abfrage zeigt die ersten zehn Datensätze der Tabelle an.

### Projektablauf

``` text
CSV → Pandas → SQLAlchemy → PostgreSQL → SQL-Abfrage
```

------------------------------------------------------------------------

## Phase 3: Reporting mit Apache Superset

Nach der Speicherung der analysierten Daten in PostgreSQL habe ich die
Datenbank mit Apache Superset verbunden.

Ziel dieser Phase war es, auf Basis der bestehenden PostgreSQL-Daten ein
einfaches BI- und Reporting-Dashboard aufzubauen, ohne zusätzliche
Visualisierungen mit Python programmieren zu müssen.

Ich habe ein **Superstore Sales Dashboard** mit drei Visualisierungen
erstellt:

-   Sales by Category
-   Sales over Time
-   Profit by Category

### Architektur

``` text
PostgreSQL
    ↓
Apache Superset
    ↓
Superstore Sales Dashboard
```

Das Dashboard ermöglicht einen schnellen Überblick über Umsatz und
Gewinn nach Kategorien sowie über die zeitliche Umsatzentwicklung.

------------------------------------------------------------------------

## Phase 4: Erster AI-Data-Agent-Prototyp

Nach Datenanalyse, Datenbankintegration und Reporting habe ich den
ersten KI-Baustein in das Projekt integriert.

Das Ziel dieser Phase ist, Fragen zu den vorhandenen Verkaufsdaten in
natürlicher Sprache stellen zu können, ohne die benötigte SQL-Abfrage
selbst schreiben zu müssen.

### Architektur

``` text
Frage in natürlicher Sprache
        ↓
       LLM
        ↓
Generierte PostgreSQL-Abfrage
        ↓
SQL-Sicherheitsprüfung
        ↓
   PostgreSQL
        ↓
 Abfrageergebnis
        ↓
       LLM
        ↓
Verständliche Antwort
```

### Wie funktioniert der Prototyp?

1.  Der Benutzer stellt eine Frage in natürlicher Sprache.

    Beispiel:

    > Welche Region hatte den höchsten Umsatz?

2.  Die Frage wird zusammen mit Informationen über die vorhandene
    Tabelle und relevante Spalten an ein OpenAI-Sprachmodell übergeben.

3.  Das Modell erzeugt daraus eine passende PostgreSQL-`SELECT`-Abfrage.

    Beispiel:

    ``` sql
    SELECT "Region", SUM("Sales") AS total_sales
    FROM superstore_sales
    GROUP BY "Region"
    ORDER BY total_sales DESC
    LIMIT 1;
    ```

4.  Vor der Ausführung wird die generierte SQL-Abfrage durch eine
    Sicherheitsprüfung kontrolliert.

    Der aktuelle Prototyp erlaubt ausschließlich lesende
    `SELECT`-Abfragen und soll verhindern, dass generiertes SQL Daten in
    der Datenbank verändert.

5.  Die geprüfte Abfrage wird gegen die vorhandene PostgreSQL-Datenbank
    ausgeführt.

6.  Das Datenbankergebnis wird anschließend wieder an das Sprachmodell
    übergeben und in eine kurze, verständliche Antwort umgewandelt.

    Beispiel:

    > Die Region West erzielte mit 725.457,82 den höchsten Umsatz.

### Tests

Der Prototyp wurde bisher mit verschiedenen natürlichsprachlichen Fragen
getestet, darunter:

-   Welche Kategorie hatte den höchsten Gewinn?
-   Welche Region hatte den höchsten Umsatz?

Bei beiden Tests wurde der vollständige Ablauf erfolgreich durchgeführt:

``` text
Frage
 ↓
SQL-Generierung
 ↓
Sicherheitsprüfung
 ↓
PostgreSQL-Abfrage
 ↓
Datenbankergebnis
 ↓
Antwort in natürlicher Sprache
```

### Aktueller Stand

Der aktuelle Stand ist bewusst ein kleiner erster Prototyp und noch kein
vollständiger autonomer AI Data Agent.

Der Schwerpunkt liegt zunächst auf einem kontrollierten und
nachvollziehbaren **Natural-Language-to-SQL-Workflow**. Weitere
Agentenfunktionen sollen schrittweise ergänzt werden, wenn sie für den
konkreten Anwendungsfall sinnvoll sind.

### Nächste Schritte

Mögliche nächste Entwicklungsschritte sind:

-   SQL-Validierung und Sicherheit weiter verbessern
-   mehr Arten analytischer Fragen unterstützen
-   Datenbankschema automatisch berücksichtigen
-   Fehlerbehandlung verbessern
-   AI-Agent-Logik aus dem Analyse-Notebook in eigene Komponenten
    auslagern
-   weitere Agentenfunktionen schrittweise ergänzen
-   zusätzliche Data-Engineering-Werkzeuge erst einsetzen, wenn der
    konkrete Anwendungsfall sie benötigt

------------------------------------------------------------------------

## Verwendete Technologien

-   Python
-   Pandas
-   Matplotlib
-   PostgreSQL
-   pgAdmin 4
-   SQLAlchemy
-   Jupyter Notebook
-   Docker
-   Apache Superset
-   OpenAI API

## Projektstatus

Das Projekt wird schrittweise zu einem AI Data Agent weiterentwickelt.
Der aktuelle Stand umfasst Datenanalyse, persistente Speicherung in
PostgreSQL, BI-Reporting mit Apache Superset sowie einen ersten
kontrollierten Natural-Language-to-SQL-Prototyp.
