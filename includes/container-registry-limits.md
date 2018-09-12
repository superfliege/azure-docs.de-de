---
title: Includedatei
description: Includedatei
services: container-registry
author: mmacy
ms.service: container-registry
ms.topic: include
ms.date: 08/30/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: bddb17a5163333a5aeb86b296a21867f0611d12f
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43303863"
---
| Ressource | Basic | Standard | Premium |
|---|---|---|---|---|
| Storage<sup>1</sup> | 10 GiB | 100 GiB| 500 GiB |
| Maximale Größe für Bildebenen | 20 GiB | 20 GiB | 50 GiB |
| Lesevorgänge pro Minute<sup>2, 3</sup> | 1.000 | 3,000 | 10.000 |
| Schreibvorgänge pro Minute<sup>2, 4</sup> | 100 | 500 | 2.000 |
| Downloadbandbreite MBit/s<sup>2</sup> | 30 | 60 | 100 |
| Uploadbandbreite MBit/s<sup>2</sup> | 10 | 20 | 50 |
| Webhooks | 2 | 10 | 100 |
| Georeplikation | N/V | N/V | [Unterstützt][geo-replication] |
| Inhaltsvertrauen (Vorschauversion) | N/V | N/V | [Unterstützt][content-trust] |

<sup>1</sup> Die angegebenen Speichergrenzen beziehen sich auf die Größe des *inkludierten* Speichers für jeden Tariftyp. Für Imagespeicher, der diese Grenzen überschreitet, wird Ihnen ein zusätzlicher Tagestarif pro GiB in Rechnung gestellt. Weitere Informationen zu diesen Tarifen finden Sie unter [Container Registry – Preise][pricing].

<sup>2</sup> *Lesevorgänge*, *Schreibvorgänge* und *Bandbreite* sind die geschätzten Mindestwerte. ACR soll die Leistung der Nutzung entsprechend verbessern.

<sup>3</sup> [docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) wird auf Grundlage der Zahl an Ebenen im Image sowie des Manifestabrufs in mehrere Lesevorgänge übersetzt.

<sup>4</sup> [docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) wird auf Grundlage der Zahl an Ebenen, die per Push übertragen werden müssen, in mehrere Schreibvorgänge übersetzt. Ein `docker push` enthält *Lesevorgänge* zum Abrufen eines Manifests für ein vorhandenes Image.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md