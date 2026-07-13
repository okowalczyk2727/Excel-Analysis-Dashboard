# Analiza sprzedaży i budżetu w Excelu

![Dashboard projektu](screenshots/dashboard.png)

Projekt przedstawia pełny proces pracy z danymi sprzedażowymi w Excelu: od surowego, celowo zabrudzonego pliku, przez transformacje Power Query i model Power Pivot, po miary DAX, tabele przestawne oraz interaktywny dashboard.

Dane są syntetyczne, ale zostały przygotowane tak, aby przypominały materiały spotykane w pracy analityka: niespójne nazwy, różne formaty liczb i dat, braki danych, błędne kody produktów oraz rekordy wymagające kontroli biznesowej.

## Co zrobiłem w projekcie

- oczyściłem i ujednoliciłem dane w Power Query,
- przygotowałem tabele faktów i wymiarów,
- zbudowałem model danych w układzie gwiazdy,
- połączyłem sprzedaż z klientami, produktami, kalendarzem, kategoriami i kanałami,
- utworzyłem miary DAX dla sprzedaży, marży, budżetu i zwrotów,
- przygotowałem osobne analizy czasu, kategorii, kanałów, zwrotów i magazynu,
- zbudowałem dashboard sterowany fragmentatorami i osią czasu,
- zachowałem arkusze kontroli jakości, aby problemy źródłowe były widoczne i możliwe do audytu.

## Najważniejsze wyniki

Wartości dla pełnego zakresu danych z 2025 roku zapisanych w skoroszycie:

| Wskaźnik | Wynik |
| --- | ---: |
| Przychód zrealizowany | 3 536 247,41 zł |
| Przychód po zwrotach | 3 449 254,06 zł |
| Marża | 1 332 828,26 zł |
| Marża procentowa | 37,69% |
| Liczba zrealizowanych zamówień | 1 313 |
| Średnia wartość zamówienia | 2 693,26 zł |
| Budżet sprzedaży | 5 784 080,94 zł |
| Realizacja budżetu | 61,14% |
| Odchylenie od budżetu | −2 247 833,53 zł |
| Kwota zwrotów uwzględniona w KPI | 86 993,35 zł |

## Krótkie wnioski

- Sprzedaż osiągnęła **61,14% budżetu**, co oznacza lukę w wysokości **2,25 mln zł**. Przy marży procentowej **37,69%** główne odchylenie od planu dotyczy poziomu przychodów.
- Największy przychód wygenerowała kategoria **Dom i kuchnia**: **806,7 tys. zł**. Kategoria ta miała również najwyższą marżę procentową: **39,41%**.
- Najlepszą realizację budżetu wśród kategorii osiągnęły **Sport i turystyka** (**78,43%**) oraz **Biuro** (**76,73%**). Największa luka wystąpiła w **Elektronice**, która zrealizowała **40,38%** planu i była około **1,09 mln zł** poniżej budżetu.
- **Sklep online** był największym kanałem sprzedaży: **1,46 mln zł** przychodu i **642 zamówienia**, ale zrealizował tylko **52,98% budżetu**.
- **Telefon/B2B** jako jedyny kanał przekroczył plan, osiągając **213,14% budżetu**. Kanał odpowiadał za **858,3 tys. zł** przychodu przy zaledwie **119 zamówieniach**, a średnia wartość zamówienia wyniosła **7 212,23 zł**.
- Najwyższy miesięczny przychód odnotowano we **wrześniu** (**353,7 tys. zł**), natomiast najsłabszą realizację budżetu w **grudniu** (**37,19%**). Wysokie plany na listopad i grudzień nie znalazły pokrycia w sprzedaży.
- W danych znajduje się **177 zgłoszeń** o łącznej wartości **123,6 tys. zł**. Najczęstszymi powodami były zmiana decyzji klienta oraz wadliwy produkt. Kwota zwrotów uwzględniona w głównych KPI stanowiła około **2,46% przychodu**.

Szczegółowe omówienie znajduje się w pliku [`documentation/wnioski-biznesowe.md`](documentation/wnioski-biznesowe.md).

## Model danych

Model składa się z czterech tabel faktów oraz wspólnych wymiarów. Relacje mają kardynalność jeden-do-wielu, a filtrowanie przebiega od tabel `DIM` do tabel `FAKT`.

![Model danych Power Pivot](screenshots/model-danych.png)

Główne tabele:

- `FAKT_Sprzedaz` - pozycje sprzedażowe,
- `FAKT_Budzet` - miesięczne plany według kategorii i kanału,
- `FAKT_Zwroty` - zwroty i reklamacje,
- `FAKT_Magazyn` - stany produktów w magazynach,
- `DIM_Data`, `DIM_PRODUKTY`, `DIM_KLIENCI`, `DIM_Kategoria`, `DIM_Kanal` — tabele opisowe i filtrujące.

Szczegóły relacji: [`documentation/model-danych.md`](documentation/model-danych.md).

## Dashboard i analizy

Dashboard zawiera:

- KPI sprzedażowe i budżetowe,
- sprzedaż oraz budżet w kolejnych miesiącach,
- porównanie kategorii,
- wyniki kanałów sprzedaży,
- analizę powodów zwrotów i reklamacji,
- fragmentatory kategorii i kanału oraz oś czasu.

Poniżej znajdują się dodatkowe widoki tabel analitycznych zapisanych w skoroszycie.

### Analiza czasu

![Analiza sprzedaży w czasie](screenshots/analiza-czas.png)

### Analiza kategorii

![Analiza kategorii](screenshots/analiza-kategorie.png)

### Analiza kanałów

![Analiza kanałów](screenshots/analiza-kanaly.png)

Pełny zestaw podglądów znajduje się w katalogu [`screenshots`](screenshots/).

## Zakres danych źródłowych

| Obszar | Zakres w pliku źródłowym |
| --- | ---: |
| Wiersze sprzedaży | ok. 2 500 |
| Produkty | ok. 90 |
| Klienci | ponad 430 |
| Zwroty i reklamacje | ok. 190 |
| Budżet | ok. 240 rekordów |
| Stany magazynowe | ok. 270 rekordów |

Plik źródłowy znajduje się w katalogu [`data`](data/). Zawiera dane celowo zabrudzone, dlatego częścią projektu są zarówno transformacje, jak i arkusze kontrolne.

## Użyte narzędzia

- Microsoft Excel Desktop,
- Power Query,
- Power Pivot,
- DAX,
- tabele i wykresy przestawne,
- fragmentatory i oś czasu,
- Git oraz GitHub.

## Struktura repozytorium

```text
Excel-Analysis-Dashboard/
├── README.md
├── data/
│   ├── Dane_surowe_projekt_Excel.xlsx
│   └── README.md
├── project/
│   ├── Analiza_Sprzedazy_Excel.xlsx
│   └── README.md
├── screenshots/
│   ├── dashboard.png
│   ├── model-danych.png
│   ├── analiza-czas.png
│   ├── analiza-kategorie.png
│   └── analiza-kanaly.png
└── documentation/
    ├── przygotowanie-danych.md
    ├── model-danych.md
    ├── miary-dax.md
    └── wnioski-biznesowe.md
```

## Jak uruchomić projekt

1. Pobierz repozytorium lub sklonuj je lokalnie.
2. Otwórz [`project/Analiza_Sprzedazy_Excel.xlsx`](project/Analiza_Sprzedazy_Excel.xlsx) w programie Microsoft Excel Desktop.
3. Przejdź do arkusza `DASHBOARD`.
4. Użyj fragmentatorów i osi czasu do filtrowania raportu.
5. Aby przejrzeć model, wybierz **Power Pivot → Manage → Diagram View**.

Po przeniesieniu repozytorium na inny komputer Power Query może poprosić o wskazanie nowej ścieżki do pliku źródłowego z katalogu `data`.

## Dokumentacja

- [Przygotowanie i kontrola danych](documentation/przygotowanie-danych.md)
- [Model danych i relacje](documentation/model-danych.md)
- [Miary DAX](documentation/miary-dax.md)
- [Wnioski biznesowe](documentation/wnioski-biznesowe.md)

## Autor

Oskar Kowalczyk
