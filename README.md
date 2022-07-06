# Arbeitsauftrag Indizes (Kota & Silvan)

## Was ist ein Index?

Ein Index beschleuinigt das Suchen und Sortieren nach bestimmten Felder in einer Datenbank.
Dieser besteht aus einer Ansammlung von Zeigern, welche eine sogenannte Ordnungsrelation definiert (vergleich von Elementen in einer Menge).

**Wie eine normale Abfrage funktionieren würde:**
Wir führen eine normale SELECT Abfrage durch:
```sql
select * from students where firstname = 'Peter'
```
Im Hintergrund wird nun jeder einzelner Eintrag verglichen, ob dieser der entsprechende Name hat.

**Wie die Abfrage mit Index funktioniert:**
Nun fügen wir einen Index hinzu (Code siehe bei der nächsten Frage). Im Hintergrund wird eine Art "Zuordnungs-Tabelle" erstellt.

Erkennt die Datenbank jetzt, dass in der WHERE Bedingung nur die Spalte `firstname` verwendet wird, wird er das Herraussuchen der Datensätze über diese Tabelle erledigen. Es wird also vor jedem Herraussuchen von der Datenbank abgewogen, welcher Index nun am besten passt und somit am schnellsten ausgeführt wird.

## Wie wird ein Index erstellt, gelöscht usw.?

```sql
select * from query
```

## Wie kann ein Index beurteilt werden?

Indizes ermöglichen einen Zugriff auf Daten in einer umfangreichen Datensammlung.
Die Datenbank muss somit nur nach Indizes suchen, welche oft chronologiesch angeordnet sind.
Sonst müsste die Datenbank den kompletten Datensatz abgleichen.

```sql
DECLARE @t1 DATETIME;
DECLARE @t2 DATETIME;
DECLARE @t3 INT;

SET @t1 = GETDATE();
SELECT * From Students
SET @t2 = GETDATE();
SET @t3 = DATEDIFF(millisecond,@t1,@t2);
Select @t3 as "Dauer in ms"
```

## Was bringt ein Index bei vielen Datensätzen?
Mithilfe des Indexes kann man grössere Mengen von Daten in kürzerer Zeit durchsuchen. Dazu haben wir mithilfe eines Scriptes eine DB mit Daten erstellt und zuerst ohne und dann mit Index die folgende Anfrage abgespielt:
```sql
select * from Exams where topic = 'Wanderdiktat'
```

```sql
CREATE INDEX index_name
ON table_name (column1, column2, ...);
```

Die Messungen bestätigen, dass der Index schneller ist.

## Was bringt ein Index bei vielen Tabellen bzw. Beziehungen zwischen den Tabellen?

Tabelle mit allen Students absteigend nach ihren Geburtsdatum
```sql
DECLARE @t1 DATETIME;
DECLARE @t2 DATETIME;
DECLARE @t3 INT;

SET @t1 = GETDATE();

SELECT ROUND(SUM(s.weight*e.weight*g.grade)/SUM(s.weight*e.weight), 1) FROM Grades g
JOIN Exams e ON g.fk_examId = e.id
JOIN Subjects s ON e.fk_subjectId = s.id
WHERE fk_studentId = 1

SET @t2 = GETDATE();
SET @t3 = DATEDIFF(millisecond,@t1,@t2);
Select @t3 as "Dauer in ms"
```
