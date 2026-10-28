---
title: "TicketSystem – API og dokumentation"
draft: false
summary: "Afgrænsning af API'et og dokumentation af de centrale endpoints."
---

## Hvad er implementeret?

API'et har 12 REST-endpoints samt en separat WebSocket-forbindelse.
README i kodeprojektet viser seks centrale endpoints: login, hentning, oprettelse,
opdatering, tildeling og sletning af tickets. Den fulde dokumentation ligger i
`docs/api.md` og beskriver alle endpoints, JSON-formater, adgangskrav og fejl.

## Hvorfor denne løsning?

README viser det centrale ticketforløb uden at gentage alle detaljer.
GET, POST, PUT og DELETE gør CRUD synligt, mens login og tildeling viser
forskellen på at være logget ind og have tilladelse til en handling.
Den fulde dokumentation kan bruges, når en klient skal sende de konkrete requests.

## Hvad har jeg lært?

Et endpoint beskrives ikke kun med sin URL. HTTP-metode, input, svar og statuskoder
skal også fremgå. Et gyldigt token giver ikke adgang til alle handlinger:
en USER må arbejde med egne tickets, mens tildeling kræver SUPPORT eller ADMIN.
Derfor skal adgangskravene også være en del af dokumentationen.

## Udfordringer

Dokumentationen skal følge koden, når funktioner ændres eller fjernes.
Generel brugeradministration, attachment-endpoints og health-endpointet er ikke
længere udstillet. Attachment findes stadig i datamodellen, men det betyder ikke,
at der er et endpoint til filupload.

## Næste skridt

Gennemgå eksemplerne i HTTP-filen og kontrollere statuskoder og JSON-svar mod
dokumentationen. Ved deployment skal den offentlige baseadresse tilføjes, og
IP-kontrollen skal afprøves bag Caddy.
