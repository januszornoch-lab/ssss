# Print Check Pro · Media Design

Kontrola plików do druku (preflight) działająca w przeglądarce. Plik klienta nie opuszcza komputera.

Jak uruchomić: otwórz `index.html` w przeglądarce (potrzebny internet do pobrania bibliotek pdf.js i pdf-lib z cdnjs).
`app.html` to ta sama aplikacja bez nagłówka dokumentu – wersja publikowana jako artefakt claude.ai.
Po zmianie `app.html` odtwórz `index.html` (owinięcie w `<!doctype html>…<body>`).

## Co sprawdza
Format netto, spady, TrimBox/BleedBox, rozdzielczość obrazów (efektywne dpi po skalowaniu), przestrzeń barw (RGB/CMYK/Lab),
kolory dodatkowe (Pantone), pokrycie farbą TAC (dokładnie dla wektorów CMYK, szacunkowo z podglądu), osadzenie czcionek,
grubość linii, wielkość tekstu, margines bezpieczeństwa, przezroczystości i nadruk, szyfrowanie.

Od wersji profili v2 (22.11.2026):
- progi wg GWG 2015 (offset arkuszowy CMYK): obrazy błąd < 150 / ostrzeżenie < 225 / za ciężkie > 450 ppi, grafika 1-bit 550/800 ppi,
  linia błąd < 0,125 pt / ostrzeżenie < 0,25 pt, tekst błąd < 5 pt / ostrzeżenie < 6 pt, tekst z 2+ farb min. 9 pt;
  papier niepowlekany TAC 300% (FOGRA52), roll-up 850 × 2050 mm bez spadu, 72/120 dpi;
- kolor tekstu: czarny tekst z 4 farb (> 280% = błąd), czarny tekst w RGB, mały czarny tekst bez nadruku;
- biały obiekt z nadrukiem (błąd – znika na druku);
- wykrojnik / linia cięcia jako kolor dodatkowy (CutContour, Wykrojnik, Stanze…) – kontrola nadruku; wtedy poprawki rastrowe są wyłączone;
- pliki w skali 1:10 (duży format) – wymiary, dpi, linie i tekst przeliczane ×10;
- różne formaty stron w jednym pliku, obrócone strony, CropBox ≠ MediaBox, czcionka Courier;
- PDF/X i output intent (informacyjnie).
Profile zapisane wcześniej w przeglądarce: nieedytowane przechodzą na v2, edytowane zachowują swoje wartości i dostają nowe pola.

Źródła progów: https://www.prepressure.com/pdf/basics/preflight · https://automatalabs.ca/blog/gwg-2015-sheetfed-cmyk-preflight-explained/ ·
https://registry.color.org/profile-registry/PSOuncoated_v3_FOGRA52 · https://printsmarket.pl/article/rozdzielczosci-plikow

## Poprawki automatyczne
- Ustawienie TrimBox/BleedBox, gdy spad jest w pliku (bezstratnie).
- Spłaszczenie do obrazu CMYK (150–400 dpi): dodanie spadu przez lustrzane odbicie krawędzi, konwersja RGB→CMYK z limitem TAC,
  skalowanie do formatu przy zgodnych proporcjach, usunięcie problemu nieosadzonych czcionek.
- Słabej rozdzielczości, cienkich linii, małego tekstu i tekstu przy krawędzi poprawka nie usuwa – raport przenosi te wyniki z oryginału.

Konwersja barw jest uproszczona (bez profilu ICC). Do prac z krytycznym kolorem plik przygotowuje się w programie źródłowym z profilem drukarni (np. FOGRA39/51).

## Pozostałe
Profile kontroli są wersjonowane (każda zmiana reguł = nowa wersja, raport podaje wersję). Porównanie przed/po z suwakiem,
akceptacja do druku (kto, kiedy, SHA-256 pliku), raport TXT/JSON, historia ostatnich kontroli w przeglądarce.
