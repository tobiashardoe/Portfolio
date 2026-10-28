---
title: "TicketDesk"
date: 2026-10-21
draft: false
slug: "ticketdesk"
aliases: ["/projects/resolvedesk/"]
summary: "Et ticketsystem i Java med JPA, REST, AbuseIPDB og JWT."
---

TicketDesk er mit backendprojekt på 3. semester. Brugere kan oprette tickets,
tilføje kommentarer og registrere oplysninger om attachments. Supportere og
administratorer kan behandle tickets. Attachments er på nuværende tidspunkt
metadata og en filsti; der er ikke implementeret egentlig filupload.

Projektet hed tidligere ResolveDesk. Den lokale projektmappe og databasen bruger
fortsat det oprindelige navn, mens projektets navn nu er TicketDesk.

## Opbygning

[API-dokumentation: endpoints, JSON og adgangskrav]({{< relref "projects/ticketsystem-api.md" >}})

Javalin-controllerne håndterer HTTP og kalder DAO-klasserne, der gemmer data med
JPA/Hibernate i PostgreSQL. User, Ticket, Comment og Attachment udgør modellen.
En separat AbuseIpService henter eksterne IP-oplysninger med HttpClient og mapper
JSON til AbuseIpResponseDTO med Jackson.

Ved ticketoprettelse kontrolleres afsenderens IP. En score på mindst 80 giver
403, og fejl i opslaget giver 503. Begge dele forhindrer lagring. Grænsen er en
projektregel og ikke en garanti for sikkerhed. Flere IP-adresser kan desuden
slås op via en thread pool med tre tråde.

Passwords hashes med bcrypt. Registrering giver USER, og login udsteder JWT.
Brugere kan arbejde med egne tickets; SUPPORT og ADMIN kan behandle alle.
Brugeradministration kræver ADMIN. Opretter og kommentarforfatter findes fra
tokenet, ikke fra et userId valgt af klienten.

## Status og tests

Projektets testsuite består af 26 tests:
8 DAO-tests, 1 JSON/DTO-test, 10 user/security REST-tests og 7 ticket-tests,
herunder tre WebSocket-tests. Controller-testene ligger i controllers-pakken.

Supportere og administratorer kan abonnere på nye tickets via /ws/tickets.
Første besked er deres JWT. Efter godkendelse sender serveren CONNECTED;
når en ticket er gemt, sendes TICKET_CREATED og ticketId. Detaljer hentes
gennem REST. Brugere kan også forbinde med eget JWT og modtage notifikationer
om ændringer, svar, tildeling, attachments og sletning på egne tickets.
Andre brugere modtager ikke disse beskeder. Funktionen har endnu ingen
brugergrænseflade og virker i én serverproces.

REST-testene bruger Javalin og Rest Assured med Hamcrest. PostgreSQL startes i
Testcontainers. Tickettestene bruger et fast IP-resultat fra en test-underklasse
af AbuseIpService. Det rigtige AbuseIPDB-kald er ikke verificeret som samlet
ticketforløb. Concurrency er implementeret, men ikke automatisk testet.

## Afgrænsning og næste trin

- Uge 8 med kørbar JAR, Docker og GitHub Actions er endnu ikke implementeret.
- Hibernate bruger stadig create og genskaber data ved opstart.
- Lokal/private IP-kontrol og proxyopsætning kræver yderligere afprøvning.
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
