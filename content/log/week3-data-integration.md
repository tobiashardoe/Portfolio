---
title: "Uge 3 – API, JSON og DTO"
date: 2026-10-21
draft: false
weight: 3
summary: "AbuseIPDB-integration og konvertering af JSON til en DTO."
---

Dette indlæg er en opsamling skrevet den 21. oktober 2026. Det beskriver den
nuværende implementering af uge 3's emner i TicketDesk.

## Hvad er implementeret?

AbuseIpService henter IP-oplysninger fra AbuseIPDB med Java HttpClient.
Requesten er en GET med API-key og Accept: application/json. HTTP-status skal
være 200, før svaret bruges. Jacksons ObjectMapper.readValue() omdanner bodyen
fra JSON til AbuseIpResponseDTO.

DTO'en indeholder et Data-objekt med blandt andet ipAddress,
abuseConfidenceScore, countryCode og totalReports. JsonProperty mapper isTor
til Java-feltet tor. JsonIgnoreProperties gør, at ekstra felter ignoreres.
Controlleren returnerer Java-objekterne som JSON med ctx.json().

## Hvorfor denne løsning?

Et IP-opslag har en konkret funktion i ticketsystemet: resultatet kan bruges
til at vurdere ticketoprettelse. En DTO adskiller det eksterne svar fra vores
databaseentiteter og giver adgang til data gennem almindelige Java-metoder.

## Test og afgrænsning

AbuseIpResponseDTOTest konverterer eksempel-JSON og kontrollerer alle de
udvalgte feltværdier, inklusive isTor. Testen kræver ingen API-key eller Docker.
En tidligere test med en lokal HttpServer blev fjernet for at følge en enklere
tilgang tættere på undervisningens JSON/DTO-eksempler.

Det er mappingen, der er automatisk testet. Den rigtige API-key og det eksterne
kald skal afprøves separat.
