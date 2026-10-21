---
title: "Uge 2 – Oprettelse af entiteter og relationer"
date: 2026-08-25
draft: false
weight: 2
---

## Hvad er implementeret?

ManyToOne forbinder tickets med opretter og supporter, kommentarer med ticket og forfatter samt attachments med ticket. DAO-tests kontrollerer også relationerne. TicketDAO-testene kontrollerer blandt andet, at opretteren bevares, når en ticket gemmes, og at en supporter kan tilknyttes ved opdatering. Ved sletning af ticketen skal opretteren stadig eksistere.

## Hvorfor denne løsning?

Relationerne følger ticketsystemets behov uden at tilføje unødvendige relationstyper. Flere tickets kan tilhøre samme bruger, hvilket gør ManyToOne passende. Relationerne peger kun i den retning, som opslagene kræver, så modellen ikke får ekstra samlinger og vedligeholdelse uden et konkret behov.

## Hvad har jeg lært?

Jeg har lært at opbygge JPA-relationer og planlægge databasen med et ERD. ERD'et gør det lettere at se forbindelserne, før de omsættes til kode. Jeg har også arbejdet med, at sletning af én entitet ikke automatisk bør betyde sletning af de objekter, den peger på.

## Udfordringer

Jeg begyndte på relationerne før ERD'et og måtte ændre koden. Cascade var også svært at forstå. Det var især vigtigt at skelne mellem en forbindelse i databasen og ejerskab over et objekts levetid. En ticket bruger eksempelvis en bruger som opretter, men skal ikke kunne slette brugeren sammen med ticketen.

## Næste skridt

Arbejde med API, JSON og DTO'er. Det eksterne svar skal kunne bruges som Java-objekter uden at blive blandet sammen med databasemodellen. IP-oplysningerne skal senere indgå i kontrollen ved ticketoprettelse.
