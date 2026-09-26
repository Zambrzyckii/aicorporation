# AI Corporation

Backend gry ekonomicznej. Gracz zakłada firmę, kupuje magazyny i zarabia,
a działający w tle silnik gry cyklicznie nalicza dochód z posiadanych budynków.

## Stack

- .NET 10, ASP.NET Core Web API
- Entity Framework Core z bazą PostgreSQL (Npgsql, konwencja nazw snake_case)
- Uwierzytelnianie JWT (Microsoft.AspNetCore.Authentication.JwtBearer)
- Hashowanie haseł: BCrypt.Net-Next
- Dokumentacja API: OpenAPI + Scalar (tylko w środowisku Development)

## Struktura

- `src/AICorporation.Core` – model domenowy: `Company`, `Building`, `Warehouse`,
  `BuildingFactory`, `BuildingType`, `Inventory`, `User`.
- `src/AICorporation.Api` – Web API: kontrolery `Register`, `Login`, `BuyWarehouse`,
  `GetCompanyInfo`, ich handlery w `Services/Handlers` oraz `GameEngineService`
  uruchamiany jako usługa w tle (hosted service).
- `src/Infrastructure` – `AppDbContext` i migracje EF Core (`InitialCreate`, `Refactor`).

## Uruchomienie

Wymagany .NET 10 SDK oraz dostęp do bazy PostgreSQL.

Konfiguracja (np. w `appsettings.json` lub zmiennych środowiskowych):

- `JwtSettings:Key` – klucz do podpisywania tokenów JWT (wymagany, aplikacja
  nie wystartuje bez niego).
- `ConnectionStrings:DefaultConnection` – połączenie do PostgreSQL.

```
dotnet restore
dotnet ef database update --project src/Infrastructure --startup-project src/AICorporation.Api
dotnet run --project src/AICorporation.Api
```

W trybie Development dokumentacja API jest dostępna przez Scalar.

## Stan projektu

Wczesny etap, projekt w rozwoju. Endpointy pokrywają rejestrację, logowanie,
zakup magazynu i podgląd firmy. W `Program.cs` znajduje się jeszcze zaszyta
testowa firma z magazynem (kod eksperymentalny). Brak testów automatycznych.
