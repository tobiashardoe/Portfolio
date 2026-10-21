---
title: "Uge 3 – API, JSON og DTO"
date: 2026-10-21
draft: false
weight: 3
summary: "AbuseIPDB-integration og konvertering af JSON til en DTO."
---

## Hvad er implementeret?

AbuseIpService henter IP-data fra AbuseIPDB. Jackson mapper JSON til en DTO, og en test kontrollerer mappingen.

## Hvorfor denne løsning?

IP-data bruges ved ticketoprettelse. DTO'en adskiller API-svaret fra databaseentiteterne.

## Hvad har jeg lært?

Jeg har lært at hente API-data og omsætte JSON til Java-objekter.

## Udfordringer

DTO'en skal passe til JSON-strukturen og feltnavne som isTor. Testen dækker ikke det eksterne kald.

## Næste skridt

Afprøve det rigtige API-kald og arbejde med samtidige opslag.
