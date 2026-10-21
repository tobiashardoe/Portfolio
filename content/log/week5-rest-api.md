---
title: "Uge 5 – REST API og validering"
date: 2026-10-21
draft: false
weight: 5
summary: "Javalin-routes, HTTP-statuskoder, validering og fælles fejlhåndtering."
---

Opsamling skrevet den 21. oktober 2026 om REST-delen i TicketDesk.

## Hvad er implementeret?

UserController og TicketController har routes med GET, POST, PUT og DELETE.
Context bruges til at læse path-parametre, query-parametre og JSON-body samt
returnere svar. Controllerne kalder DAO'erne til databasearbejdet.

ID'er læses med pathParamAsClass()/queryParamAsClass() og skal være positive.
bodyValidator() bruges ved JSON-input. Simple betingelser kontrollerer
påkrævet tekst, e-mailformat og tilladte roller, statusser og prioriteter.
Statusværdierne er OPEN, IN_PROGRESS og CLOSED; prioriteterne er LOW, MEDIUM
og HIGH. Valideringsregler er ikke det samme som adgangskontrol.

ApiException og fælles exception-handlers i Main giver fejl som JSON.
Eksempler er 400 ved ugyldigt input, 404 ved en ukendt bruger/ticket og 500
ved uventede fejl. Uventede fejl logges uden at sende interne detaljer til
klienten. Et before-filter logger requestens metode og sti.

## Ticketoprettelse og IP-kontrol

Før save() kaldes, hentes forbindelsens IP, som slås op i AbuseIPDB. En score
på mindst 80 giver 403. Fejl i opslaget giver 503. I begge tilfælde gemmes
ticketen ikke. En accepteret oprettelse giver 201. Grænsen er vores egen
projektregel og er ikke et sikkert bevis på, at en bruger er skadelig.

Efter security-delen findes opretteren fra JWT, så klienten ikke vælger userId.
Det samme gælder kommentarens forfatter. Ticketens titel eller beskrivelse
sendes ikke til AbuseIPDB.

## Afgrænsning

Databaserelaterede slettefejl er ikke særskilt oversat til eksempelvis 409.
Lokale/private IP-adresser kan give 503. Bag en proxy skal klient-IP håndteres
ud fra den konkrete opsætning. Der er ikke implementeret filupload; attachments
indeholder filoplysninger og en sti.

