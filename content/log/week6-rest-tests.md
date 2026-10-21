---
title: "Uge 6 – REST-tests"
date: 2026-10-21
draft: false
weight: 6
summary: "Endpoint-tests med Rest Assured, Hamcrest og en isoleret database."
---

## Hvad er implementeret?

Rest Assured og Testcontainers tester CRUD, validering og adgangskontrol. WebSockets giver support/admin besked om nye tickets og ejeren besked om ændringer og svar. Projektet har 26 tests, herunder tre WebSocket-tests. REST-testene kontrollerer statuskoder, JSON og lagrede data. WebSocket-beskeder indeholder kun type og ticketId, og tokenet kontrolleres, før en notifikation sendes til de relevante modtagere.

## Hvorfor denne løsning?

HTTP-tests kontrollerer flere lag sammen. Faste IP-resultater undgår API-afhængighed, og WebSockets sender beskeder uden løbende forespørgsler. Testcontainers holder testdata adskilt fra udviklingsdatabasen. Det faste IP-resultat gør scoregrænsen reproducerbar, mens beskeder med kun et ticketId lader klienten hente detaljer gennem REST med adgangskontrol.

## Hvad har jeg lært?

Jeg har lært forskellen på DAO- og HTTP-tests og at vente på asynkrone beskeder. DAO-testene kalder metoder direkte, mens REST-testene også afprøver routing og JSON. Ved WebSockets kommer resultatet som en senere besked, så testen må vente med en tidsgrænse i stedet for at læse et direkte retursvar.

## Udfordringer

Testdata og forbindelser skal ryddes op, og beskeder må kun nå de rette brugere. Offlinebeskeder og frontend er ikke implementeret. Testene kontrollerer derfor også, at en anden bruger ikke får ejerens beskeder. Notifikationerne virker kun for tilsluttede klienter og er ikke en historik, der kan hentes igen efter en afbrudt forbindelse.

## Næste skridt

Arbejde med login og roller samt afprøve notifikationer fra en brugergrænseflade. Adgangen skal skelne mellem en almindelig bruger, support og administrator. En kommende brugergrænseflade skal kunne omsætte notifikationerne til synlige beskeder og hente opdaterede ticketdata.
