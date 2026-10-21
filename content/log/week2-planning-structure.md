---
title: "Uge 2 – Oprettelse af entiteter og relationer"
date: 2026-08-25
draft: false
weight: 2
---

## Hvad er implementeret?

ManyToOne forbinder tickets med opretter og supporter, kommentarer med ticket og forfatter samt attachments med ticket. DAO-tests kontrollerer også relationerne.

## Hvorfor denne løsning?

Relationerne følger ticketsystemets behov uden at tilføje unødvendige relationstyper.

## Hvad har jeg lært?

Jeg har lært at opbygge JPA-relationer og planlægge databasen med et ERD.

## Udfordringer

Jeg begyndte på relationerne før ERD'et og måtte ændre koden. Cascade var også svært at forstå.

## Næste skridt

Arbejde med API, JSON og DTO'er.
