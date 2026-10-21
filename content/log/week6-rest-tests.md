---
title: "Uge 6 – REST-tests"
date: 2026-10-21
draft: false
weight: 6
summary: "Endpoint-tests med Rest Assured, Hamcrest og en isoleret database."
---

## Hvad er implementeret?

Rest Assured og Testcontainers tester CRUD, validering og adgangskontrol. WebSockets giver support/admin besked om nye tickets og ejeren besked om ændringer og svar. Projektet har 26 tests, herunder tre WebSocket-tests.

## Hvorfor denne løsning?

HTTP-tests kontrollerer flere lag sammen. Faste IP-resultater undgår API-afhængighed, og WebSockets sender beskeder uden løbende forespørgsler.

## Hvad har jeg lært?

Jeg har lært forskellen på DAO- og HTTP-tests og at vente på asynkrone beskeder.

## Udfordringer

Testdata og forbindelser skal ryddes op, og beskeder må kun nå de rette brugere. Offlinebeskeder og frontend er ikke implementeret.

## Næste skridt

Arbejde med login og roller samt afprøve notifikationer fra en brugergrænseflade.
