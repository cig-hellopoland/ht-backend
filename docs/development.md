# HelloTicket — architektura i praca nad modułem

Sprawdzono lokalny kod **31.10.2026**. Dokument nie potwierdza konfiguracji działających kontenerów.

## Odpowiedzialność

HelloTicket obsługuje definicje biletów, typy, pule, dostępność, rezerwacje i operacje na sprzedanych biletach. Backend HP korzysta z niego jako systemu biletowego. Identyfikatory HP i HT należą do różnych modeli; nie można ich zamieniać wyłącznie na podstawie podobnej nazwy.

## Mapa kodu

Główny pakiet to `pl.hellopolandticket`.

| Miejsce | Rola |
| --- | --- |
| [rest](../src/main/java/pl/hellopolandticket/rest) | Endpointy JAX-RS: m.in. rezerwacje, definicje biletów, pule i oferty biletowe |
| [service](../src/main/java/pl/hellopolandticket/service) | Reguły biletowe, walidacja, rezerwacje i zadania okresowe |
| [dao](../src/main/java/pl/hellopolandticket/dao) | Dostęp do danych |
| [model](../src/main/java/pl/hellopolandticket/model) | Encje i model domenowy |
| [security](../src/main/java/pl/hellopolandticket/security) | Kontrola dostępu |
| [ModelObjectsToDTOConverter](../src/main/java/pl/hellopolandticket/service/util/ModelObjectsToDTOConverter.java) | Mapowanie modelu do kontraktów odpowiedzi |

W zmianach biletowych trzeba rozróżniać definicję puli i jej wystąpienie. Sprawdzamy nie tylko cenę, ale też datę, cykliczność, limity i widoczność na portalu. Miejsca wejścia to `TicketPoolDefinitionService`, `TicketPoolService`, `BookingService` i ich endpointy.

## Budowanie

[POM](../pom.xml) deklaruje Java 21, Jakarta EE 10, artefakt `helloticket.war` oraz zależność DTO **1.3.15**. [Dockerfile](../Dockerfile) wskazuje WildFly 34 z JDK 21. Starsza informacja o runtime Java 17 nie dotyczy tak skompilowanego WAR-a.

Z katalogu `ht-backend`, po przygotowaniu JDK 21, Maven i zależności:

```sh
mvn -P tst clean package
```

Wynik: `target/helloticket.war`. Dla PROD używamy jawnie `-P prod`. Samo istnienie nowszego lokalnego `hp-dto` nie zmienia wersji wskazanej przez ten POM.

## Konfiguracja

Profile Maven wybierają `maven.build.tst.properties` lub `maven.build.prod.properties`. Zmieniają zasoby podczas budowania. Datasource `java:module/helloticketDS` jest wskazany w [persistence.xml](../src/main/resources/META-INF/persistence.xml).

[ServiceSuperclass](../src/main/java/pl/hellopolandticket/service/ServiceSuperclass.java) ładuje domyślne `runtime.properties` z WAR-a do właściwości JVM, a następnie zewnętrzny plik wskazany przez `-Dlocal.runtime.properties=...`. Zmiana pliku wymaga ponownej inicjalizacji aplikacji; plik nie jest odczytywany przy każdym żądaniu.

W uruchomieniu trzeba zapewnić bazę/datasource, zewnętrzny plik właściwości i DMS. Nie przenosimy wartości dostępowych do instrukcji. Diagnostyka DEBUG w obecnym loaderze wypisuje właściwości — logi wymagają oczyszczenia przed udostępnieniem.

## Kontrakty HTTP

[JaxRsActivator](../src/main/java/pl/hellopolandticket/rest/JaxRsActivator.java) ustawia `v1`; przy bezpośrednim dostępie do WAR-a jest to zwykle `/helloticket/v1`. Konkretne ścieżki i metody określają klasy w `rest`, m.in.:

- `BookingRestService`: rezerwacje i ich obsługa;
- `TicketDefinitionRestService`: definicje biletów;
- `TicketPoolDefinitionRestService`: definicje pul i terminy;
- `SightEventRestService`: biletowe oferty/wydarzenia;
- `AuthenticationRestService`: logowanie, odświeżenie i wylogowanie.

Nie należy oceniać działania usługi wyłącznie przez `GET /login`: metodę, payload i uprawnienia trzeba dobrać do endpointu. Odpowiedź 401 nie potwierdza sprawności bazy ani rezerwacji.

## Weryfikacja zmiany

W tym przeglądzie nie znaleziono katalogu `src/test`. Sam sukces budowania nie zastępuje sprawdzenia reguł biletowych. Na TST należy dobrać scenariusze do zmiany: odczyt dostępności, pojedyncza i cykliczna pula, wyczerpany limit, rezerwacja testowa oraz anulowanie. Operacje zapisu wymagają przygotowanych danych testowych.

Wynik integracji sprawdzamy także przez backend HP, ponieważ bezpośrednia poprawna odpowiedź HT nie dowodzi poprawnego mapowania w HP.

## Wdrożenie i wycofanie

Przed wdrożeniem potwierdzamy wersję wymaganego DTO, obrazu, WAR-a, konfiguracji i DMS oraz ewentualne zmiany bazy. Zachowujemy poprzedni artefakt; przywrócenie obrazu nie cofa rezerwacji ani migracji danych.

Dotychczasowy [README](../README.md) zachowuje instrukcję TST i odnośnik do Wiki. Nie sprawdzono aktualności tych danych na serwerze. Polecenia budowania w tym dokumencie nie były uruchamiane w ramach rozbudowy dokumentacji.
