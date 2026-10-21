---
title: "Uge 1 – Valg af projekt og opsætning"
date: 2026-08-18
draft: false
weight: 1
---

Denne uge har jeg valgt at skulle udvikle et ticketsystem,
**TicketDesk**. Formålet med systemet er at give 
brugere mulighed for at indsende en ticket, 
når de har et spørgsmål, oplever en fejl 
eller har brug for hjælp, mens supportmedarbejdere 
senere skal kunne administrere og løse disse henvendelser.

## Hvad har jeg lavet?

* Valgt projektidéen TicketDesk.
* Sat portfolioen op med Hugo og Blowfish.
* Konfigureret GitHub Pages og GitHub Actions, 
så siden bliver deployet automatisk.

## Hvorfor denne idé?

Jeg valgte et ticketsystem, fordi det giver en god 
mulighed for at bygge videre på projektet gennem 
hele semesteret. Systemet kan starte som en simpel 
CRUD-applikation og senere udvides med blandt andet 
brugerroller, autentifikation, relationer i databasen 
og andre funktioner, efterhånden som vi lærer nye 
teknologier.

## Udfordringer

Den største udfordring var opsætningen af portfolioen. 
Jeg stødte blandt andet på problemer med Git-branches 
(`master` og `main`), deployment via GitHub Actions 
og konfigurationen af Hugo, før siden fungerede korrekt.

## Hvad har jeg lært?

Jeg har fået en bedre forståelse af, 
hvordan GitHub Pages fungerer sammen med 
GitHub Actions, og hvordan Hugo er opbygget med 
konfigurationsfiler og Markdown-indhold.

## Næste skridt

Næste skridt er at begynde udviklingen af 
TicketDesk ved at designe de første entiteter og 
implementere den grundlæggende CRUD-funktionalitet.

## Statusopdatering – 21. oktober 2026

Projektet hedder nu TicketDesk. Den grundlæggende JPA-del er implementeret
med User, Ticket, Comment og Attachment. Entiteterne har automatisk ID-generering,
og DAO-klasserne bruger persist, find, merge og remove samt JPQL til opslag.
UserDAO og TicketDAO har hver fire tests for lagring/læsning, findAll, update og
delete mod PostgreSQL i Testcontainers. Testene er tilføjet senere i forløbet.

Dette demonstrerer CRUD og DAO-arkitektur. Named queries, aggregatefunktioner og
JPA lifecycle-callbacks fra ugens læringsmål er ikke implementeret i projektet.

[Ugens læringsmål](https://3sem.kursusmaterialer.dk/backend/jpa-basics/learning-objectives/)
