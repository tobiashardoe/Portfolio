---
title: "Uge 4 – Concurrency"
date: 2026-10-21
draft: false
weight: 4
summary: "Samtidige IP-opslag med ExecutorService, Callable og Future."
---

## Hvad er implementeret?

checkMultiple() udfører IP-opslag med tre tråde. Callable returnerer resultater gennem Future, og puljen lukkes i finally.

## Hvorfor denne løsning?

Opslag kan køre samtidig, mens de venter på netværket.

## Hvad har jeg lært?

Jeg har lært at bruge ExecutorService, Callable og Future.

## Udfordringer

Ét fejlet opslag får hele metoden til at fejle. Der er ingen eksplicit timeout eller automatisk concurrency-test.

## Næste skridt

Arbejde med REST-endpoints og validering.
