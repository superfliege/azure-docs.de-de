---
title: Übersicht über das Veröffentlichen von Azure IoT Edge-Modulangeboten | Microsoft-Dokumentation
description: Übersicht über die Vorgehensweise zum Veröffentlichen eines IoT Edge-Modulangebots im Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal
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
ms.openlocfilehash: 68c58c16d7083182d412adb6ed0d243a3291cc42
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49432828"
---
# <a name="iot-edge-module-offer-publishing-overview"></a>Übersicht über das Veröffentlichen von IoT Edge-Modulangeboten

Dieser Artikel beschreibt den Veröffentlichungsprozess und die wichtigsten Bestandteile eines IoT Edge-Modulangebots. Verwenden Sie diesen Artikel als Ausgangspunkt für die Veröffentlichung Ihres Angebots im [Azure Marketplace](https://azuremarketplace.microsoft.com).

## <a name="publishing-process"></a>Veröffentlichungsvorgang

Die Veröffentlichung eines IoT Edge-Modulangebots umfasst ganz allgemein folgende Schritte:

1. Erstellen des Angebots<br> Geben Sie detaillierte Angebotsinformationen an. Hierzu zählen unter anderem die Angebotsbeschreibung, Marketingmaterial, Supportinformationen und Ressourcenspezifikationen.

2. Erstellen der geschäftlichen und technischen Ressourcen<br> Erstellen Sie die geschäftlichen Ressourcen (rechtlich relevante Dokumente und Marketingmaterial) und die technischen Ressourcen für die zugeordnete Lösung (die in einer Azure Container Registry-Instanz gehosteten IoT Edge-Modulimages).

3. Erstellen der SKU(s)<br> Erstellen Sie die dem Angebot zugeordneten SKUs. Für jedes Image, das Sie veröffentlichen möchten, ist eine eindeutige SKU erforderlich.

4. Zertifizieren und Veröffentlichen des Angebots <br>Nach Fertigstellung der Angebotsressourcen und der technischen Ressourcen kann das Angebot übermittelt werden. Durch diese Übermittlung wird der Veröffentlichungsprozess gestartet. Im Rahmen dieses Prozesses wird die Lösung getestet, überprüft, zertifiziert und anschließend im Azure Marketplace live geschaltet.

## <a name="parts-of-an-offer"></a>Komponenten eines Angebots

In den folgenden Artikeln werden die wichtigsten Bestandteile eines IoT Edge-Modulangebots behandelt.

- [Voraussetzungen](./cpp-prerequisites.md) <br>In diesem Artikel erfahren Sie, welche technischen und geschäftlichen Anforderungen erfüllt werden müssen, um ein IoT Edge-Modulangebot erstellen oder veröffentlichen zu können.
- [Vorbereiten der technischen Ressourcen für das IoT Edge-Modul](./cpp-create-technical-assets.md) <br>In diesem Artikel erfahren Sie, wie Sie die technischen Ressourcen für ein IoT Edge-Modul vorbereiten. Diese Ressourcen müssen jeweils die erforderlichen technischen Kriterien erfüllen, damit das IoT Edge-Modul im Azure Marketplace veröffentlicht werden kann.
- [Erstellen eines IoT Edge-Modulangebots](./cpp-create-offer.md) <br>In diesem Artikel werden die Schritte aufgelistet, die ausgeführt werden müssen, um über das [Cloud-Partnerportal](https://cloudpartner.azure.com) ein neues IoT Edge Modulangebot zu erstellen.
- [Veröffentlichen eines IoT Edge-Modulangebots](./cpp-publish-offer.md)<br> In diesem Artikel erfahren Sie, wie Sie das Angebot zur Veröffentlichung im Azure Marketplace übermitteln.

## <a name="next-steps"></a>Nächste Schritte

Überprüfen Sie die [technischen und geschäftlichen Anforderungen](./cpp-prerequisites.md) für die Veröffentlichung eines IoT Edge-Moduls im Microsoft Azure Marketplace.
