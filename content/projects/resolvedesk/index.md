---
title: "TicketSystem"
date: 2026-10-21
draft: false
slug: "ticketsystem"
aliases: ["/projects/resolvedesk/", "/projects/ticketdesk/"]
summary: "Et ticketsystem i Java med JPA, REST, AbuseIPDB og JWT."
---

TicketSystem er mit backendprojekt på 3. semester. Brugere kan oprette og følge
supporthenvendelser, mens supportere og administratorer kan behandle dem.
I ugeindlæggene beskriver jeg mine valg, udfordringer og det, jeg har lært undervejs.

## Projekt og dokumentation

- [GitHub og README](https://github.com/tobiashardoe/ticketsystem) – opbygning, datamodel, tests og user stories.
- [Fuld API-dokumentation](https://github.com/tobiashardoe/ticketsystem/blob/main/docs/api.md).
- [Deployet API](https://ticketsystem.tobiashardo.dk/api/tickets).

API'et kræver JWT til tickets. Et svar med 401 uden token er derfor forventet;
der er ingen login-side eller frontend.

## Ugeindlæg

- [Uge 1 – Opsætning]({{< relref "log/week1-setup.md" >}})
- [Uge 2 – Entiteter og relationer]({{< relref "log/week2-planning-structure.md" >}})
- [Uge 3 – API, JSON og DTO]({{< relref "log/week3-data-integration.md" >}})
- [Uge 4 – Concurrency]({{< relref "log/week4-concurrency.md" >}})
- [Uge 5 – REST API]({{< relref "log/week5-rest-api.md" >}})
- [Uge 6 – REST-tests]({{< relref "log/week6-rest-tests.md" >}})
- [Uge 7 – Security]({{< relref "log/week7-security.md" >}})
- [Uge 8 – Deployment]({{< relref "log/week8-deployment.md" >}})
