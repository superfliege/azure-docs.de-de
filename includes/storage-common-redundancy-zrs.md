---
title: Includedatei
description: Includedatei
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/11/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 4a43966180850645584043690b1be9d6ae232f6e
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2018
ms.locfileid: "44027446"
---
Zonenredundanter Speicher (ZRS) repliziert Ihre Daten synchron über drei Speichercluster in einer Region. Jeder Speichercluster ist physisch unabhängig von den anderen und befindet sich in einer eigenen Verfügbarkeitszone. Jede Verfügbarkeitszone und der ZRS-Cluster darin ist autonom und verfügt über getrennte Hilfsprogramme und Netzwerkfunktionen.

Durch das Speichern Ihrer Daten in einem ZRS-Konto wird sichergestellt, dass Sie auch dann auf Ihre Daten zugreifen und diese verwalten können, wenn eine Zone nicht verfügbar ist. ZRS bietet eine herausragende Leistung bei geringer Latenz. ZRS bietet die gleichen [Skalierbarkeitsziele](../articles/storage/common/storage-scalability-targets.md) wie [lokal redundanter Speicher (LRS)](../articles/storage/common/storage-redundancy-lrs.md).

Erwägen Sie ZRS für Szenarien, die eine hohe Konsistenz, Dauerhaftigkeit und Verfügbarkeit erfordern, selbst wenn das Rechenzentrum einer Zone bei einem Ausfall oder einer Naturkatastrophe nicht verfügbar ist. Der ZRS bietet eine Dauerhaftigkeit für Speicherobjekte von mindestens 99,9999999999 % (12-mal die Zahl 9) über einen Zeitraum von einem Jahr.

Weitere Informationen zu Verfügbarkeitszonen finden Sie unter [Übersicht über Verfügbarkeitszonen](https://docs.microsoft.com/azure/availability-zones/az-overview).