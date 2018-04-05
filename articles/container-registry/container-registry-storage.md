---
title: Speichern von Images in Azure Container Registry
description: Details darüber, wie Ihre Docker-Containerimages in Azure Container Registry gespeichert werden, sowie über Sicherheit, Redundanz und Kapazität.
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 03/21/2018
ms.author: marsma
ms.openlocfilehash: df46712889a3eba54f1a2288ba93c82b21b92deb
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="container-image-storage-in-azure-container-registry"></a>Speichern von Containerimages in Azure Container Registry

Jede Azure Container Registry vom Typ [Basic, Standard und Premium](container-registry-skus.md) profitiert von erweiterten Azure-Speicherfunktionen wie Verschlüsselung ruhender Daten für die Imagedatensicherheit und Georedundanz für den Imagedatenschutz. Die folgenden Abschnitte beschreiben sowohl die Funktionen als auch die Imagespeichergrenzen in Azure Container Registry (ACR).

## <a name="encryption-at-rest"></a>Verschlüsselung ruhender Daten

Alle Containerimages in Ihrer Registrierung werden im Ruhezustand verschlüsselt. Azure verschlüsselt ein Image automatisch, bevor es gespeichert wird, und entschlüsselt es dynamisch, sobald Sie oder Ihre Anwendungen und Dienste einen Pull für das Image ausführen.

## <a name="geo-redundant-storage"></a>Georedundanter Speicher

Azure verwendet ein georedundantes Speicherschema, um den Verlust Ihrer Containerimages zu verhindern. Azure Container Registry repliziert die Containerimages automatisch in mehrere geografisch entfernte Rechenzentren und verhindert deren Verlust im Falle eines regionalen Speicherausfalls.

## <a name="geo-replication"></a>Georeplikation

Für Szenarien, die eine noch zuverlässigere Hochverfügbarkeit erfordern, sollten Sie das Feature [Georeplikation](container-registry-geo-replication.md) der Premium-Registrierungen nutzen. Die Georeplikation schützt vor dem Verlust des Zugriffs auf Ihre Registrierung im Falle eines *Totalausfalls* der Region, nicht nur eines Speicherausfalls. Zudem bietet die Georeplikation weitere Vorteile, wie z.B. netzwerknahe Imagespeicher für schnellere Push- und Pullvorgänge in verteilten Entwicklungs- oder Bereitstellungsszenarien.

## <a name="image-limits"></a>Imagegrenzen

Die folgende Tabelle beschreibt das Containerimage und die Speichergrenzen für Azure-Containerregistrierungen.

| Ressource | Begrenzung |
| -------- | :---- |
| Repositorys | Keine Begrenzung |
| Bilder | Keine Begrenzung |
| Ebenen | Keine Begrenzung |
| Tags | Keine Begrenzung|
| Speicher | 5 TB |

Eine hohe Anzahl von Repositorys und Tags können die Leistung Ihrer Registrierung beeinträchtigen. Löschen Sie regelmäßig unbenutzte Repositorys, Tags und Images, indem Sie die [Azure CLI](/cli/azure/acr), die [ACR-REST-API](/rest/api/containerregistry/) oder das [Azure-Portal][portal] in die Wartungsroutine für Ihre Registrierung einbinden. Gelöschte Registrierungsressourcen wie Repositorys, Images und Tags können nach dem Löschen *nicht* wiederhergestellt werden.

## <a name="storage-cost"></a>Speicherkosten

Ausführliche Informationen zu den Preisen finden Sie unter [Containerregistrierung – Preise][pricing].

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den verschiedenen Azure Container Registry-SKUs (Basic, Standard, Premium) finden Sie unter den [Azure Container Registry-SKUs](container-registry-skus.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[portal]: https://portal.azure.com
[pricing]: http://aka.ms/acr/pricing

<!-- LINKS - Internal -->
