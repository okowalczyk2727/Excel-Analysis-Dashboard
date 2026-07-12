# Miary DAX

Miary są obliczane dynamicznie w kontekście filtrów tabeli przestawnej, fragmentatorów i osi czasu.

> Nazwy tabel i kolumn odpowiadają modelowi użytemu w skoroszycie. W zależności od ustawień regionalnych separator argumentów może być przecinkiem albo średnikiem.

## Przychód zrealizowany

```DAX
Przychod zrealizowany :=
CALCULATE(
    SUM(FAKT_Sprzedaz[Przychod]),
    FILTER(
        FAKT_Sprzedaz,
        FAKT_Sprzedaz[Status] = "Zrealizowane"
            || FAKT_Sprzedaz[Status] = "Wysłane"
            || FAKT_Sprzedaz[Status] = "Zwrócone"
    )
)
```

## Liczba zamówień

```DAX
Liczba zamowien :=
CALCULATE(
    DISTINCTCOUNT(FAKT_Sprzedaz[Nr_Zamowienia]),
    FILTER(
        FAKT_Sprzedaz,
        FAKT_Sprzedaz[Status] = "Zrealizowane"
            || FAKT_Sprzedaz[Status] = "Wysłane"
            || FAKT_Sprzedaz[Status] = "Zwrócone"
    )
)
```

`DISTINCTCOUNT` jest potrzebny, ponieważ jedno zamówienie może zawierać kilka pozycji produktowych.

## Liczba sztuk

```DAX
Liczba sztuk :=
CALCULATE(
    SUM(FAKT_Sprzedaz[Ilosc]),
    FAKT_Sprzedaz[Status] <> "Anulowane"
)
```

## Średnia wartość zamówienia

```DAX
Srednia wartosc zamowienia :=
DIVIDE([Przychod zrealizowany], [Liczba zamowien], 0)
```

## Koszt sprzedanych produktów

```DAX
Koszt sprzedanych produktow :=
CALCULATE(
    SUMX(
        FAKT_Sprzedaz,
        FAKT_Sprzedaz[Ilosc] * RELATED(DIM_PRODUKTY[Cena_zakupu])
    ),
    FAKT_Sprzedaz[Status] <> "Anulowane"
)
```

## Marża i marża procentowa

```DAX
Marza :=
[Przychod zrealizowany] - [Koszt sprzedanych produktow]
```

```DAX
Marza % :=
DIVIDE([Marza], [Przychod zrealizowany], 0)
```

Marża procentowa jest liczona z zagregowanych wartości. Nie jest średnią procentów zapisanych w poszczególnych wierszach.

## Budżet i jego realizacja

```DAX
Budzet :=
SUM(FAKT_Budzet[Budzet])
```

```DAX
Odchylenie od budzetu :=
[Przychod zrealizowany] - [Budzet]
```

```DAX
Realizacja budzetu % :=
DIVIDE([Przychod zrealizowany], [Budzet], 0)
```

## Zwroty

```DAX
Liczba zgloszen :=
COUNTROWS(FAKT_Zwroty)
```

```DAX
Kwota zwrotow :=
CALCULATE(
    SUM(FAKT_Zwroty[Kwota_zwrotu]),
    FAKT_Zwroty[Typ] = "Zwrot"
)
```

```DAX
Przychod po zwrotach :=
[Przychod zrealizowany] - [Kwota zwrotow]
```

## Magazyn

```DAX
Stan magazynowy :=
SUM(FAKT_Magazyn[Stan])
```

```DAX
Pozycje do zamowienia :=
COUNTROWS(
    FILTER(
        FAKT_Magazyn,
        FAKT_Magazyn[Stan] <= FAKT_Magazyn[Punkt_zamowienia]
    )
)
```
