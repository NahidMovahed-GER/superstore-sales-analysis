# Superstore Sales Analysis

# Was macht dieses Projekt?
# Phase 1: Datenanalyse mit Pandas

In diesem Projekt analysiere ich die Verkaufsdaten eines Superstores mit Python und Pandas.

Das Ziel ist herauszufinden:

- Wie entwickeln sich Umsatz und Gewinn?
- Welche Kategorien und Produkte sind profitabel?
- Welche Produkte verursachen Verluste?
- Welchen Einfluss haben Rabatte auf den Gewinn?
- Welche Regionen und Kundensegmente sind besonders erfolgreich?
- In welchen Monaten ist das Geschäft stark oder schwach?

## Welche Daten werden verwendet?

Der Datensatz enthält unter anderem:

- Bestelldatum
- Produktname und Produktkategorie
- Umsatz (`Sales`)
- Gewinn (`Profit`)
- Rabatt (`Discount`)
- verkaufte Menge (`Quantity`)
- Region
- Kundensegment

## Was habe ich gemacht?

Zuerst habe ich die CSV-Datei mit Pandas geladen und die Daten kontrolliert. Danach habe ich:

1. Umsatz und Gewinn nach Jahr analysiert.
2. Kategorien und Unterkategorien verglichen.
3. Gewinnmargen berechnet.
4. Produkte mit dem höchsten Umsatz und Gewinn gefunden.
5. Produkte und Unterkategorien mit Verlust identifiziert.
6. Den Zusammenhang zwischen Rabatt und Gewinn untersucht.
7. Regionen und Kundensegmente verglichen.
8. die monatliche Entwicklung von Umsatz und Gewinn analysiert.
9. Ergebnisse mit Tabellen und Diagrammen dargestellt.


# Phase 2: PostgreSQL-Integration

Nach der Datenanalyse habe ich den Datensatz in einer PostgreSQL-Datenbank gespeichert.

Dafür habe ich:

1. eine PostgreSQL-Datenbank namens `superstore_db` erstellt,
2. Python mithilfe von SQLAlchemy mit PostgreSQL verbunden,
3. den Pandas-DataFrame als Tabelle `superstore_sales` gespeichert,
4. die erfolgreiche Übertragung mit einer SQL-Abfrage kontrolliert.

Verwendete SQL-Abfrage:

```sql
SELECT *
FROM superstore_sales
LIMIT 10;
```

Diese Abfrage zeigt die ersten zehn Datensätze der Tabelle an.

Projektablauf:

`CSV → Pandas → SQLAlchemy → PostgreSQL → SQL Query`

## Verwendete Technologien

* Python
* Pandas
* Matplotlib
* PostgreSQL
* pgAdmin 4
* SQLAlchemy
* Jupyter Notebook

## Wichtigste Ergebnisse

- Umsatz und Gewinn sind insgesamt gestiegen.
- 2014 war das erfolgreichste Jahr.
- Technology ist die profitabelste Kategorie.
- Furniture erzielt viel Umsatz, aber nur wenig Gewinn.
- Tables und Bookcases verursachen Verluste.
- Rabatte über 20 % sind insgesamt nicht rentabel.
- West ist die profitabelste Region.
- Central hat die niedrigste Gewinnmarge.
- Consumer bringt den höchsten Gesamtgewinn.
- Home Office hat die höchste Gewinnmarge.
- November und Dezember sind besonders starke Monate.
- Juli 2011 und Januar 2012 waren die einzigen Verlustmonate.

## Fazit

Die Analyse zeigt, dass ein hoher Umsatz nicht automatisch einen hohen Gewinn bedeutet. Besonders hohe Rabatte sowie einzelne Produkte und Unterkategorien reduzieren die Rentabilität. Das Unternehmen sollte hohe Rabatte überprüfen und verlustreiche Produkte genauer untersuchen.

# Phase 3: First AI Data Agent Prototype

After analyzing the Superstore dataset with Pandas, storing the data in PostgreSQL, and creating a dashboard with Apache Superset, the next step was to introduce the first AI component.

The goal of this phase is to allow users to ask questions about the data in natural language without writing SQL manually.

### Architecture

```text
Natural Language Question
        ↓
      LLM
        ↓
Generated PostgreSQL Query
        ↓
SQL Safety Validation
        ↓
   PostgreSQL
        ↓
 Query Result
        ↓
      LLM
        ↓
Natural Language Answer
