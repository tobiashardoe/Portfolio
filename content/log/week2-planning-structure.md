---
title: "Uge 2 – Oprettelse af entitetet og relation"
date: 2026-08-25
draft: false
weight: 2
---
Denne uge har jeg planlagt strukturen for **TicketDesk** og arbejdet med de første entiteter og relationer i projektet. Fokus har været på at få et godt fundament i `JPA`, så databasen kan understøtte de funktioner, der skal bygges videre på senere i semesteret.

## Hvad har jeg lavet?

* Oprettet projektet i IntelliJ og sat `JPA` op.
* Oprettet en database, som `Hibernate` kan kommunikere med.
* Lavet de første entiteter, der skal bruges i applikationen.
* Begyndt at implementere `CRUD`-funktionalitet i `DAO`-laget.

## Hvorfor denne løsning?

Jeg valgte at starte med entiteter og relationer, fordi de danner fundamentet for resten af systemet. Når databasen er struktureret rigtigt fra starten, bliver det lettere senere at tilføje funktioner som brugerroller, `DTO'er`, `JSON`-kommunikation og autentifikation.

## Udfordringer

Den største udfordring var at finde ud af, hvilke entiteter der skulle oprettes, og hvor jeg skulle starte.

Jeg havde også udfordringer med at forstå Cascade og, hvornår de forskellige cascade-typer giver mening i `JPA`.

En anden fejl var, at jeg begyndte at implementere relationer, før jeg havde lavet et `ERD`. Det betød, at jeg flere gange måtte gå tilbage og ændre i koden, hvilket kostede tid.

## Hvad har jeg lært?

* Hvordan relationer mellem entiteter opbygges i `JPA`.
* Hvordan et `DAO`-lag struktureres med `CRUD`-funktionalitet.
* At det betaler sig at lave et `ERD` og planlægge databasen, før man begynder at kode.

## Næste skridt

Næste skridt er at koble backenden sammen med eksterne data ved at arbejde med `JSON`, `DTO'er` og hente data fra et `API`.

## Statusopdatering – 21. oktober 2026

Den nuværende model har fem ManyToOne-relationer: Ticket til opretter og supporter,
Comment til ticket og forfatter samt Attachment til ticket. Relationerne er
unidirectional. Ticket bruger JoinColumn til at navngive fremmednøglerne.

TicketDAOTest kontrollerer, at opretteren bevares ved lagring, at en supporter kan
tilknyttes ved opdatering, og at sletning af en ticket bevarer opretteren.
Vi har valgt ikke at tilføje OneToOne eller ManyToMany uden et konkret behov i
ticketsystemet. Modellen har heller ikke en eksplicit OneToMany-side, cascade eller
orphanRemoval. Det er pensum, men ikke demonstreret i den aktuelle kode.

[Ugens læringsmål](https://3sem.kursusmaterialer.dk/backend/jpa-relations/learning-objectives/)
