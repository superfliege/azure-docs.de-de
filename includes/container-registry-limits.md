---
title: Includedatei
description: Includedatei
services: container-registry
author: mmacy
ms.service: container-registry
ms.topic: include
ms.date: 03/23/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 575483192954f4e05db50e701e223829e041cffc
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
| Ressource | Basic | Standard | Premium |
|---|---|---|---|---|
| Speicher | 10 GiB | 100 GiB| 500 GiB |
| Lesevorgänge pro Minute<sup>1, 2</sup> | 1000 | 3000 | 10000 |
| Schreibvorgänge pro Minute<sup>1, 3</sup> | 100 | 500 | 2000 |
| Downloadbandbreite MBit/s<sup>1</sup> | 30 | 60 | 100 |
| Uploadbandbreite MBit/s<sup>1</sup> | 10 | 20 | 50 |
| Webhooks | 2 | 10 | 100 |
| Georeplikation | N/V | N/V | [Unterstützt *(Vorschauversion)*](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication) |

<sup>1</sup> *Lesevorgänge*, *Schreibvorgänge* und *Bandbreite* sind die geschätzten Mindestwerte. ACR soll die Leistung der Nutzung entsprechend verbessern.

<sup>2</sup> [docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) wird auf Grundlage der Zahl an Ebenen im Image sowie des Manifestabrufs in mehrere Lesevorgänge übersetzt.

<sup>3</sup> [docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) wird auf Grundlage der Zahl an Ebenen, die per Push übertragen werden müssen, in mehrere Schreibvorgänge übersetzt. Ein `docker push` enthält *Lesevorgänge* zum Abrufen eines Manifests für ein vorhandenes Image.