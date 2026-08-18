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

---

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
```

---

### How it Works

1. **Natural Language Input:** The user asks a question in natural language.
   * *Example:* "Which region had the highest sales?"
2. **SQL Generation:** The question is sent to an OpenAI language model together with information about the available table and columns.
3. **Query Formulation:** The model generates a PostgreSQL `SELECT` query.
   * *Example:*
     ```sql
     SELECT "Region", SUM("Sales") AS total_sales
     FROM superstore_sales
     GROUP BY "Region"
     ORDER BY total_sales DESC
     LIMIT 1;
     ```
4. **Safety Validation:** Before execution, the generated SQL is validated by a safety check.
   * The current prototype **only allows read-only `SELECT` queries** and prevents generated SQL from modifying the database.
5. **Database Execution:** The validated query is executed against the existing PostgreSQL database.
6. **Answer Synthesis:** The database result is passed back to the language model and converted into a short, human-readable answer.
   * *Example Result:* "The West region achieved the highest sales with 725,457.82."

---

### Tests

The prototype was successfully tested with different natural-language questions, including:
* *Which category had the highest profit?*
* *Which region had the highest sales?*

For both questions, the model generated the appropriate SQL query, the query passed the safety validation, PostgreSQL returned the result, and the result was converted back into a natural-language answer.

---

### Current Status

This is intentionally a **small first prototype** rather than a complete autonomous AI agent.

The current focus is on building a controlled and understandable **Natural-Language-to-SQL workflow** before adding more advanced agent capabilities.

---

### Tech Stack

* **Python**
* **Pandas**
* **PostgreSQL**
* **SQLAlchemy**
* **OpenAI API**
* **Jupyter Notebook**
* **Docker**
* **Apache Superset**

---

### Next Steps

Possible next steps include:
* Improving SQL validation and security
* Handling a wider range of analytical questions
* Adding database schema awareness
* Improving error handling
* Separating the AI agent logic from the analysis notebook
* Evaluating when additional data engineering tools are actually needed

---

## 🔒 Wichtiger Sicherheitshinweis vor dem Upload

> **OpenAI API Key auf keinen Fall in GitHub hochladen!**  
> In deinem Notebook ist das bereits gut gelöst, weil du ihn mit `getpass()` eingibst und der eigentliche Key nicht im Code steht.

* **Benennung:** Den Begriff **„First AI Data Agent Prototype“** beizubehalten ist ideal. Das ist für GitHub professionell und technisch ehrlich, da es transparent kommuniziert, dass es sich um einen kontrollierten Prototypen und noch keinen vollautonom agierenden Agenten handelt.
* **Integration & Git Commit:** Im nächsten Schritt können wir festlegen, an welcher Stelle Phase 3 im bestehenden `README.md` eingefügt wird und welcher Commit-Name (z. B. `docs: add phase 3 AI data agent prototype section`) am besten dazu passt.
