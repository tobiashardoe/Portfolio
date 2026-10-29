---
title: "Uge 8 – Deployment"
date: 2026-10-21
draft: false
weight: 8
summary: "Kørbar JAR, Docker, miljøvariabler og HTTPS gennem Caddy."
---

## Hvad er implementeret?

TicketSystem kører på en DigitalOcean-droplet i en Docker-container med Java 21.
Maven pakker kode og biblioteker i ticketsystem.jar, som overføres med scp.
Den eksisterende Caddyfile sender HTTPS-trafik til containerens port 7070.
API'et findes på [ticketsystem.tobiashardo.dk](https://ticketsystem.tobiashardo.dk/api/tickets).

## Hvorfor denne løsning?

Docker samler programmets kørselsmiljø, og Caddy håndterer domæne og HTTPS.
Appen og PostgreSQL deler Docker-netværk, så appen kan nå databasen via navnet db.
Passwords og nøgler læses fra en .env-fil på serveren og er udelukket fra JAR-filen.
Deployment udføres manuelt; der er ikke oprettet GitHub Actions eller automatisk udrulning.

## Hvad har jeg lært?

Jeg har arbejdet med forskellen på en port inde i en container og en offentlig adresse.
Flere apps kan bruge samme interne port, når Caddy sender til det rigtige containernavn.
Login, CRUD og roller er afprøvet online, og en supporter modtog TICKET_CREATED
gennem WebSocket i Chrome, da en ticket blev oprettet fra IntelliJ.

## Udfordringer

HTTPS virkede først, da DNS-recorden var oprettet, og Caddy kunne hente et certifikat.
En gammel version-kolonne i databasen gav 500 ved oprettelse, fordi Java ikke længere
sendte feltet. Standardværdien 0 løste indsættelsen uden at slette eksisterende data.
Lombok skulle desuden angives som annotation processor, før Maven kunne bygge med Java 25.

## Næste skridt

Afklare kontrol af den oprindelige klient-IP bag Caddy; koden læser forbindelsens adresse.
Undersøge den observerede WebSocket-idle-timeout og afprøve ejerbeskeder online.
Derefter kan automatiske builds overvejes som en udvidelse af den manuelle deployment.

