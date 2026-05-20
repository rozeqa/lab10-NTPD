# lab10-NTPD - Róża Domańska 122328

Laboratorium wykonałam w środowisku Google Colab z użyciem biblioteki PySpark.

<img width="308" height="111" alt="Zrzut ekranu 2026-05-20 o 14 12 47" src="https://github.com/user-attachments/assets/58998d21-b57f-4788-9571-74136b522eaf" />

## Zadanie 1
Przygotowałam przykładowe dane i zapisałam je do pliku Parquet.

<img width="457" height="206" alt="Zrzut ekranu 2026-05-20 o 14 14 47" src="https://github.com/user-attachments/assets/65ad7cee-7644-4948-ac50-341d2b141916" />

Podgląd danych:

**Wynik `show(10)`:**
```
=== Pierwsze wiersze ===
+---+------+-------+------+--------+
| id|region|product|amount|quantity|
+---+------+-------+------+--------+
|  1| North| Laptop| 500.0|       1|
|  2| South|  Phone| 513.5|       2|
|  3|  East| Tablet| 527.0|       3|
|  4|  West|Monitor| 540.5|       4|
|  5| North| Laptop| 554.0|       5|
|  6| South|  Phone| 567.5|       6|
|  7|  East| Tablet| 581.0|       7|
|  8|  West|Monitor| 594.5|       8|
|  9| North| Laptop| 608.0|       9|
| 10| South|  Phone| 621.5|      10|
+---+------+-------+------+--------+
only showing top 10 rows
```

**Wynik `printSchema()`:**
```
=== Schemat danych ===
root
 |-- id: long (nullable = true)
 |-- region: string (nullable = true)
 |-- product: string (nullable = true)
 |-- amount: double (nullable = true)
 |-- quantity: long (nullable = true)
```

## Zadanie 2 

Przygotowałam drugi zestaw danych i zapisałam go do pliku csv:

<img width="489" height="172" alt="Zrzut ekranu 2026-05-20 o 14 19 47" src="https://github.com/user-attachments/assets/6094e8d4-e2fb-4065-a54a-fcdd779d61e0" />

Podgląd danych:

**Wynik `show(10)`:**

```
=== Dane CSV ===
+-----------+-----------+------+---+
|customer_id|       name|region|age|
+-----------+-----------+------+---+
|          1| Customer_1| North| 20|
|          2| Customer_2| South| 21|
|          3| Customer_3|  East| 22|
|          4| Customer_4|  West| 23|
|          5| Customer_5| North| 24|
|          6| Customer_6| North| 25|
|          7| Customer_7| South| 26|
|          8| Customer_8|  East| 27|
|          9| Customer_9|  West| 28|
|         10|Customer_10| North| 29|
+-----------+-----------+------+---+
only showing top 10 rows
```

**Wynik `printSchema()`:**

```
root
 |-- customer_id: integer (nullable = true)
 |-- name: string (nullable = true)
 |-- region: string (nullable = true)
 |-- age: integer (nullable = true)
```

Oba DataFrame'y zostały zarejestrowane jako widoki tymczasowe, umożliwiając wykonywanie zapytań SQL:

<img width="489" height="40" alt="Zrzut ekranu 2026-05-20 o 14 22 42" src="https://github.com/user-attachments/assets/4c8262c3-f7bc-4020-878c-0a60f74d8efb" />
<img width="489" height="40" alt="Zrzut ekranu 2026-05-20 o 14 22 53" src="https://github.com/user-attachments/assets/c0cefbee-cdbb-4b21-aa1d-3b9693dc8766" />

## Zadanie 3

Zapytanie obliczające podstawowe statystyki sprzedaży pogrupowane według produktu, wynik:

<img width="562" height="135" alt="Zrzut ekranu 2026-05-20 o 14 25 05" src="https://github.com/user-attachments/assets/97dab023-6c93-4bd9-8d2d-baa0d20ad18a" />

Każdy produkt posiada po 25 transakcji. Najwyższy łączny przychód wygenerowały monitory (29 712,5), najniższy — laptopy (28 700,0).


Zapytanie grupujące dane jednocześnie po regionie i produkcie, wynik:

<img width="316" height="135" alt="Zrzut ekranu 2026-05-20 o 14 26 01" src="https://github.com/user-attachments/assets/5c168606-647f-49c6-ab5c-b7ff74595d73" />

Wyniki pokazują, że każdy region handluje wyłącznie jednym typem produktu — co wynika z zastosowanego cyklicznego generowania danych testowych.


Zapytanie zwracające wyłącznie transakcje o wartości powyżej 1000 w regionie North, posortowane malejąco, wynik:

<img width="362" height="294" alt="Zrzut ekranu 2026-05-20 o 14 27 15" src="https://github.com/user-attachments/assets/4f7c50e1-4bf0-4067-ab35-686abd965ffb" />

Filtrowanie z warunkiem `amount > 1000 AND region = 'North'` zwróciło 15 rekordów — wszystkie dotyczą produktu Laptop.


Złączenie widoku `sales` z widokiem `customers` po kolumnie `region`. Zapytanie zwraca transakcje powyżej 800 wraz z informacjami o powiązanych klientach z danego regionu, wynik:

<img width="391" height="306" alt="Zrzut ekranu 2026-05-20 o 14 28 06" src="https://github.com/user-attachments/assets/7a18ee17-8bf6-40a9-a84b-be3fa34f1ff8" />

JOIN po kolumnie `region` spowodował efekt mnożenia wierszy — każda transakcja łączy się z wszystkimi klientami z danego regionu, co jest zachowaniem poprawnym dla złączenia tego typu.


Wyniki zapytania agregującego zapisałam do pliku Parquet, a wyniki JOINu do pliku CSV:
<img width="536" height="137" alt="Zrzut ekranu 2026-05-20 o 14 29 14" src="https://github.com/user-attachments/assets/e09fe13a-b0c1-4e61-940a-cad6fb17b6ea" />

<img width="329" height="50" alt="Zrzut ekranu 2026-05-20 o 14 29 40" src="https://github.com/user-attachments/assets/276e95ee-9d8a-4583-9c50-448334982dae" />

## 6. Wnioski

- W ramach laboratorium zapoznałam się z podstawami Apache Spark SQL.
- Spark umożliwia wczytywanie danych z różnych formatów plików — Parquet jest formatem binarnym, zoptymalizowanym pod kątem wydajności, natomiast CSV jest bardziej uniwersalny, lecz wolniejszy przy dużych zbiorach.
- Rejestracja DataFrame jako widoku tymczasowego (`createOrReplaceTempView`) pozwala na wykonywanie standardowych zapytań SQL bez znajomości API PySpark.
- Spark SQL obsługuje pełen zakres operacji analitycznych: agregacje, grupowanie, filtrowanie oraz złączenia — składnia jest zgodna ze standardem SQL.
- Wyniki zapytań można zapisać z powrotem do pliku w dowolnym obsługiwanym formacie, co ułatwia integrację z kolejnymi etapami przetwarzania.

