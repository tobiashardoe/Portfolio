---
title: "Uge 7 – Login, bcrypt og JWT"
date: 2026-10-21
draft: false
weight: 7
summary: "Registrering, password-hashing og adgangskontrol til tickets."
---

Opsamling skrevet den 21. oktober 2026 om security-delen i TicketDesk.

## Hvad er implementeret?

SecurityController har POST /api/auth/register og POST /api/auth/login.
LoginDTO indeholder navn, e-mail og password. Registrering kontrollerer
input og eksisterende e-mail, hasher passwordet og opretter altid en USER.
User.setPassword() bruger jBCrypt, og verifyPassword() bruger BCrypt.checkpw().
passwordHash skjules i JSON og toString().

Undervisningens TokenSecurity-bibliotek opretter JWT med én times gyldighed.
Tokenet sendes som Authorization: Bearer i efterfølgende requests. Et before-filter
kontrollerer signatur og udløbstid og tilføjer den aktuelle bruger til Context.
Brugeren og rollen genindlæses fra databasen, så serveren anvender den aktuelle rolle.

## Adgang til systemet

- USER kan arbejde med egne tickets og deres kommentarer og attachments.
- SUPPORT og ADMIN kan behandle alle tickets og tildele en supporter.
- Kun ADMIN kan administrere brugere og oprette supportere.
- Registrering og login samt health-endpointet er offentlige.
- Det separate IP-opslag kræver SUPPORT eller ADMIN.

Opretter og kommentarforfatter findes fra tokenet. userId og adminId i
query-parametre bestemmer ikke længere, hvem der udfører handlingen.
Projektet beholder én rolle pr. bruger frem for at indføre en Role-entitet med
ManyToMany. Denne del af undervisningens eksempel er dermed ikke implementeret.

## Konfiguration og tests

JWT_SECRET kommer fra en miljøvariabel og skal være mindst 32 bytes.
Første administrator kan oprettes fra ADMIN_EMAIL og ADMIN_PASSWORD ved
opstart. Der er ingen indbygget standardadgangskode. Hibernate bruger dog
stadig create, så persistens ved genstart skal løses i deployment-delen.

Testene kontrollerer registrering/login, hashning og skjulte passwordfelter,
forkert password, manglende/ugyldigt/udløbet token, brugerens manglende
adminadgang og en administrators oprettelse af en supporter, der kan logge ind.
Tickettesten kontrollerer også adgang til en anden brugers ticket.

Passwordændring/reset og token-refresh er ikke implementeret. Løsningen er en
undervisningsimplementering og ikke en fuld gennemgang af alle sikkerhedsrisici.

