---
title: Voraussetzungen für Azure IoT Edge-Module | Microsoft-Dokumentation
description: Voraussetzungen für die Veröffentlichung eines IoT Edge-Moduls.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 7271a97dc7ab9de1840d809ded0ba1c2940ed83f
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49432924"
---
# <a name="iot-edge-module-publishing-prerequisites"></a>Voraussetzungen für die Veröffentlichung von IoT Edge-Modulen

In diesem Artikel werden die Voraussetzungen für die Veröffentlichung eines IoT Edge-Modulangebots beschrieben.

Weitere Informationen zu IoT Edge-Modulen sowie zu den Vorteilen der Veröffentlichung eines Moduls im Azure Marketplace finden Sie im [Leitfaden zur Veröffentlichung von IoT Edge-Modulen](https://docs.microsoft.com/azure/marketplace/iot-edge-module).

## <a name="publishing-prerequisites"></a>Voraussetzungen für die Veröffentlichung

Wenn Sie ein IoT Edge-Modul im Azure Marketplace veröffentlichen möchten, müssen folgende Voraussetzungen erfüllt sein:

<!-- P2: It would be great to point to the terms of use of CPP here. This can often be a blocker for big companies and these terms of use are not anonymously visible yet.-->
- Sie benötigen Zugriff auf das [Cloud-Partnerportal](https://cloudpartner.azure.com/). Weitere Informationen hierzu finden Sie unter [Veröffentlichungsleitfaden für Azure Marketplace und AppSource](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).
- Sie müssen den [Azure Marketplace-Nutzungsbedingungen](https://azure.microsoft.com/support/legal/marketplace-terms/) zustimmen.
- Sie müssen die technische Ressource Ihres IoT Edge-Moduls in einer Azure Container Registry-Instanz hosten.  Weitere Informationen finden Sie unter [Vorbereiten der technischen Ressourcen für das IoT Edge-Modul](./cpp-create-technical-assets.md).
- Sie müssen über verwendungsbereite Metadaten für Ihr IoT Edge-Modul verfügen. Hierzu zählen unter anderem folgende:
    - Titel
    - Beschreibung (im HTML-Format)
    - Logo (PNG-Format und feste Bildgrößen wie 40 x 40 px, 90 x 90 px, 115 x 115 px, 255 x 115 px)
    - Nutzungsbedingungen und Datenschutzrichtlinie
    - Standardmodulkonfiguration mit Routen, gewünschten Eigenschaften von Modulzwillingen, Erstellungsoptionen und Umgebungsvariablen
    - Dokumentation
    - Supportkontakte

## <a name="next-steps"></a>Nächste Schritte

- [Vorbereiten der technischen Ressourcen für das IoT Edge-Modul](./cpp-create-technical-assets.md)
- [Erstellen Ihres IoT Edge-Modulangebots](./cpp-create-offer.md)
