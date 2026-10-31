# Zależności i klucze konfiguracji

Inwentaryzacja lokalnych źródeł: **2026-10-31**. Ostatni commit spoza dokumentacji: `f645773`; uwzględniono również bieżące pliki robocze.

Plik generowany przez `hellopoland-docs/scripts/build-technical-reference.py`. Opis architektury i ograniczeń: [technical.md](technical.md). Wykaz nie potwierdza wdrożenia ani wykonania testów.

| Bezpośrednia zależność Maven | Wersja / wyrażenie POM | Scope |
| --- | --- | --- |
| `jakarta.platform:jakarta.jakartaee-api` | 10.0.0 | provided |
| `com.fasterxml.jackson.dataformat:jackson-dataformat-csv` | 2.18.1 | compile |
| `org.projectlombok:lombok` | 1.18.38 | provided |
| `org.slf4j:slf4j-jdk14` | 2.0.16 | compile |
| `pl.hellopoland:hellopoland-dto` | 1.3.15 | compile |
| `com.google.zxing:core` | 3.5.3 | compile |
| `com.google.zxing:javase` | 3.5.3 | compile |
| `org.freemarker:freemarker` | 2.3.33 | compile |
| `org.eclipse.angus:angus-mail` | 2.0.3 | provided |
| `io.jsonwebtoken:jjwt-api` | 0.12.6 | compile |
| `io.jsonwebtoken:jjwt-impl` | 0.12.6 | compile |
| `io.jsonwebtoken:jjwt-jackson` | 0.12.6 | compile |
| `org.apache.commons:commons-lang3` | 3.15.0 | compile |
| `io.swagger.core.v3:swagger-jaxrs2-jakarta` | 2.2.26 | compile |
| `io.swagger.core.v3:swagger-jaxrs2-servlet-initializer-jakarta` | 2.2.26 | compile |
| `org.postgresql:postgresql` | 42.7.3 | runtime |

## Klucze plików properties

Poniżej wyłącznie nazwy kluczy, bez wartości. Lista wskazuje deklaracje w zasobach, nie gwarantuje użycia każdego klucza ani kompletności zewnętrznego `local.runtime.properties`. Klucze filtrowania Maven i właściwości runtime nie są zamienne.

### maven.build.prod.properties

| Klucz | Deklaracja |
| --- | --- |
| `hibernate.hbm2ddl.auto` | [maven.build.prod.properties](../src/main/resources/maven.build.prod.properties#L1) |
| `jakarta.persistence.sql-load-script-source` | [maven.build.prod.properties](../src/main/resources/maven.build.prod.properties#L2) |
| `jakarta.persistence.jdbc.host` | [maven.build.prod.properties](../src/main/resources/maven.build.prod.properties#L3) |
| `jakarta.persistence.jdbc.port` | [maven.build.prod.properties](../src/main/resources/maven.build.prod.properties#L4) |
| `jakarta.persistence.jdbc.database` | [maven.build.prod.properties](../src/main/resources/maven.build.prod.properties#L5) |
| `jakarta.persistence.jdbc.user` | [maven.build.prod.properties](../src/main/resources/maven.build.prod.properties#L6) |
| `jakarta.persistence.jdbc.password` | [maven.build.prod.properties](../src/main/resources/maven.build.prod.properties#L7) |

### maven.build.properties

| Klucz | Deklaracja |
| --- | --- |
| `hibernate.hbm2ddl.auto` | [maven.build.properties](../src/main/resources/maven.build.properties#L1) |
| `jakarta.persistence.sql-load-script-source` | [maven.build.properties](../src/main/resources/maven.build.properties#L2) |
| `jakarta.persistence.jdbc.host` | [maven.build.properties](../src/main/resources/maven.build.properties#L3) |
| `jakarta.persistence.jdbc.port` | [maven.build.properties](../src/main/resources/maven.build.properties#L4) |
| `jakarta.persistence.jdbc.database` | [maven.build.properties](../src/main/resources/maven.build.properties#L5) |
| `jakarta.persistence.jdbc.user` | [maven.build.properties](../src/main/resources/maven.build.properties#L6) |
| `jakarta.persistence.jdbc.password` | [maven.build.properties](../src/main/resources/maven.build.properties#L7) |

### maven.build.tst.properties

| Klucz | Deklaracja |
| --- | --- |
| `hibernate.hbm2ddl.auto` | [maven.build.tst.properties](../src/main/resources/maven.build.tst.properties#L1) |
| `jakarta.persistence.sql-load-script-source` | [maven.build.tst.properties](../src/main/resources/maven.build.tst.properties#L2) |
| `jakarta.persistence.jdbc.host` | [maven.build.tst.properties](../src/main/resources/maven.build.tst.properties#L3) |
| `jakarta.persistence.jdbc.port` | [maven.build.tst.properties](../src/main/resources/maven.build.tst.properties#L4) |
| `jakarta.persistence.jdbc.database` | [maven.build.tst.properties](../src/main/resources/maven.build.tst.properties#L5) |
| `jakarta.persistence.jdbc.user` | [maven.build.tst.properties](../src/main/resources/maven.build.tst.properties#L6) |
| `jakarta.persistence.jdbc.password` | [maven.build.tst.properties](../src/main/resources/maven.build.tst.properties#L7) |

### runtime.properties

| Klucz | Deklaracja |
| --- | --- |
| `mail.personal` | [runtime.properties](../src/main/resources/runtime.properties#L1) |
| `mail.smtp.enabled` | [runtime.properties](../src/main/resources/runtime.properties#L2) |
| `mail.username` | [runtime.properties](../src/main/resources/runtime.properties#L3) |
| `mail.password` | [runtime.properties](../src/main/resources/runtime.properties#L4) |
| `mail.smtp.host` | [runtime.properties](../src/main/resources/runtime.properties#L5) |
| `mail.smtp.auth` | [runtime.properties](../src/main/resources/runtime.properties#L6) |
| `mail.smtp.starttls.enable` | [runtime.properties](../src/main/resources/runtime.properties#L7) |
| `mail.hellopoland.biuro` | [runtime.properties](../src/main/resources/runtime.properties#L8) |
| `resteasy.preferJacksonOverJsonB` | [runtime.properties](../src/main/resources/runtime.properties#L10) |
| `pool.entry.date.repairer.enabled` | [runtime.properties](../src/main/resources/runtime.properties#L11) |
| `mail.ticket.backup` | [runtime.properties](../src/main/resources/runtime.properties#L12) |
| `mail.hellopoland.helpdesk` | [runtime.properties](../src/main/resources/runtime.properties#L13) |
| `mail.partner.credentials.recipient` | [runtime.properties](../src/main/resources/runtime.properties#L14) |

## Skrypty SQL

To pliki obecne w repozytorium, a nie automatyczny plan migracji. Kolejność i stan wykonania wymagają sprawdzenia dla konkretnego wdrożenia.

| Plik |
| --- |
| [add_ticket_pool_visibility_and_special_ticket_type.sql](../src/main/resources/sql/add_ticket_pool_visibility_and_special_ticket_type.sql) |
| [alter_sight_event_text_fields.sql](../src/main/resources/sql/alter_sight_event_text_fields.sql) |
| [audit_sight_event_partner_consistency.sql](../src/main/resources/sql/audit_sight_event_partner_consistency.sql) |
| [fix_sight_event_partner_consistency.example.sql](../src/main/resources/sql/fix_sight_event_partner_consistency.example.sql) |
| [module4.sql](../src/main/resources/sql/module4.sql) |
