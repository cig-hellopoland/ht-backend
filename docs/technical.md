# HelloTicket — dokumentacja techniczna

Przegląd lokalnego repozytorium: **31.10.2026**. Stan kontenerów i bazy nie był sprawdzany na serwerze.

## Dokumenty referencyjne

- [Endpointy JAX-RS](api-reference.md).
- [Zależności Maven, klucze konfiguracji i skrypty SQL](configuration-reference.md).
- [Budowanie, konfiguracja i weryfikacja](development.md).

## Struktura implementacji

Główny pakiet: `pl.hellopolandticket`.

| Pakiet | Zadanie techniczne |
| --- | --- |
| [rest](../src/main/java/pl/hellopolandticket/rest) | Zasoby JAX-RS, konwertery parametrów i konfiguracja JSON |
| [service](../src/main/java/pl/hellopolandticket/service) | Operacje na modelu, walidatory, zdarzenia i schedulery |
| [dao](../src/main/java/pl/hellopolandticket/dao) | Zapytania i dostęp do encji |
| [model](../src/main/java/pl/hellopolandticket/model) | Encje i typy modelu |
| [security](../src/main/java/pl/hellopolandticket/security) | JWT, IdentityStore, current user i filtry |
| [app](../src/main/java/pl/hellopolandticket/app) | Inicjalizacja, producenci CDI i obsługa infrastrukturalna |

Typowy przebieg: zasób REST → serwis → DAO/JPA → model → DTO. Konwersja do wspólnych DTO jest m.in. w [ModelObjectsToDTOConverter](../src/main/java/pl/hellopolandticket/service/util/ModelObjectsToDTOConverter.java). Sprawdzenie endpointu powinno uwzględniać również konwersję i dane ładowane z relacji.

## Kontekst HTTP i bezpieczeństwo

[JaxRsActivator](../src/main/java/pl/hellopolandticket/rest/JaxRsActivator.java) ustawia `v1`. Dla bezpośrednio wdrożonego WAR-a baza to zwykle `/helloticket/v1`; publiczna/infrastrukturalna trasa zależy od proxy.

[JwtAuthenticationMechanism](../src/main/java/pl/hellopolandticket/security/jwt/JwtAuthenticationMechanism.java) obsługuje credentials HTTP, a [JpaIdentityStore](../src/main/java/pl/hellopolandticket/security/JpaIdentityStore.java) weryfikuje tożsamość. Uprawnienia należy sprawdzić w deklaracjach usług i konfiguracji aplikacji. Sama lista endpointów nie jest macierzą ról.

`RestServiceSuperclass` definiuje odziedziczony `GET /ping`; jego finalna trasa zależy od klasy zasobu. Generator wykazuje jawne adnotacje w konkretnych klasach i nie dopisuje automatycznie metod odziedziczonych.

## Dane i współpraca z HP

HT utrzymuje własne identyfikatory definicji biletu, definicji puli, konkretnego wystąpienia puli i powiązania biletu z pulą (ATNA). Nie są one tożsame z `SightEvent.id` HP. Integracja używa m.in. `SightEvent.hptId` po stronie HP do powiązania oferty z HT.

Istotne miejsca techniczne: `TicketPoolDefinitionService`, `TicketPoolService`, `TicketPoolDefinitionAtnasDiffApplier`, `BookingService`, DAO tych modeli oraz walidatory w `service/validator`. Zmiana struktury puli wymaga sprawdzenia aktualizacji powiązań i konwertera DTO, nie tylko zapisu encji nadrzędnej.

Schedulery i listenery w `service` działają w kontenerze aplikacyjnym. Ich obecność w źródłach nie potwierdza ostatniego wykonania. Nie uruchamiamy procesów rezerwacji na danych produkcyjnych jako kontroli samej dokumentacji.

## Persistence i konfiguracja

[persistence.xml](../src/main/resources/META-INF/persistence.xml) wskazuje `primary` oraz `java:module/helloticketDS`. [web.xml](../src/main/webapp/WEB-INF/web.xml) deklaruje PostgreSQL datasource z placeholderami builda. `hibernate.hbm2ddl.auto` i skrypt ładowania SQL są parametrami zasobów; przed startem trzeba potwierdzić ich wynikowe wartości.

Maven filtruje zasoby zgodnie z profilem `tst` lub `prod`. [ServiceSuperclass](../src/main/java/pl/hellopolandticket/service/ServiceSuperclass.java) dodatkowo ładuje classpath `runtime.properties`, a następnie zewnętrzny plik z `local.runtime.properties`. Plik runtime nie zastępuje automatycznie wszystkich parametrów deskryptora datasource.

Konfiguracja zewnętrzna, dostęp do bazy i DMS są wymaganiami uruchomienia. Domyślne zasoby WAR-a nie są dowodem poprawnej konfiguracji konkretnego środowiska.

## Budowanie i artefakt

POM deklaruje **Javę 21**, Jakarta EE 10 i DTO **1.3.15**. Dockerfile używa WildFly 34 z JDK 21. Z katalogu `ht-backend`, z przygotowanymi JDK/Maven/zależnościami:

```sh
mvn -P tst clean package
```

Wynik: `target/helloticket.war`. Zmiana wersji DTO wymaga jawnej zmiany POM i sprawdzenia zgodności kontraktów, nie tylko pobrania nowszego katalogu `hp-dto`.

Nie znaleziono `src/test` w lokalnym module. Budowanie nie zastępuje sprawdzenia JPA, autoryzacji, rezerwacji, generowania biletu i integracji przez HP. Plan technicznej weryfikacji należy dobrać do zmienianego fragmentu.

## Wdrożenie i diagnoza

Przed podmianą artefaktu ustalamy wersję schematu, zachowujemy poprzedni WAR/obraz oraz plik konfiguracji i sprawdzamy montowanie DMS. Wycofanie obrazu nie cofa danych. Przy błędzie trzeba odróżnić HTTP/proxy, credentials, datasource, serializację DTO i zależności plikowe.

Dotychczasowe komendy serwerowe w README są materiałem do ponownego potwierdzenia przez operatora. Nie wykonano ich podczas opracowania tej dokumentacji.
