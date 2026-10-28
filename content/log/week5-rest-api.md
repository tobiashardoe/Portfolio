---
title: "Uge 5 – REST API og validering"
date: 2026-09-22
draft: false
weight: 5
summary: "Javalin-routes, HTTP-statuskoder, validering og fælles fejlhåndtering."
---

## Hvad er implementeret?

Routes-klassen samler de 12 REST-endpoints og forbinder dem med controllerne. API'et giver CRUD, inputvalidering og JSON-fejl. IP-score på mindst 80 giver 403, og fejl i opslaget giver 503. Ingen af delene gemmer en ticket. Der kontrolleres blandt andet positive ID'er, påkrævet tekst og tilladte statusser og prioriteter. Fejl returneres som JSON, mens uventede interne fejl logges uden at sende detaljer til klienten.

## Hvorfor denne løsning?

Controllerne håndterer HTTP, og DAO'erne håndterer databasen. Fælles fejlhåndtering giver ensartede svar. IP-kontrollen udføres før lagring, så en afvist request ikke efterlader data. Scoregrænsen er en projektregel, ikke en garanti for, at en bestemt bruger er skadelig.

## Hvad har jeg lært?

Jeg har lært at validere input og bruge passende HTTP-statuskoder. Jeg har arbejdet med forskellen på 400 ved ugyldigt input, 404 ved ukendte ressourcer og 500 ved uventede fejl. Validering handler om inputtets form og indhold, mens adgangskontrol handler om, hvem der må udføre handlingen.

## Udfordringer

Lokale IP-adresser og proxyopsætning kan påvirke IP-kontrollen. Attachments er kun filoplysninger, ikke filupload. Ved en proxy kan forbindelsens IP være en anden end slutbrugerens, og den konkrete opsætning skal derfor afprøves. Attachment findes kun i datamodellen og DAO'en; der er ikke attachment-endpoints eller filupload.

## Næste skridt

Teste endpoints gennem HTTP. Testene skal kontrollere både statuskoder, JSON-svar og om data faktisk er gemt. Ticketoprettelse skal testes med accepteret score, afvist score og fejl i IP-opslaget.
