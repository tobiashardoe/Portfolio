---
title: "Uge 5 – REST API og validering"
date: 2026-10-21
draft: false
weight: 5
summary: "Javalin-routes, HTTP-statuskoder, validering og fælles fejlhåndtering."
---

## Hvad er implementeret?

Javalin-routes giver CRUD, inputvalidering og JSON-fejl. IP-score på mindst 80 giver 403, og fejl i opslaget giver 503. Ingen af delene gemmer en ticket.

## Hvorfor denne løsning?

Controllerne håndterer HTTP, og DAO'erne håndterer databasen. Fælles fejlhåndtering giver ensartede svar.

## Hvad har jeg lært?

Jeg har lært at validere input og bruge passende HTTP-statuskoder.

## Udfordringer

Lokale IP-adresser og proxyopsætning kan påvirke IP-kontrollen. Attachments er kun filoplysninger, ikke filupload.

## Næste skridt

Teste endpoints gennem HTTP.
