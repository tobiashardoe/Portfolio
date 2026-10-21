---
title: "Uge 4 – Concurrency"
date: 2026-10-21
draft: false
weight: 4
summary: "Samtidige IP-opslag med ExecutorService, Callable og Future."
---

Opsamling skrevet den 21. oktober 2026 om concurrency i TicketDesk.

## Hvad er implementeret?

AbuseIpService.checkMultiple() modtager en liste af IP-adresser. Metoden
opretter en ExecutorService med Executors.newFixedThreadPool(3). Hvert opslag
indsendes med submit(() -> checkIp(ipAddress)), og den tilhørende Future gemmes.
Lambdaen fungerer som en Callable, fordi den returnerer en DTO.

Alle opgaver indsendes først. Derefter kaldes future.get() for at samle
resultaterne i inputrækkefølge. get() venter, hvis den pågældende opgave stadig
kører. Opgaverne kan fortsat være i gang samtidig, selvom resultaterne læses i
rækkefølge. Puljen lukkes for nye opgaver med shutdown() i finally.

## Hvorfor denne løsning?

Et HTTP-kald bruger tid på at vente på netværket. Flere tråde gør det muligt
at arbejde på andre opslag imens. Tre tråde begrænser antallet af samtidige
opgaver pr. checkMultiple-kald; det er ikke en fælles grænse for hele serveren.
Det er heller ikke en begrænsning af antal requests pr. sekund.

## Afgrænsning

Concurrency er ikke automatisk testet. Hvis ét opslag fejler, fejler hele
metoden. Der er ingen eksplicit timeout på future.get() eller HTTP-requesten.
shutdown() afbryder ikke allerede indsendte opgaver. Runnable bruges ikke
direkte; forskellen til Callable er, at Runnable ikke returnerer et resultat.

Ticketoprettelse kalder checkIp() én gang. checkMultiple() bruges på det
separate endpoint til flere adresser.

