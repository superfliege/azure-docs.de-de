---
title: Includedatei
description: Includedatei
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 08/30/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 844e4a0d2715799b808f2c7630c201f6e792bd63
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58124418"
---
| Ressource | Basic | Standard | Premium |
|---|---|---|---|
| Storage<sup>1</sup> | 10 GiB | 100 GiB| 500 GiB |
| Maximale Größe für Imageebenen | 20 GiB | 20 GiB | 50 GiB |
| Lesevorgänge pro Minute<sup>2, 3</sup> | 1.000 | 3,000 | 10.000 |
| Schreibvorgänge pro Minute<sup>2, 4</sup> | 100 | 500 | 2.000 |
| Downloadbandbreite MBit/s<sup>2</sup> | 30 | 60 | 100 |
| Uploadbandbreite MBit/s<sup>2</sup> | 10 | 20 | 50 |
| Webhooks | 2 | 10 | 100 |
| Georeplikation | – | – | [Unterstützt][geo-replication] |
| Inhaltsvertrauen (Vorschauversion) | – | – | [Unterstützt][content-trust] |

<sup>1</sup> Die angegebenen Speichergrenzen beziehen sich auf die Größe des *inbegriffenen* Speichers für jeden Tarif. Für Imagespeicher, der diese Grenzen überschreitet, wird Ihnen ein zusätzlicher Tagestarif pro GiB in Rechnung gestellt. Weitere Informationen zu diesen Tarifen finden Sie unter [Container Registry – Preise][pricing].

<sup>2</sup> *Lesevorgänge*, *Schreibvorgänge* und *Bandbreite* sind geschätzte Mindestwerte. Die Azure Container Registry soll die Leistung der Nutzung nach Bedarf verbessern.

<sup>3</sup> Ein [docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image)-Befehl wird basierend auf der Anzahl von Ebenen im Image in mehrere Lesevorgänge plus Manifestabruf übersetzt.

<sup>4</sup> Ein [docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image)-Befehl wird basierend auf der Anzahl von Ebenen, die per Push übertragen werden müssen, in mehrere Schreibvorgänge übersetzt. Ein `docker push` enthält *Lesevorgänge* zum Abrufen eines Manifests für ein vorhandenes Image.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md
