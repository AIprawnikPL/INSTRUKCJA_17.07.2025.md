# INSTRUKCJA_17.07.2025.md

# API Kancelarii Prawnej

Poniżej znajduje się ulepszona i szczegółowa dokumentacja dla projektu RESTful API do zarządzania kancelariami prawnymi i klientami, zbudowanego z użyciem FastAPI. Dokumentacja została przetłumaczona na język polski i rozszerzona o kluczowe, brakujące funkcjonalności oraz najlepsze praktyki w zakresie struktury projektu.

## Opis Projektu

API Kancelarii Prawnej to kompleksowe rozwiązanie backendowe, mające na celu cyfryzację i automatyzację operacji w kancelariach prawnych. System umożliwia zarządzanie kancelariami, ich klientami, prowadzonymi sprawami oraz personelem. Zbudowany przy użyciu nowoczesnego frameworka FastAPI, zapewnia wysoką wydajność, automatyczną walidację danych oraz interaktywną dokumentację.

## Ulepszona Struktura Projektu

Dla zapewnienia skalowalności, łatwości w utrzymaniu i testowaniu, projekt został zorganizowany w sposób modularny, oddzielając od siebie poszczególne warstwy aplikacji.

```
kancelaria/
│
├── app/
│   ├── api/
│   │   └── v1/
│   │       ├── endpoints/      # Moduły z logiką endpointów (np. kancelarie.py, klienci.py)
│   │       │   ├── kancelarie.py
│   │       │   └── klienci.py
│   │       └── schemas/        # Schematy Pydantic do walidacji danych (np. kancelaria.py)
│   │           └── kancelaria.py
│   ├── core/                   # Konfiguracja, ustawienia globalne, bezpieczeństwo
│   │   └── config.py
│   ├── db/                     # Zarządzanie sesją i połączeniem z bazą danych
│   │   └── session.py
│   ├── models/                 # Modele bazy danych (np. SQLAlchemy)
│   │   └── kancelaria.py
│   ├── services/               # Logika biznesowa aplikacji (oddzielona od endpointów)
│   │   └── kancelaria_service.py
│   ├── tests/                  # Testy jednostkowe i integracyjne
│   └── main.py                 # Główny plik aplikacji FastAPI
│
├── .env.example                # Przykładowy plik ze zmiennymi środowiskowymi
├── openapi.yaml                # Statyczna specyfikacja OpenAPI
├── requirements.txt            # Zależności Python
└── README.md                   # Dokumentacja projektu
```

### Objaśnienie Struktury:
*   **`endpoints/`**: Odpowiada za obsługę żądań HTTP, walidację danych wejściowych (przy pomocy Pydantic) i wywoływanie odpowiednich serwisów.
*   **`schemas/`**: Definiuje kształt danych przychodzących i wychodzących z API przy użyciu modeli Pydantic.
*   **`services/`**: Zawiera całą logikę biznesową. Oddzielenie jej od endpointów sprawia, że kod jest czystszy i łatwiejszy do testowania.
*   **`core/config.py`**: Zarządza konfiguracją aplikacji, wczytując zmienne środowiskowe (np. dane do bazy danych, klucze API) z pliku `.env`.
*   **`db/`**: Odpowiada za konfigurację połączenia z bazą danych i zarządzanie sesjami SQLAlchemy.
*   **`.env.example`**: Służy jako szablon dla pliku `.env`, w którym przechowywane są wrażliwe dane konfiguracyjne.

---

## Konfiguracja i Instalacja

#### 1. Klonowanie Repozytorium i Nawigacja
```bash
git clone <adres-repozytorium>
cd kancelaria
```

#### 2. Konfiguracja Środowiska
Utwórz plik `.env` na podstawie dołączonego szablonu `.env.example` i uzupełnij go odpowiednimi wartościami.
```bash
cp .env.example .env
# Otwórz plik .env w edytorze i uzupełnij zmienne
# np. DATABASE_URL="postgresql://user:password@localhost/kancelaria_db"
```

#### 3. Wirtualne Środowisko i Zależności
Zalecane jest użycie wirtualnego środowiska w celu izolacji zależności projektu.

```bash
# Utwórz wirtualne środowisko
python -m venv venv

# Aktywuj środowisko
# Windows
.\venv\Scripts\activate
# macOS/Linux
source venv/bin/activate

# Zainstaluj zależności Pythona
pip install -r requirements.txt
```

---

## Uruchomienie Aplikacji

Do uruchomienia serwera deweloperskiego FastAPI wykorzystujemy Uvicorn.

```bash
uvicorn app.main:app --reload
```
*   `--reload`: Powoduje automatyczne przeładowanie serwera po każdej zmianie w kodzie.

Aplikacja będzie dostępna pod adresem:
*   **API**: `http://127.0.0.1:8000`
*   **Interaktywna dokumentacja (Swagger UI)**: `http://127.0.0.1:8000/docs`
*   **Alternatywna dokumentacja (ReDoc)**: `http://127.0.0.1:8000/redoc`

---

## Rozszerzone Funkcjonalności API (v1)

API zostało rozszerzone o dodatkowe zasoby, kluczowe dla oprogramowania do zarządzania kancelarią.

### Zarządzanie Kancelariami (`/api/v1/kancelarie`)
| Metoda | Ścieżka                 | Opis                               |
|--------|--------------------------|------------------------------------|
| `POST` | `/`                      | Tworzy nową kancelarię.            |
| `GET`  | `/`                      | Zwraca listę wszystkich kancelarii.|
| `GET`  | `/{kancelaria_id}`       | Pobiera szczegóły kancelarii.      |
| `PUT`  | `/{kancelaria_id}`       | Aktualizuje dane kancelarii.       |
| `DELETE`| `/{kancelaria_id}`      | Usuwa kancelarię.                  |

### Zarządzanie Klientami (`/api/v1/klienci`)
| Metoda | Ścieżka                 | Opis                               |
|--------|--------------------------|------------------------------------|
| `POST` | `/`                      | Dodaje nowego klienta.             |
| `GET`  | `/`                      | Zwraca listę wszystkich klientów.  |
| `GET`  | `/{klient_id}`           | Pobiera szczegóły klienta.         |
| `PUT`  | `/{klient_id}`           | Aktualizuje dane klienta.          |
| `DELETE`| `/{klient_id}`           | Usuwa klienta.                     |

### Zarządzanie Sprawami (`/api/v1/sprawy`)
Zasób do zarządzania sprawami prowadzonymi przez kancelarię, powiązanymi z konkretnymi klientami.

| Metoda | Ścieżka                 | Opis                               |
|--------|--------------------------|------------------------------------|
| `POST` | `/`                      | Rejestruje nową sprawę.            |
| `GET`  | `/`                      | Zwraca listę spraw (z filtrowaniem).|
| `GET`  | `/{sprawa_id}`           | Pobiera szczegóły sprawy.          |
| `PUT`  | `/{sprawa_id}`           | Aktualizuje status lub dane sprawy.|
| `DELETE`| `/{sprawa_id}`           | Usuwa sprawę (archiwizuje).        |

---

## Specyfikacja OpenAPI

Specyfikacja OpenAPI jest generowana automatycznie przez FastAPI i dostępna pod adresem `/openapi.json`. Statyczna wersja jest utrzymywana w pliku `openapi.yaml` dla celów dokumentacyjnych i generowania klientów (SDK).

## Plan Rozwoju (Dalsze Kroki)

1.  **Pełna integracja z bazą danych**: Implementacja wszystkich modeli SQLAlchemy i logiki serwisów.
2.  **Uwierzytelnianie i autoryzacja**: Wdrożenie mechanizmów opartych na JWT (JSON Web Tokens) i OAuth2, aby zabezpieczyć endpointy i wprowadzić system ról (np. Administrator, Prawnik, Klient).
3.  **Testy**: Stworzenie kompleksowego zestawu testów jednostkowych i integracyjnych przy użyciu `pytest` i `TestClient`, aby zapewnić niezawodność i stabilność API.
4.  **Ciągła Integracja i Dostarczanie (CI/CD)**: Skonfigurowanie pipeline'ów (np. przy użyciu GitHub Actions) do automatycznego testowania i wdrażania aplikacji.
5.  **Zarządzanie dokumentami**: Dodanie funkcjonalności przesyłania, przechowywania i zarządzania dokumentami powiązanymi ze sprawami.
6.  **Generowanie SDK**: Rozwój skryptów do automatycznego generowania bibliotek klienckich (SDK) dla Pythona i JavaScript na podstawie pliku `openapi.yaml`.
7.  **Rozwój generatora snippetów**: Stworzenie aplikacji (np. w React), która ułatwi programistom korzystanie z API poprzez generowanie gotowych fragmentów kodu.



=================================


---

### Instalacja i Uruchomienie Frontend'u

Aplikacja frontendowa, służąca jako interfejs użytkownika dla API (np. do generowania snippetów kodu lub zarządzania danymi), znajduje się w katalogu `frontend/`. Do zarządzania jej zależnościami projekt wykorzystuje `pnpm`.

#### Czym jest pnpm?
`pnpm` (performant npm) to nowoczesny menedżer pakietów dla Node.js. Jego głównymi zaletami w stosunku do standardowego `npm` czy `Yarn` są:
*   **Wydajność i szybkość:** `pnpm` jest znacznie szybszy, ponieważ instaluje pakiety równolegle i unika powtarzania tych samych operacji.
*   **Oszczędność miejsca na dysku:** Zamiast kopiować wszystkie pakiety do folderu `node_modules` każdego projektu, `pnpm` tworzy globalne repozytorium na dysku i używa dowiązań (linków) do współdzielenia zależności między projektami. Dzięki temu ten sam pakiet, nawet w tej samej wersji, jest fizycznie przechowywany na dysku tylko raz.
*   **Niezawodność i bezpieczeństwo:** Struktura, którą tworzy `pnpm`, jest bardziej przewidywalna i zapobiega sytuacji, w której kod ma dostęp do pakietów, które nie zostały jawnie zadeklarowane w `package.json`.

#### 1. Instalacja pnpm
Jeśli nie masz jeszcze zainstalowanego `pnpm`, najłatwiej jest go zainstalować globalnie przy pomocy `npm` (który jest domyślnie dołączony do Node.js).

```bash
npm install -g pnpm
```
Aby zweryfikować instalację, uruchom:
```bash
pnpm --version
```

#### 2. Instalacja Zależności Frontendowych
Po zainstalowaniu `pnpm`, możesz zainstalować wszystkie zależności potrzebne dla aplikacji frontendowej.

```bash
# Przejdź do katalogu frontend
cd frontend

# Zainstaluj zależności za pomocą pnpm
pnpm install
```
Polecenie to przeczyta plik `package.json` w katalogu `frontend` i pobierze wszystkie wymagane biblioteki do folderu `node_modules`.

#### 3. Uruchomienie Aplikacji Frontendowej
Aby uruchomić serwer deweloperski aplikacji frontendowej (np. stworzonej w React, Vue lub Angular), najczęściej używa się jednego z poniższych poleceń:

```bash
# Uruchom serwer deweloperski
pnpm dev
```
lub
```bash
pnpm start
```
*Dokładne polecenie zależy od tego, jak zostało ono zdefiniowane w sekcji `"scripts"` w pliku `package.json`.*

Po uruchomieniu aplikacja frontendowa będzie zazwyczaj dostępna w przeglądarce pod adresem `http://localhost:3000` lub podobnym (dokładny adres URL pojawi się w konsoli po wykonaniu polecenia).
