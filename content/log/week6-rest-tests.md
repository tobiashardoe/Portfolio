---
title: "Uge 6 – REST-tests"
date: 2026-10-21
draft: false
weight: 6
summary: "Endpoint-tests med Rest Assured, Hamcrest og en isoleret database."
---

Opsamling skrevet den 21. oktober 2026 om API-testene i TicketDesk.

## Hvad er implementeret?

Rest Assured sender HTTP-requests til en lokal Javalin-applikation. Hamcrest
kontrollerer statuskoder og JSON-værdier. Testcontainers starter PostgreSQL,
så testene ikke bruger den almindelige udviklingsdatabase.

BeforeAll opretter EntityManagerFactory og starter Javalin. BeforeEach rydder
testdata, og AfterAll lukker ressourcerne. Testene bruger de samme routes og
fejl-handlers som applikationen. Efter uge 7 bruges også security-filteret.

## Hvad testes?

De fem oprindelige UserControllerTest-tests dækker oprettelse, læsning,
opdatering, sletning og afvisning af et tomt navn. De demonstrerer blandt andet
201, 200, 204, 404 og 400. Senere er fem security-tests føjet til samme klasse.

TicketControllerTest kontrollerer scoregrænsen: 79 tillader oprettelse,
80 afviser den, og fejl i IP-opslaget giver 503. Testene henter ticketlisten
bagefter for at kontrollere, om noget faktisk blev gemt. En senere test
kontrollerer også, at en bruger ikke kan læse en anden brugers ticket.

Testen bruger en simpel underklasse af AbuseIpService, der returnerer et kendt
resultat eller kaster en fejl. Det er vores behandling af resultatet, som
testes, ikke det eksterne HTTP-kald. Der bruges ingen API-key eller kvote.

## Forskellen fra DAO-testene

DAO-testene kalder Java-metoder direkte og kontrollerer lagring. REST-testene
går gennem HTTP og afprøver desuden routing, JSON, validering og statuskoder.
Hele suiten bestod med 23 tests den 21. oktober 2026 før den efterfølgende
flytning af TicketControllerTest til controllers-pakken. Efter flytningen er
kompilering kontrolleret.

Der er ikke test af alle endpoints eller alle fejltilfælde. WebSockets og
Webhooks fra ugeplanen er ikke implementeret i ticketsystemet.

