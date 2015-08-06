# MySQL crash course

- SQL-operatoren altijd in hoofdletters
- Elke operator op een nieuwe lijn en indents gebruiken
- Elk SQL-statement afsluiten met een ; tenzij het het laatste statement is
- Naming convention: underscore voor alle benamingen van databases, tabellen en kolommen

## Database aanmaken

Een nieuwe database aanmaken (analoog: een nieuw Excel-bestand aanmaken).

````
  CREATE DATABASE databasenaam
````

## Tabel met kolommen aanmaken

Een nieuwe tabel aanmaken (analoog: een nieuw datasheet in Excel-bestand aanmaken)

````
  CREATE TABLE tabelnaam
  (
    kolomnaam1 data_type( datatype1 ),
    [
    kolomnaam2 data_type( datatype2 ),
    ...
    ]
  )
````

## Datatypes van kolommen

(enkel die voor web-backend van toepassing zijn)
- INT: gebruikt voor getallen aan de duiden 
- VARCHAR( 1-256  ) : Korte strings die maximum een n-aantal karakters mogen zijn. Beperkt tot 256 karakters
- TEXT: Onbeperkte lengte van string
- BOOLEAN : TRUE of FALSE
- TIMESTAMP : Timestamp vanaf unix epoch

### Extra eigenschappen

- PRIMARY KEY : dit getal is het identificatienummer van een rij in een tabel
- FOREIGN KEY : verwijst naar een PRIMARY KEY kolom uit een andere tabel
- UNIQUE : deze waarde kan maar één keer in de tabel in die bepaalde voorkomen (vaak in samenwerking met PRIMARY KEY)
- AUTO_INCREMENT : bij het toevoegen van een nieuwe rij, wordt er automatisch ééntje bij het getal van de voorgaande rij opgeteld (vaak 
- NULL : duidt aan of de kolom automatisch ingevuld moet worden als met de waarde NULL (=leeg)
- UNSIGNED : duidt aan dat het getal niet negatief kan zijn (vaak in samenwerking met PRIMARY KEY). Bespaart 1 karakter.

## Data selecteren

````
SELECT kolomnaam1 [, kolomnaam2, ...]
  FROM tabelnaam
````

## Data toevoegen

Data aan een tabel toevoegen

1. Zonder specifieke kolommen te benoemen. Dit betekent dat je evenveel values moet toevoegen als er kolommen zijn.
````
INSERT INTO tabelnaam
  VALUES ( waarde1 [, waarde2, ... ] )
````

2. Data aan een specifieke kolom toevoegen
````
INSERT INTO tabelnaam ( kolomnaam1 [, kolomnaam2, ... ] )
  VALUES ( waarde1 [, waarde2, ... ] )
````
Opmerking: String moeten altijd tussen dubbele quotes! Voor getallen (int) is dit niet nodig. 

bv.
````
INSERT INTO interstellar ( acteur, personage )
  VALUES ( "Ellen Burstyn", "Murph" )
````

## Data updaten

Bestaande rij met nieuwe data updaten.

````
UPDATE tabelnaam 
  SET kolomnaam1 = value1 [, kolomnaam2 = value2, ... ]
````

## Data verwijderen

Rij uit tabel verwijderen

````
DELETE FROM tabelnaam 
  WHERE kolomnaam1  = 1
````
Opmerking: zie [WHERE](#where)


### Wildcard

Een manier om alle kolommen te selecteren (denk aan css-selector \*)

\* :  alle kolommen

````
SELECT *
  FROM tabelnaam
````

### WHERE

Data opvragen die voldoet aan een specifieke voorwaarde

````
SELECT *
  FROM tabelnaam
  WHERE kolomnaam1 operator value
````

bv.
````
SELECT *
  FROM interstellar
  WHERE is_human = TRUE
````

#### LIKE (=operator)

Zoekt naar bepaald string-patroon.
"%" is een wildcard en matcht alle karakters 0 of n keer

bv.
````
SELECT *
  FROM interstellar
  WHERE acteur LIKE "E%"
````

Zoekt naar alle acteurs waarvan de naam met een "E" begint

### LIMIT

Een aantal rijen selecteren op basis van het gevraagde LIMIT getal.

````
SELECT *
  FROM tabelnaam
  LIMIT getal
````

bv.
````
SELECT *
  FROM interstellar
  LIMIT 1
````

Opmerking: komt altijd op het einde van de SQL-query-statement.

### AND

Meerdere WHERE-clauses instellen

````
SELECT *
  FROM interstellar
  WHERE is_human = TRUE
  AND is_male = TRUE
````

### JOIN

Twee of meerdere tabellen samenvoegen op basis van een gemeenschappelijke kolom.

````
SELECT tabelnaam1.kolomnaam1, tabelnaam2.kolomnaam1
  FROM tabelnaam
  INNER JOIN tabelnaam2
    ON tabelnaam1.GebruikersId = tabelnaam2.GebruikersId
````
- Opmerking: best altijd de tabelnaam vermelden bij de kolomnaam om later conflicten te vermijden
- Opmerking 2: bij gelijknamige kolommen, een [alias](#alias) gebruiken

[![Meer over joins op Stack Overflow](http://i.stack.imgur.com/VQ5XP.png)](http://stackoverflow.com/questions/5706437/whats-the-difference-between-inner-join-left-join-right-join-and-full-join)



### ALIAS

Een manier om een kolom een andere naam te geven (belangrijk voor wanneer met (`INNER`/`LEFT`/`RIGHT`) `JOIN` wordt gewerkt)

````
SELECT kolomnaam1 AS aliasnaam1 [, kolomnaam2 AS aliasnaam2, ...]
  FROM tabelnaam
````

