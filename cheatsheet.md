# ============================================
# BASH/DUCKDB CHEATSHEET 
# ============================================
## NAVIGATION
cd ..                     # Upp en nivå
cd ../..                  # Upp två nivåer
cd ~                      # Hem-directory
cd -                      # Toggle förra/nuvarande directory
pwd                       # Visa nuvarande path

# Shortcuts (från ~/.bashrc):
sqlc                      # → SQL course root
sqldb                     # → db folder
sqlq                      # → sql folder
sqldata                   # → data folder

---

## DUCKDB BASICS

### Skapa/öppna databas (CLI)
cd db
duckdb lecture_03.db      # Skapa/öppna databas
.exit                     # Avsluta DuckDB CLI

### Öppna databas med UI
cd ..                     # Tillbaka till root
duckdb db/lecture_03.db -ui

# Eller med shortcut:
ddb-open lecture_03       # Öppnar db/lecture_03.db i UI

---

## DUCKDB CLI COMMANDS

### Visa tabeller och schema
SHOW TABLES;              # Lista alla tabeller
DESCRIBE table_name;      # Visa schema för tabell
.schema table_name        # Alternativ syntax

### Ladda CSV
CREATE TABLE my_table AS 
SELECT * FROM read_csv_auto('path/to/file.csv');

-- Med options:
CREATE TABLE my_table AS 
SELECT * FROM read_csv_auto('file.csv', 
    header=true, 
    delimiter=',',
    ignore_errors=true
);

### Export till CSV
COPY (SELECT * FROM my_table) 
TO 'output.csv' (HEADER, DELIMITER ',');

### Avsluta CLI
.exit
-- eller
quit;

---

## BASH SHORTCUTS (från ~/.bashrc)

### duckdb för att öppna upp ddb ui
duckdb db/databas_namn.duckdb -ui

### Git
gs                        # git status
ga .                      # git add .
gc "message"              # git commit -m "message"
gp                        # git push
gl                        # git log (pretty)

### Utility
ll                        # ls -la (lista alla filer)
..                        # cd ..
...                       # cd ../..
clear                     # Rensa terminal

---

## SQL QUICK REFERENCE

### SELECT Basics
SELECT * FROM table_name LIMIT 5;
SELECT column1, column2 FROM table_name WHERE condition;
SELECT * FROM table_name ORDER BY column DESC;

### Aggregations
SELECT category, COUNT(*) as count
FROM products
GROUP BY category
HAVING count > 5
ORDER BY count DESC;

### CRUD Operations
-- INSERT
INSERT INTO table_name (col1, col2) VALUES ('val1', 'val2');

-- UPDATE
UPDATE table_name SET col1 = 'new_value' WHERE condition;

-- DELETE
DELETE FROM table_name WHERE condition;

---

## COMMON WORKFLOWS

### Ny lektion
mkdir -p sql/lecture_XX
touch sql/lecture_XX/notes.sql
duckdb db/lecture_XX.db
# (Ladda data, kör queries)
.exit
ddb-open lecture_XX

### Fortsätt på befintlig databas
ddb-open lecture_XX       # Öppnar UI
# eller
duckdb db/lecture_XX.db   # Öppnar CLI

### Kör SQL-script från fil
duckdb db/lecture_XX.db < sql/lecture_XX/queries.sql

---

## TROUBLESHOOTING

### "File not found"
pwd                       # Kolla var du är
ls                        # Lista filer
cd sqlc                   # Gå till project root

### CSV parsing error
-- Tillåt errors:
CREATE TABLE test AS 
SELECT * FROM read_csv_auto('file.csv', ignore_errors=true);

-- Explicit delimiter:
CREATE TABLE test AS 
SELECT * FROM read_csv_auto('file.csv', delimiter=';');

### UI öppnas inte
# Kolla om port upptagen:
lsof -i :8080

# Använd annan port:
duckdb db/lecture_03.db -ui -port 8081

---

## TIPS & TRICKS

### Autocomplete (Tab)
cd sq[TAB]                # Autocomplete till sql_course/
cd d[TAB]                 # Autocomplete till db/

### Command history (Arrow keys)
↑                         # Tidigare kommando
↓                         # Nästa kommando

### Reload bashrc
source ~/.bashrc          # Efter ändringar i .bashrc

---


### SQL Analytics 
duckdb db/space_bridge.db
CREATE TABLE counts AS 
SELECT * FROM read_csv_auto('../data/planet_counts.csv');

---

Last updated: 2025-11-14