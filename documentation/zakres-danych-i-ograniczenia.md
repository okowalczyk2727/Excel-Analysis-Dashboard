# Zakres danych i ograniczenia

## Dlaczego dwa skoroszyty pokazują różne wartości

Skoroszyt analityczny i model finansowy korzystają z tego samego materiału źródłowego, ale odpowiadają na inne pytania.

`Analiza_Sprzedazy_Excel.xlsx` jest raportem operacyjnym. Wartości na dashboardzie wynikają z miar DAX, relacji modelu danych i filtrów statusu, daty, kategorii oraz kanału.

`Model_Finansowo_Controllingowy.xlsx` jest modelem planistycznym. Najpierw uzgadnia pełne agregaty źródłowe, następnie wyłącza rekordy wskazane w arkuszach `KONTROLA_*` i na tej podstawie buduje wykonanie 2025 oraz plan 2026.

| Zakres | Przychód 2025 | Budżet 2025 | Realizacja |
| --- | ---: | ---: | ---: |
| Dashboard operacyjny | 3 536 247,41 zł | 5 784 080,94 zł | 61,14% |
| Model po kontrolach | 3 514 692,03 zł | 5 462 374,78 zł | 64,34% |

Różnic nie wyrównano ręcznie. Każdy wynik zachowuje własną definicję. Uzgodnienie można prześledzić w arkuszach `00_INSTRUKCJA`, `02_ZRODLO_MIESIACE`, `05_WYLACZENIA_KONTROLNE` i `25_ZRODLA_I_WNIOSKI`.

## Wyłączenia kontrolne

Model finansowy wyłącza tylko rekordy pokazane w arkuszach kontroli. Nie zastosowano dodatkowego czyszczenia, które zmieniałoby wynik bez śladu.

| Obszar | Liczba rekordów kontrolnych | Sposób użycia |
| --- | ---: | --- |
| Sprzedaż | 83 | wyłączenie z wykonania finansowego |
| Budżet | 16 | wyłączenie z czystego budżetu |
| Zwroty | 5 | wyłączenie z analizy zwrotów |
| Produkty, klienci i magazyn | liczniki kontrolne | zachowanie informacji o jakości danych |

Wpływ wyłączeń sprzedaży na pełny agregat źródłowy wynosi 54 824,21 zł.

## Różnica na osi czasu

Po kontrolach występuje różnica 3 228,11 zł między sumą 12 miesięcy a pełnym wynikiem kategorii i kanałów. Model nie rozdziela tej kwoty sztucznie na miesiące. Pokazuje ją jako pozycję poza osią miesięczną, dzięki czemu suma roczna pozostaje zgodna ze źródłem.

## Dane rzeczywiste i założenia

W modelu rozdzielono dwa typy informacji:

- wartości 2025 pochodzą z dołączonego skoroszytu analitycznego,
- wartości 2026 wynikają z jawnych założeń controllingowych.

Założeniami są między innymi wzrost wolumenu i ceny, stopa zwrotów, udział kosztu sprzedaży, inflacja kosztów, zatrudnienie, CAPEX, DSO, DIO, DPO, finansowanie i podatek. Nie należy traktować planu 2026 jako prognozy rynkowej. Jest to wynik przyjętych parametrów.

## Ograniczenia

- Dane są syntetyczne i nie opisują konkretnej firmy.
- Model nie zawiera prognozy kursów walut, stóp procentowych ani popytu rynkowego.
- Zapasy w skoroszycie analitycznym są pokazane ilościowo. Wycena NWC w modelu opiera się na driverze DIO, a nie na pełnej kartotece magazynowej.
- Koszty OPEX, zatrudnienie, CAPEX i finansowanie są założeniami, ponieważ nie występowały w źródłowym raporcie sprzedażowym.
- Scenariusze pokazują zależność wyniku od parametrów, ale nie przypisują prawdopodobieństwa każdemu wariantowi.
- Power Query może wymagać wskazania lokalnej ścieżki do pliku źródłowego po pobraniu repozytorium.
- Pełna obsługa Power Pivot, DAX, fragmentatorów i osi czasu wymaga Microsoft Excel Desktop dla Windows.

## Jak czytać wyniki

Najpierw należy sprawdzić status w `24_KONTROLE_MODELU`. Następnie można oceniać wykonanie, plan i scenariusze. Status `PASS` potwierdza zgodność techniczną obliczeń, ale nie oznacza automatycznej akceptacji założeń biznesowych.
