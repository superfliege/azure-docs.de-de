---
title: Includedatei
description: Includedatei
services: container-registry
author: mmacy
ms.service: container-registry
ms.topic: include
ms.date: 05/29/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 92a5d162e7a0b2c752a2f8e9c5941edf43e539e3
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991070"
---
| Ressource | Basic | Standard | Premium |
|---|---|---|---|---|
| Speicher | 10 GiB | 100 GiB| 500 GiB |
| Maximale Größe für Bildebenen | 20 GiB | 20 GiB | 50 GiB |
| Lesevorgänge pro Minute<sup>1, 2</sup> | 1.000 | 3,000 | 10.000 |
| Schreibvorgänge pro Minute<sup>1, 3</sup> | 100 | 500 | 2.000 |
| Downloadbandbreite MBit/s<sup>1</sup> | 30 | 60 | 100 |
| Uploadbandbreite MBit/s<sup>1</sup> | 10 | 20 | 50 |
| Webhooks | 2 | 10 | 100 |
| Georeplikation | N/V | N/V | [Unterstützt](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication) |

<sup>1</sup> *Lesevorgänge*, *Schreibvorgänge* und *Bandbreite* sind die geschätzten Mindestwerte. ACR soll die Leistung der Nutzung entsprechend verbessern.

<sup>2</sup> [docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) wird auf Grundlage der Zahl an Ebenen im Image sowie des Manifestabrufs in mehrere Lesevorgänge übersetzt.

<sup>3</sup> [docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) wird auf Grundlage der Zahl an Ebenen, die per Push übertragen werden müssen, in mehrere Schreibvorgänge übersetzt. Ein `docker push` enthält *Lesevorgänge* zum Abrufen eines Manifests für ein vorhandenes Image.
