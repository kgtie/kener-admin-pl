# Kener Admin PL

Autor i opiekun forka: **kgtie**. Nowe opracowanie polskiej lokalizacji przygotowane dla Kenera 4.2.1. Wersja robocza pozostaje lokalnie na X10Pro; publiczny fork zawiera wyłącznie kod upstreamu do czasu osobnej zgody na publikację.

To zmiana kodu aplikacji i osobna warstwa lokalizacji panelu. Samo skopiowanie `pl.json` do oryginalnego `src/lib/locales/` nie spolszczy panelu administracyjnego. Baza i licencja Kenera pozostają opisane w [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md).

## Wybór języka

W nagłówku panelu oraz na ekranach konta znajduje się przełącznik **Polski / English**. Zmiana działa od razu; wybór zapisuje cookie `kener_admin_locale` na rok, osobno w każdej przeglądarce. Przy pierwszej wizycie wybierany jest obsługiwany język przeglądarki, a przy braku takiego dopasowania — polski.

Opcja **Języki i formaty → Języki** nadal określa języki strony publicznej. Nie zmienia języka panelu. Konteksty językowe tworzone są osobno dla każdego renderowania serwerowego, bez wspólnego stanu użytkowników.

## Zakres

Lokalizacja obejmuje ekrany konta i logowania, wszystkie sekcje zarządzania, konfiguracje typów monitorów, incydenty, prace serwisowe, alerty i ich kanały, subskrypcje, role i uprawnienia, szablony, dostawców integracji, odznaki, osadzanie i klucze API. Uwzględnia etykiety, opisy, placeholdery, wskazówki, walidację, powiadomienia, etykiety dostępności, daty, jednostki czasu i polecenia CodeMirror.

Znane komunikaty Kenera z serwera są tłumaczone przy wyświetlaniu. Treści w bazie, odpowiedzi API i protokoły zachowują oryginalne wartości. Ustawienia autoryzacji, schemat bazy i zależności aplikacji nie są zmieniane.

Celowo zachowane:

- nazwy i treści użytkownika, również przykładowe wpisy nawigacji z instalatora, tytuły stron i treść edytowalnych wiadomości;
- adresy, identyfikatory, zapytania, przykłady kodu, klucze zmiennych i nazwy technologii;
- dokładne frazy `delete <tag>` i `delete <tag> data`, wymagane przez oryginalną walidację usuwania; otaczające je objaśnienia są po polsku;
- dowolne komunikaty zewnętrznych serwerów, baz, systemu, skryptów użytkownika i bibliotek, których Kener nie definiuje w swoim kodzie;
- systemowe kontrolki przeglądarki: ich język zależy od przeglądarki i macOS.

Nie zmieniamy treści emaili w bazie podczas przełączania języka. Podgląd formatu dat strony publicznej prezentuje jej ustawienia, niezależne od języka panelu.

## Pliki i kontrola kompletności

- `src/lib/admin-i18n/en.json` i `pl.json`: pary tekstów źródłowych i tłumaczeń;
- `messages.ts`: czyste tłumaczenie, interpolacja i rozpoznawanie preferencji;
- `context.ts`: kontekst panelu, cookie i polecenia edytora;
- `scripts/admin-i18n/source-inventory.json`: wersja bazowa oraz spis tekstów z odnośnikami do plików i uwagami przeglądu;
- `scripts/admin-i18n/technical-exceptions.json`: przejrzane terminy pozostawione bez zmiany;
- `docs/admin-pl/ODBIOR.md`: faktyczna weryfikacja i ograniczenia.

Z katalogu projektu:

```bash
node scripts/admin-i18n/audit.mjs --report /tmp/kener-admin-pl-audit.json
npm run check
npm test
npm run build
```

Audyt skanuje komponenty osiągalne z panelu i konta, statyczne wywołania tłumaczeń, surowe teksty i atrybuty interfejsu, metadane, błędy kontrolerów i monitorów oraz generowane nazwy grup uprawnień i szablonów. Sprawdza zgodność kluczy i placeholderów obu słowników, nieprzejrzane identyczne tłumaczenia i zmianę wersji upstreamu. Nie modyfikuje kodu ani słowników. Wynik nie zastępuje ręcznego przeglądu językowego i dynamicznych ekranów.

## Aktualizowanie Kenera

1. Potwierdź aktualne stabilne wydanie w oficjalnym repozytorium. Zapisz tag i commit; nie pracuj na niezweryfikowanej gałęzi `main`.
2. Porównaj zmiany od poprzedniej bazy, zwłaszcza `(account)`, `(manage)`, współdzielone komponenty, kontrolery, usługi, uprawnienia i szablony. Zachowaj lokalne zmiany i historię upstreamu.
3. Uruchom audyt. Dodaj nowe teksty do obu słowników, zachowując placeholdery oraz kody w payloadach. Obejrzyj nowe lub zmienione ekrany i teksty budowane dynamicznie.
4. Zaktualizuj spis źródeł i wersję bazową dopiero po ręcznym rozliczeniu zmian. Nie dopisuj automatycznie wyjątków tylko po to, aby audyt przeszedł.
5. Uruchom kontrolę typów, testy i kompilację. Sprawdź PL/EN, odświeżenie strony, błędy, dialogi, edytory i zachowanie treści użytkownika.
6. Przygotuj lokalny commit do odbioru. Push, wydanie, PR i wdrożenie wymagają osobnej zgody właściciela.

Nie uruchamiaj kompilacji produkcyjnej równolegle z lokalnym Vite w tym samym katalogu: oba korzystają z `.svelte-kit`.

## Uruchamianie i publikacja

Wymagania i instalacja pozostają takie jak w oryginalnym Kenerze: Node.js w wersji zgodnej z `package.json`, Redis i obsługiwana baza danych. Własne sekrety przechowuj w prywatnym `.env`, poza Git.

Lokalny podgląd do odbioru używał odrębnego SQLite i Redisa na adresie `127.0.0.1`, fikcyjnego konta oraz danych przykładowych. Nie uruchamiał harmonogramu monitorów, poczty ani procesów produkcyjnych. Katalogi `.local-preview/` i `.local-tools/` są wyłączone lokalnie z Git; nie są częścią rozwiązania ani sposobem wdrożenia.

Publiczny fork: https://github.com/kgtie/kener-admin-pl. Zdalne adresy push lokalnego repozytorium są celowo zablokowane do osobnej zgody. To zabezpieczenie lokalnego Git, nie ograniczenie konta GitHub.
