---
title: "TicketSystem – API"
draft: false
summary: "Endpoints, adgangskrav, JSON-formater og fejl i TicketSystem."
---

## Adresse og adgang

Lokal server: `http://localhost:7070`. REST-endpoints starter med `/api`.
Eksemplerne beskriver den aktuelle backend; subdomænet er ikke dokumentation for et færdigt deployment.

JSON-input sendes med `Content-Type: application/json`. Beskyttede endpoints kræver:

```http
Authorization: Bearer <token-fra-login>
```

JWT gælder i én time. Serveren kontrollerer tokenet og henter brugerens aktuelle rolle fra databasen.
USER har adgang til egne tickets. SUPPORT og ADMIN har adgang til alle tickets.
Kun ADMIN kan administrere brugere og oprette supportere. Adgang til en ticket
omfatter også dens kommentarer og attachments. Alle ID-parametre skal være positive heltal.

## Login og registrering

| Metode | Endpoint | Adgang | JSON-input | Succes | Fejl |
|---|---|---|---|---|---|
| POST | `/api/auth/register` | Offentlig | name, email, password | 201: token og User | 400: ugyldigt input; 409: e-mail findes |
| POST | `/api/auth/login` | Offentlig | email, password | 200: token og User | 400: manglende input; 401: forkert login |
| GET | `/api/health` | Offentlig | Ingen | 200: teksten `API is running` | 500 ved uventet serverfejl |

Registrering kræver navn, e-mail med gyldigt format og password på mindst otte tegn
og højst 72 UTF-8-bytes. Rollen bliver altid USER, selv hvis klienten sender en anden rolle.

Registrering:

```json
{
  "name": "Demo User",
  "email": "demo@example.com",
  "password": "Demo-password-2026!"
}
```

Login bruger samme email og password, men kræver ikke name. Begge endpoints returnerer:

```json
{
  "token": "<JWT>",
  "user": {
    "id": 1,
    "name": "Demo User",
    "email": "demo@example.com",
    "role": "USER"
  }
}
```

Password og passwordhash returneres ikke. Eksemplets password er kun demonstrationsdata.

## Tickets

"Ejer/staff" betyder ticketens opretter, SUPPORT eller ADMIN.
Alle endpoints kan desuden returnere de fælles fejl beskrevet nederst.

| Metode | Endpoint | Adgang | Input | Succes | Særlige fejl |
|---|---|---|---|---|---|
| GET | `/api/tickets` | USER, SUPPORT, ADMIN | Ingen | 200: liste af Ticket; USER får kun egne | 401: ugyldigt login |
| GET | `/api/tickets/{id}` | Ejer/staff | id i sti | 200: Ticket | 403: ikke ejer/staff; 404: ukendt ticket |
| POST | `/api/tickets` | USER, SUPPORT, ADMIN | Ticket-input nedenfor | 201: gemt Ticket | 400: ugyldigt input; 403: IP-score mindst 80; 503: IP-kontrol fejler |
| PUT | `/api/tickets/{id}` | Ejer/staff | id og alle fire Ticket-inputfelter | 200: opdateret Ticket | 400, 403, 404 |
| DELETE | `/api/tickets/{id}` | Ejer/staff | id i sti | 204: ingen body | 403, 404; 500 ved databasekonflikt |
| PUT | `/api/tickets/{id}/assign?supporterId={supporterId}` | SUPPORT, ADMIN | ticket-id og supporterId som query-parameter; ingen body | 200: Ticket med assignedTo og status IN_PROGRESS | 400: ugyldigt ID eller modtager ikke SUPPORT; 403: USER; 404: ticket/bruger mangler |

Ticket-input til POST og PUT:

```json
{
  "title": "Kan ikke logge ind",
  "description": "Login viser en fejlmeddelelse.",
  "status": "OPEN",
  "priority": "MEDIUM"
}
```

Alle fire felter er påkrævede. Tilladte statusser er OPEN, IN_PROGRESS og CLOSED;
prioriteterne er LOW, MEDIUM og HIGH. PUT erstatter disse fire værdier og er ikke en delvis opdatering.
Ved POST findes createdBy fra JWT. Klienten vælger ikke opretter gennem userId.
Tildeling foretages på det separate assign-endpoint.

Et Ticket-svar kan se sådan ud:

```json
{
  "id": 42,
  "title": "Kan ikke logge ind",
  "description": "Login viser en fejlmeddelelse.",
  "status": "OPEN",
  "priority": "MEDIUM",
  "createdBy": {
    "id": 1,
    "name": "Demo User",
    "email": "demo@example.com",
    "role": "USER"
  },
  "assignedTo": null
}
```

assignedTo bliver et User-objekt ved tildeling. Tomme lister returneres som `[]`.
IP-kontrollen bruger forbindelsens adresse, ikke et felt i JSON. Ved afvisning gemmes ticketen ikke.
Lokale IP-adresser og serverens proxyopsætning kan påvirke opslaget.

## Kommentarer og attachments

| Metode | Endpoint | Adgang | JSON-input | Succes | Særlige fejl |
|---|---|---|---|---|---|
| GET | `/api/tickets/{id}/comments` | Ejer/staff | Ingen | 200: liste af Comment | 400, 403, 404 |
| POST | `/api/tickets/{id}/comments` | Ejer/staff | message, ikke tom | 201: Comment | 400, 403, 404 |
| GET | `/api/tickets/{id}/attachments` | Ejer/staff | Ingen | 200: liste af Attachment | 400, 403, 404 |
| POST | `/api/tickets/{id}/attachments` | Ejer/staff | fileName, fileType, filePath; alle udfyldt | 201: Attachment | 400, 403, 404 |

Kommentar-input:

```json
{"message": "Vi undersøger problemet."}
```

Comment-svaret indeholder id, message, ticket (Ticket-objektet ovenfor) og author (User-objekt).
Forfatteren findes fra JWT, mens ticketen findes fra URL'en.

Attachment-input:

```json
{
  "fileName": "fejl.png",
  "fileType": "image/png",
  "filePath": "/files/fejl.png"
}
```

Attachment-svaret indeholder id, fileName, fileType, filePath og ticket (Ticket-objekt).
Der gemmes kun metadata. Endpointet uploader ikke filer og kontrollerer ikke, at stien findes.
Der findes ikke selvstændige PUT/DELETE-endpoints til kommentarer eller attachments.

## Brugeradministration

Alle endpoints i denne tabel kræver ADMIN. USER og SUPPORT afvises med 403.

| Metode | Endpoint | JSON-input | Succes | Særlige fejl |
|---|---|---|---|---|
| GET | `/api/users` | Ingen | 200: liste af User | Fælles fejl |
| GET | `/api/users/{id}` | Ingen | 200: User | 400: ugyldigt ID; 404: bruger mangler |
| POST | `/api/users` | name, email; valgfrit role og password | 201: User; role er USER hvis udeladt | 400: ugyldigt input; 409: e-mail findes |
| PUT | `/api/users/{id}` | name, email, role | 200: User | 400: ugyldigt input; 404: bruger mangler; 409: e-mail optaget |
| DELETE | `/api/users/{id}` | Ingen | 204: ingen body | 400, 404; 500 ved databasekonflikt |
| POST | `/api/supporters` | name, email, password | 201: User med rollen SUPPORT | 400: manglende felter; 409: e-mail findes |

User-formatet er id, name, email og role som i login-svaret. Rollen skal være USER, SUPPORT eller ADMIN.
Navn og e-mail er påkrævede, og e-mailen skal have gyldigt format og være unik.

Oprettelse af supporter:

```json
{
  "name": "Supporter",
  "email": "support@example.com",
  "password": "Support-password-2026!"
}
```

På `/api/users` er password valgfrit; uden det kan den oprettede bruger ikke logge ind.
Brug almindelig registrering til oprettelse af en loginbruger, eller angiv password ved adminoprettelse.
Passwordet skal overholde bcrypt-reglen på 8 tegn til 72 UTF-8-bytes.
Den nuværende adminoprettelse kan returnere 500 ved brud på passwordreglen,
fordi valideringen sker i entitetens setter. Passwordændring og reset har ingen særskilte endpoints.
PUT ændrer navn, e-mail og rolle, ikke password.

## IP-opslag

| Metode | Endpoint | Adgang | JSON-input | Succes | Særlige fejl |
|---|---|---|---|---|---|
| POST | `/api/ip/check` | SUPPORT, ADMIN | Liste med 1–20 ikke-tomme IP-adresser | 200: liste af AbuseIpResponseDTO i inputrækkefølge | 400: ugyldigt input; 403: USER; 503: opslag fejler |

Eksempel på input:

```json
["8.8.8.8", "1.1.1.1"]
```

Hvert element i svaret har denne struktur (illustrative værdier):

```json
{
  "data": {
    "ipAddress": "8.8.8.8",
    "abuseConfidenceScore": 0,
    "countryCode": "US",
    "isp": "Example ISP",
    "domain": "example.com",
    "totalReports": 0,
    "isTor": false
  }
}
```

Felternes værdier afhænger af det eksterne API; tekstfelter kan være null.
Opslagene bruger op til tre samtidige tråde pr. kald. Ét fejlet opslag giver 503 for hele requesten.
API-nøglen konfigureres på serveren og skal ikke sendes af klienten.

## WebSocket-notifikationer

Forbind til `ws://localhost:7070/ws/tickets`. I HTTPS-drift bruges `wss://`.
Første tekstbesked skal være selve JWT'et, uden Bearer-præfiks eller JSON-indpakning.
Ved godkendelse sender serveren:

```json
{"type":"CONNECTED"}
```

Ugyldigt token lukker forbindelsen med kode 1008. Token og rolle kontrolleres igen før hver notifikation.

| type | Modtagere |
|---|---|
| TICKET_CREATED | SUPPORT og ADMIN |
| TICKET_UPDATED, TICKET_ASSIGNED, COMMENT_CREATED, ATTACHMENT_CREATED, TICKET_DELETED | Ticketens ejer samt SUPPORT og ADMIN |

Beskeder sendes efter databaseoperationen og indeholder kun type og ticketId:

```json
{"type":"COMMENT_CREATED","ticketId":42}
```

Klienten henter detaljer gennem REST med samme adgangskontrol. Notifikationer kan også
udløses af ejerens egne ændringer. Kun tilsluttede klienter modtager dem;
der er ingen offlinehistorik, genafspilning eller leveringsgaranti.

## Fælles fejl og begrænsninger

| Status | Betydning |
|---|---|
| 400 | Manglende eller ugyldigt input |
| 401 | Manglende, ugyldigt eller udløbet token, slettet bruger eller forkert login |
| 403 | Rollen/ejerskabet giver ikke adgang, eller ticketens IP-kontrol afviser |
| 404 | Den efterspurgte ressource findes ikke |
| 409 | E-mailen er allerede registreret |
| 500 | Uventet server- eller databasefejl |
| 503 | Det eksterne IP-opslag kunne ikke gennemføres |

Almindelige API-fejl returneres som JSON; statuskoden står i HTTP-svaret:

```json
{"message": "Insufficient permissions"}
```

Javalin-valideringsfejl returnerer message `Invalid request input` og et ekstra `errors`-objekt
med de konkrete valideringsfejl. Uventede fejl returnerer `Internal server error`.
Autentifikation og autorisation kan afvise requesten, før input eller ressourcen undersøges.

Sletning er ikke cascading: Referencer fra tickets, kommentarer og attachments kan
forhindre sletning. Disse databasefejl oversættes aktuelt til 500, ikke 409.
REST-svar bruger entiteter direkte bortset fra IP-data og login-input; dokumentationen
beskriver derfor også de indlejrede User- og Ticket-objekter.
