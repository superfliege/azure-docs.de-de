---
title: Voraussetzungen für Azure IoT Edge-Module | Microsoft-Dokumentation
description: Voraussetzungen für die Veröffentlichung eines IoT Edge-Moduls.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: pbutlerm
ms.openlocfilehash: a4f1023bdf8a49fccbbda1fd0dc537f83a3acee1
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58013813"
---
# <a name="iot-edge-module-publishing-prerequisites"></a>Voraussetzungen für die Veröffentlichung von IoT Edge-Modulen

In diesem Artikel werden die Voraussetzungen für die Veröffentlichung eines IoT Edge-Modulangebots beschrieben.  Lesen Sie den [Leitfaden für die Veröffentlichung von IoT Edge-Modulen](../..//iot-edge-module.md), falls Sie dies nicht bereits getan haben.


## <a name="publishing-prerequisites"></a>Voraussetzungen für die Veröffentlichung

Wenn Sie ein IoT Edge-Modul im Azure Marketplace veröffentlichen möchten, müssen folgende Voraussetzungen erfüllt sein:

<!-- P2: It would be great to point to the terms of use of CPP here. This can often be a blocker for big companies and these terms of use are not anonymously visible yet.-->
- Sie benötigen Zugriff auf das [Cloud-Partnerportal](https://cloudpartner.azure.com/). Weitere Informationen hierzu finden Sie unter [Veröffentlichungsleitfaden für Azure Marketplace und AppSource](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).
- Sie müssen den [Azure Marketplace-Nutzungsbedingungen](https://azure.microsoft.com/support/legal/marketplace-terms/) zustimmen.
- Sie müssen die technische Ressource Ihres IoT Edge-Moduls in einer Azure Container Registry-Instanz hosten.  Weitere Informationen finden Sie unter [Vorbereiten der technischen Ressourcen für das IoT Edge-Modul](./cpp-create-technical-assets.md).
- Sie müssen über verwendungsbereite Metadaten für Ihr IoT Edge-Modul verfügen. Bereiten Sie z.B. die folgenden Ressourcen vor:
    - Titel
    - Beschreibung (im HTML-Format)
    - Logo (PNG-Format und feste Bildgrößen wie 40 x 40 px, 90 x 90 px, 115 x 115 px, 255 x 115 px)
    - Nutzungsbedingungen und Datenschutzrichtlinie
    - Standardmodulkonfiguration mit Routen, gewünschten Eigenschaften von Modulzwillingen, Erstellungsoptionen und Umgebungsvariablen
    - Moduldokumentation
    - Supportkontakte


## <a name="next-steps"></a>Nächste Schritte

Sobald Sie [die technische Ressource für Ihr IoT Edge-Modul vorbereitet haben](./cpp-create-technical-assets.md), können Sie [Ihr IoT Edge-Modulangebot erstellen](./cpp-create-offer.md). 
