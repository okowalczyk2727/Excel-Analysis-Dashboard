# Przygotowanie danych w Power Query

## Założenie

Plik źródłowy zawiera dane syntetyczne, celowo przygotowane z problemami jakościowymi. Nie poprawiałem ich ręcznie w arkuszach źródłowych. Transformacje zostały wykonane w Power Query, dzięki czemu proces można powtórzyć po odświeżeniu danych.

## Główne źródła

- sprzedaż,
- produkty i cennik,
- klienci,
- zwroty i reklamacje,
- budżet sprzedaży,
- stany magazynowe.

## Najważniejsze transformacje

### Tekst i słowniki

- usunięcie zbędnych i twardych spacji,
- oczyszczenie znaków niedrukowalnych,
- ujednolicenie wielkości liter,
- mapowanie wariantów kategorii, marek, dostawców, województw, segmentów, kanałów, statusów i metod płatności,
- pozostawienie jednej poprawnej wartości biznesowej dla każdego wariantu źródłowego.

### Produkty

- ujednolicenie kodów i nazw produktów,
- uzupełnienie brakujących kodów przez połączenie z cennikiem po oczyszczonej nazwie,
- kontrola ceny zakupu, ceny katalogowej i stawki VAT.

### Klienci

- oczyszczenie adresów e-mail,
- utworzenie kluczy pomocniczych z nazwy klienta i miasta,
- przypisanie `ID_Klient` do sprzedaży,
- kontrola braków danych kontaktowych oraz numerów NIP.

### Sprzedaż

- ustawienie prawidłowych typów danych,
- konwersja ilości, cen, rabatów i kosztu dostawy do liczb,
- zamiana wariantów bezpłatnej dostawy na `0`,
- kontrola dat zamówienia i wysyłki,
- obliczenie przychodu na poziomie pozycji,
- pozostawienie numeru zamówienia jako identyfikatora powtarzalnego dla wielu produktów.

### Budżet

- ujednolicenie kategorii i kanałów,
- utworzenie daty miesiąca jako pierwszego dnia danego miesiąca,
- kontrola kompletności kombinacji miesiąc–kategoria–kanał.

### Magazyn i zwroty

- ujednolicenie kodów produktów,
- kontrola stanów pustych, zerowych i ujemnych,
- oddzielenie błędów technicznych od alertów operacyjnych,
- kontrola zgodności zwrotów z zamówieniami i produktami.

## Arkusze kontroli

Skoroszyt zawiera osobne arkusze `KONTROLA_*`. Ich zadaniem jest pokazanie rekordów, których nie należy poprawiać automatycznie bez decyzji biznesowej, np.:

- wysyłka przed zamówieniem,
- brak ilości lub ceny,
- ujemny stan magazynowy,
- brak VAT,
- zwrot bez jednoznacznego produktu,
- niekompletny rekord budżetu.

Dzięki temu proces jest audytowalny: poprawne dane trafiają do modelu, a wyjątki pozostają widoczne do dalszej weryfikacji.

## Ładowanie zapytań

- zapytania pomocnicze i mapujące: tylko połączenie,
- tabele kontrolne: arkusze Excela,
- tabele `DIM_*` i `FAKT_*`: model danych Power Pivot,
- dashboard i analizy: tabele przestawne korzystające z modelu.
