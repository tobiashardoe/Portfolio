---
title: "Uge 3 – API, JSON og DTO"
date: 2026-09-01
draft: false
weight: 3
summary: "AbuseIPDB-integration og konvertering af JSON til en DTO."
---

## Hvad er implementeret?

AbuseIpService henter IP-data fra AbuseIPDB. Jackson mapper JSON til en DTO, og en test kontrollerer mappingen. Kaldet bruger Java HttpClient og en API-key. Svaret skal have status 200, før Jackson omsætter det til AbuseIpResponseDTO med et indlejret Data-objekt.

## Hvorfor denne løsning?

IP-data bruges ved ticketoprettelse. DTO'en adskiller API-svaret fra databaseentiteterne. En separat service samler kommunikationen med AbuseIPDB, så controlleren ikke skal kende detaljerne i HTTP-kaldet. DTO'en indeholder de felter, projektet skal bruge, eksempelvis abuseConfidenceScore.

## Hvad har jeg lært?

Jeg har lært at hente API-data og omsætte JSON til Java-objekter. Jeg har arbejdet med ObjectMapper og med at forbinde et JSON-feltnavn til et andet Java-feltnavn gennem JsonProperty. Det gør det muligt at læse værdierne gennem almindelige Java-metoder frem for manuelt at gennemgå JSON.

## Udfordringer

DTO'en skal passe til JSON-strukturen og feltnavne som isTor. Testen dækker ikke det eksterne kald. Et svar kan have korrekt JSON-format og stadig mangle de forventede data. Derfor skal både HTTP-status og indhold vurderes, mens mapping-testen holdes uafhængig af API-key og netværk.

## Næste skridt

Afprøve det rigtige API-kald og arbejde med samtidige opslag. Det skal kontrolleres, at både et gyldigt svar og et fejlet opslag håndteres korrekt. Derefter kan flere IP-adresser behandles samlet uden at vente på hvert opslag ét ad gangen.
