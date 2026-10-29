---
title: "TicketSystem"
date: 2026-10-21
draft: false
slug: "ticketsystem"
aliases: ["/projects/resolvedesk/", "/projects/ticketdesk/"]
summary: "Et ticketsystem i Java med JPA, REST, AbuseIPDB og JWT."
---

TicketSystem er mit backendprojekt på 3. semester. Brugere kan oprette og
behandle egne tickets, mens supportere og administratorer kan arbejde med alle.
Kommentarer og notifikationer gør det muligt at følge en henvendelse og dens svar.
Databasen hedder ticketsystem. Den lokale projektmappe hedder stadig ResolveDesk.

[Kode og README](https://github.com/tobiashardoe/ticketsystem) ·
[Deployet API](https://ticketsystem.tobiashardo.dk/api/tickets)

API'et kræver JWT til tickets; der er ingen login-side i browseren.

## Opbygning

[API og dokumentation]({{< relref "projects/ticketsystem-api.md" >}})

Javalin-controllerne håndterer HTTP og kalder DAO-klasserne, der gemmer data med
JPA/Hibernate i PostgreSQL. User, Ticket, Comment og Attachment udgør modellen.
En separat AbuseIpService henter eksterne IP-oplysninger med HttpClient og mapper
JSON til AbuseIpResponseDTO med Jackson.

Ved ticketoprettelse kontrolleres forbindelsens IP. En score på mindst 80 giver
403, og fejl i opslaget giver 503. Begge dele forhindrer lagring. Grænsen er en
projektregel og ikke en garanti for sikkerhed. Flere IP-adresser kan desuden
slås op via en thread pool med tre tråde.

Passwords hashes med bcrypt. Registrering giver USER, og login udsteder JWT.
Brugere kan arbejde med egne tickets; SUPPORT og ADMIN kan behandle alle.
Kun ADMIN kan oprette supportere. Opretter og kommentarforfatter findes fra
tokenet, ikke fra et userId valgt af klienten.

## Tests og notifikationer

Projektets testsuite består af 26 tests:
8 DAO-tests, 1 JSON/DTO-test, 7 user/security REST-tests og 10 ticket-tests,
herunder tre WebSocket-tests. Controller-testene ligger i controllers-pakken.

Supportere og administratorer kan abonnere på nye tickets via /ws/tickets.
Første besked er deres JWT. Efter godkendelse sender serveren CONNECTED;
når en ticket er gemt, sendes TICKET_CREATED og ticketId. Detaljer hentes
gennem REST. Brugere kan også forbinde med eget JWT og modtage notifikationer
om ændringer, svar, tildeling og sletning på egne tickets.
Andre brugere modtager ikke disse beskeder. Funktionen har endnu ingen
brugergrænseflade og virker i én serverproces.

REST-testene bruger Javalin og Rest Assured med Hamcrest. PostgreSQL startes i
Testcontainers. Tickettestene bruger et fast IP-resultat fra en test-underklasse
af AbuseIpService. Det rigtige AbuseIPDB-kald er ikke automatisk testet. Concurrency er implementeret,
men ikke automatisk testet. Login, CRUD og centrale roller er manuelt afprøvet
online. En supporter blev også godkendt med JWT i Chrome og modtog
TICKET_CREATED med korrekt ticket-ID efter oprettelse gennem HTTP-filen.

## User stories

- Som bruger vil jeg kunne registrere mig og logge ind, så mine tickets er knyttet til mig.
- Som bruger vil jeg kunne oprette, se, opdatere og slette mine egne tickets.
- Som bruger vil jeg kunne læse svar og få besked om ændringer på mine tickets.
- Som supporter vil jeg kunne se alle tickets, tildele en supporter og svare på henvendelser.
- Som administrator vil jeg kunne oprette supportere, så de kan behandle tickets.

## Afgrænsning og næste trin

- JAR-filen er manuelt deployet i Docker på DigitalOcean med HTTPS gennem Caddy. GitHub Actions er ikke sat op.
- Hibernate bruger update. Gamle kolonner fjernes ikke automatisk, så ændringer i skemaet skal kontrolleres.
- IP-kontrollen læser forbindelsens adresse, ikke den oprindelige klient-IP bag Caddy. Den del skal afklares.
- Passwordændring, reset og token-refresh er ikke implementeret.
- LLM og Webhooks er ikke en del af den nuværende løsning.

## Ugeindlæg

- [Uge 1 – Opsætning]({{< relref "log/week1-setup.md" >}})
- [Uge 2 – Entiteter og relationer]({{< relref "log/week2-planning-structure.md" >}})
- [Uge 3 – API, JSON og DTO]({{< relref "log/week3-data-integration.md" >}})
- [Uge 4 – Concurrency]({{< relref "log/week4-concurrency.md" >}})
- [Uge 5 – REST API]({{< relref "log/week5-rest-api.md" >}})
- [Uge 6 – REST-tests]({{< relref "log/week6-rest-tests.md" >}})
- [Uge 7 – Security]({{< relref "log/week7-security.md" >}})
- [Uge 8 – Deployment]({{< relref "log/week8-deployment.md" >}})
