# Print Check Pro · Media Design

Kontrola plików do druku (preflight) działająca w przeglądarce. Plik klienta nie opuszcza komputera.

Jak uruchomić: otwórz `index.html` w przeglądarce (potrzebny internet do pobrania bibliotek pdf.js i pdf-lib z cdnjs).
`app.html` to ta sama aplikacja bez nagłówka dokumentu – wersja publikowana jako artefakt claude.ai.
Po zmianie `app.html` odtwórz `index.html` (owinięcie w `<!doctype html>…<body>`).

## Co sprawdza
Format netto, spady, TrimBox/BleedBox, rozdzielczość obrazów (efektywne dpi po skalowaniu), przestrzeń barw (RGB/CMYK/Lab),
kolory dodatkowe (Pantone), pokrycie farbą TAC (dokładnie dla wektorów CMYK, szacunkowo z podglądu), osadzenie czcionek,
grubość linii, wielkość tekstu, margines bezpieczeństwa, przezroczystości i nadruk, szyfrowanie.

## Poprawki automatyczne
- Ustawienie TrimBox/BleedBox, gdy spad jest w pliku (bezstratnie).
- Spłaszczenie do obrazu CMYK (150–400 dpi): dodanie spadu przez lustrzane odbicie krawędzi, konwersja RGB→CMYK z limitem TAC,
  skalowanie do formatu przy zgodnych proporcjach, usunięcie problemu nieosadzonych czcionek.
- Słabej rozdzielczości, cienkich linii, małego tekstu i tekstu przy krawędzi poprawka nie usuwa – raport przenosi te wyniki z oryginału.

Konwersja barw jest uproszczona (bez profilu ICC). Do prac z krytycznym kolorem plik przygotowuje się w programie źródłowym z profilem drukarni (np. FOGRA39/51).

## Pozostałe
Profile kontroli są wersjonowane (każda zmiana reguł = nowa wersja, raport podaje wersję). Porównanie przed/po z suwakiem,
akceptacja do druku (kto, kiedy, SHA-256 pliku), raport TXT/JSON, historia ostatnich kontroli w przeglądarce.
