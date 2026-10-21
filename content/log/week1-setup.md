---
title: "Uge 1 – Valg af projekt og opsætning"
date: 2026-08-18
draft: false
weight: 1
---

## Hvad er implementeret?

Portfolioen er sat op med Hugo og GitHub Pages. JPA-entiteter og DAO-klasser giver CRUD, og UserDAO og TicketDAO har otte tests. Entiteterne User, Ticket, Comment og Attachment gemmes med JPA/Hibernate. DAO-testene kontrollerer lagring, læsning, opdatering og sletning mod PostgreSQL i Testcontainers.

## Hvorfor denne løsning?

Ticketsystemet kan udvides gradvist. DAO-laget holder databasearbejdet samlet. En bruger kan starte med at oprette en henvendelse, mens support senere kan behandle den. Det giver en konkret sammenhæng mellem datamodellen og de funktioner, der bygges ovenpå.

## Hvad har jeg lært?

Jeg har lært om JPA, CRUD og opsætning af Hugo og GitHub Pages. Jeg har fået en bedre forståelse af, hvordan Java-objekter gemmes i en database, og hvorfor databasekoden placeres i egne klasser. Portfolioens opsætning har samtidig givet erfaring med Markdown og automatisk publicering.

## Udfordringer

Branches og Hugo-konfiguration gav problemer under opsætningen. Problemer med master og main samt GitHub Actions gjorde det nødvendigt at gennemgå opsætningen. Det viste, at projektets konfiguration også skal være på plads, før indholdet kan vises korrekt.

## Næste skridt

Arbejde videre med entiteter og relationer. Her skal det afklares, hvordan en ticket forbindes med både opretter og supporter. Datamodellen skal også kunne rumme kommentarer og attachments uden at blande deres ansvar sammen.
