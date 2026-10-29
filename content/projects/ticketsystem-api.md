---
title: "TicketSystem – API og dokumentation"
draft: false
summary: "Afgrænsning af API'et og dokumentation af de centrale endpoints."
---

## Hvad er implementeret?

API'et har 12 REST-endpoints samt en separat WebSocket-forbindelse.
README i kodeprojektet viser seks centrale endpoints: login, hentning, oprettelse,
opdatering, tildeling og sletning af tickets. Den fulde dokumentation ligger i
[docs/api.md](https://github.com/tobiashardoe/ticketsystem/blob/main/docs/api.md)
og beskriver alle endpoints, JSON-formater, adgangskrav og fejl.
Online baseadresse er `https://ticketsystem.tobiashardo.dk/api`.

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

Holde HTTP-eksemplerne og dokumentationen samlet om de samme felter og statuskoder.
Login, CRUD og centrale roller er manuelt afprøvet online. Det næste er at afklare,
hvordan den oprindelige klient-IP kontrolleres bag Caddy, uden at stole på
vilkårlige IP-headers fra klienten.
