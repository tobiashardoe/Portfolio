---
title: "Uge 7 – Login, bcrypt og JWT"
date: 2026-10-06
draft: false
weight: 7
summary: "Registrering, password-hashing og adgangskontrol til tickets."
---

## Hvad er implementeret?

Registrering og login bruger bcrypt og JWT. USER kan arbejde med egne tickets, SUPPORT/ADMIN med alle tickets, og kun ADMIN kan oprette supportere. Tests kontrollerer login og adgangskontrol. Nye registreringer får altid rollen USER. Passwordhashen skjules i JSON, og tokenets signatur og udløbstid kontrolleres, før den aktuelle bruger og rolle hentes fra databasen.

## Hvorfor denne løsning?

Bcrypt beskytter gemte passwords. JWT identificerer brugeren, mens serveren kontrollerer rolle og ejerskab. JWT_SECRET kan læses fra miljøvariabler eller den lokale config.properties; ved deployment bruges miljøvariabler. Klienten må ikke selv vælge opretter eller kommentarforfatter gennem et bruger-ID. Serveren finder identiteten fra tokenet og kontrollerer den konkrete ticket, så et gyldigt login ikke giver adgang til andres data.

## Hvad har jeg lært?

Jeg har lært forskellen på autentifikation og autorisation. Autentifikation fastslår, hvem brugeren er, mens autorisation afgør, hvad brugeren må gøre. Hashning, tokenkontrol og kontrol af ejerskab løser hver sin del af sikkerheden og kan ikke erstatte hinanden.

## Udfordringer

Rettigheder skal kontrolleres i både REST og WebSockets. Et godkendt WebSocket-login er ikke nok, hvis tokenet senere udløber eller rollen ændres. Hibernate bruger update, så opstart ikke genskaber tabellerne. Passwordreset og token-refresh er endnu ikke implementeret.

## Næste skridt

Klargøre deployment med vedvarende database, Docker og automatiske builds. Miljøvariabler skal levere hemmeligheder uden at lægge dem i koden. Den samlede opsætning skal afprøves med login, adgangskontrol og IP-kontrol, når applikationen kører bag den valgte serveropsætning.
