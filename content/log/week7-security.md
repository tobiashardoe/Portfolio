---
title: "Uge 7 – Login, bcrypt og JWT"
date: 2026-10-21
draft: false
weight: 7
summary: "Registrering, password-hashing og adgangskontrol til tickets."
---

## Hvad er implementeret?

Registrering og login bruger bcrypt og JWT. USER kan arbejde med egne tickets, SUPPORT/ADMIN med alle tickets, og kun ADMIN kan administrere brugere. Tests kontrollerer login og adgangskontrol.

## Hvorfor denne løsning?

Bcrypt beskytter gemte passwords. JWT identificerer brugeren, mens serveren kontrollerer rolle og ejerskab. Hemmeligheder hentes fra miljøvariabler.

## Hvad har jeg lært?

Jeg har lært forskellen på autentifikation og autorisation.

## Udfordringer

Rettigheder skal kontrolleres i både REST og WebSockets. Hibernate bruger stadig create, så data ikke bevares ved genstart.

## Næste skridt

Klargøre deployment med vedvarende database, Docker og automatiske builds.
