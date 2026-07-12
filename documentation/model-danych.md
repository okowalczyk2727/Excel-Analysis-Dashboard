# Model danych i relacje

## Konstrukcja

Model został zbudowany w układzie gwiazdy. Tabele wymiarów zawierają unikalne wartości opisowe, natomiast tabele faktów przechowują zdarzenia i wartości liczbowe.

![Model danych](../screenshots/model-danych.png)

## Tabele faktów

| Tabela | Poziom szczegółowości | Przykładowe pola |
| --- | --- | --- |
| `FAKT_Sprzedaz` | jedna pozycja produktu w zamówieniu | numer zamówienia, data, produkt, klient, ilość, przychód, status |
| `FAKT_Budzet` | miesiąc × kategoria × kanał | budżet, cel marży, plan zamówień |
| `FAKT_Zwroty` | jedno zgłoszenie | data, typ, powód, ilość, kwota |
| `FAKT_Magazyn` | produkt × magazyn | stan, punkt zamówienia |

## Tabele wymiarów

| Tabela | Klucz | Zastosowanie |
| --- | --- | --- |
| `DIM_Data` | `Data` | rok, miesiąc i analiza w czasie |
| `DIM_PRODUKTY` | `Kod_produktu` | nazwa, kategoria, marka i cena zakupu |
| `DIM_KLIENCI` | `ID_Klient` | klient, miasto i dane kontaktowe |
| `DIM_Kategoria` | `Kategoria` | wspólny filtr sprzedaży i budżetu |
| `DIM_Kanal` | `Kanal_Sprzedazy` | wspólny filtr sprzedaży i budżetu |

## Relacje

| Strona `1` | Strona `*` |
| --- | --- |
| `DIM_Data[Data]` | `FAKT_Sprzedaz[Data_zamowienia]` |
| `DIM_Data[Data]` | `FAKT_Zwroty[Data_zgloszenia]` |
| `DIM_Data[Data]` | `FAKT_Budzet[Miesiac]` |
| `DIM_PRODUKTY[Kod_produktu]` | `FAKT_Sprzedaz[Kod_produktu]` |
| `DIM_PRODUKTY[Kod_produktu]` | `FAKT_Zwroty[Kod_produktu]` |
| `DIM_PRODUKTY[Kod_produktu]` | `FAKT_Magazyn[Kod_produktu]` |
| `DIM_KLIENCI[ID_Klient]` | `FAKT_Sprzedaz[ID_Klient]` |
| `DIM_Kategoria[Kategoria]` | `FAKT_Sprzedaz[Kategoria]` |
| `DIM_Kategoria[Kategoria]` | `FAKT_Budzet[Kategoria]` |
| `DIM_Kanal[Kanal_Sprzedazy]` | `FAKT_Sprzedaz[Kanal_Sprzedazy]` |
| `DIM_Kanal[Kanal_Sprzedazy]` | `FAKT_Budzet[Kanal_sprzedazy]` |

## Dlaczego osobne wymiary kategorii i kanału

Kategoria i kanał występują zarówno w sprzedaży, jak i w budżecie. Bez osobnych wymiarów fragmentator utworzony z jednej tabeli faktów nie filtrowałby drugiej. `DIM_Kategoria` i `DIM_Kanal` zapewniają jedną wspólną listę wartości dla obu obszarów.

## Zasady modelu

- klucz po stronie `DIM` jest unikalny,
- typ danych po obu stronach relacji jest taki sam,
- tabele faktów nie są łączone bezpośrednio między sobą,
- pola używane na osiach i fragmentatorach pochodzą z tabel wymiarów,
- `DIM_Data` jest oznaczona jako tabela dat.
