---
title: "Uge 4 – Concurrency"
date: 2026-09-08
draft: false
weight: 4
summary: "Samtidige IP-opslag med ExecutorService, Callable og Future."
---

## Hvad er implementeret?

checkMultiple() udfører IP-opslag med tre tråde. Callable returnerer resultater gennem Future, og puljen lukkes i finally. Alle opgaver indsendes først, hvorefter resultaterne hentes i samme rækkefølge som inputlisten. Funktionen bruges på endpointet til flere IP-adresser; almindelig ticketoprettelse laver kun ét opslag.

## Hvorfor denne løsning?

Opslag kan køre samtidig, mens de venter på netværket. HTTP-kald bruger en del af tiden på at vente, og trådene kan derfor overlappe denne ventetid. En fast pulje holder antallet af samtidige opgaver pr. metodekald begrænset i stedet for at oprette en tråd til hver adresse.

## Hvad har jeg lært?

Jeg har lært at bruge ExecutorService, Callable og Future. Future.get() venter på et resultat, men de øvrige indsendte opgaver kan stadig køre imens. Jeg har også arbejdet med, at shutdown() lukker for nye opgaver uden at afbryde dem, der allerede er indsendt.

## Udfordringer

Ét fejlet opslag får hele metoden til at fejle. Der er ingen eksplicit timeout eller automatisk concurrency-test. Tre tråde er ikke det samme som en grænse for requests pr. sekund eller for hele serveren. Det er derfor nødvendigt at skelne mellem parallel behandling, API-kvote og håndtering af ventetid.

## Næste skridt

Arbejde med REST-endpoints og validering. API'et skal give klienten tydelige svar ved både succes og fejl. Samtidig skal det være klart, hvilke input der accepteres, før databaseoperationerne kaldes.
