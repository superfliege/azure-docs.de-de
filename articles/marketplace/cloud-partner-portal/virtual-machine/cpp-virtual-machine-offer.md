---
title: VM-Angebot im Azure Marketplace | Microsoft-Dokumentation
description: Übersicht über die Vorgehensweise zum Veröffentlichen eines VM-Angebots im Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/28/2018
ms.author: pbutlerm
ms.openlocfilehash: d3682d18fb849b2d851bae0986f9e61f216aaf2c
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639133"
---
# <a name="virtual-machine-offer"></a>VM-Angebot

Dieser Abschnitt enthält eine Übersicht über die Veröffentlichung eines virtuellen Computers und ist als Herausgeberleitfaden für den [Azure Marketplace](https://azuremarketplace.microsoft.com) konzipiert.  Aus diesem Grund ist er in folgende Hauptbereiche unterteilt:

- [Voraussetzungen:](./cpp-prerequisites.md) Listet die technischen und geschäftlichen Anforderungen auf, die zum Erstellen oder Veröffentlichen eines VM-Angebots erfüllt sein müssen.
- [Erstellen eines VM-Angebots:](./cpp-create-offer.md) Hier werden die Schritte aufgelistet, die ausgeführt werden müssen, um über das [Cloud-Partnerportal](https://cloudpartner.azure.com) ein neues VM-Angebot zu erstellen.
- [Erstellen technischer VM-Ressourcen:](./cpp-create-technical-assets.md) Hier erfahren Sie, wie Sie die technischen Ressourcen für eine VM-Lösung erstellen und dieses Paket als VM-Angebot im Azure Marketplace konfigurieren.
- [Veröffentlichen des VM-Angebots](./cpp-publish-offer.md) Hier erfahren Sie, wie Sie das Angebot zur Veröffentlichung an den Azure Marketplace übermitteln.


## <a name="vm-publishing-process-flow"></a>Ablauf des VM-Veröffentlichungsprozesses

Das folgende Diagramm veranschaulicht die allgemeinen Schritte zum Veröffentlichen eines VM-Angebots. 

![VM-Veröffentlichungsprozess](./media/publishvm_001.png)

1. Erstellen des Angebots: In diesem Schritt werden sämtliche Details und Informationen für das Angebot konfiguriert. Hierzu zählen die Angebotsbeschreibung, Marketingmaterial, rechtliche Informationen, Supportinformationen und Ressourcenspezifikationen.

2. Erstellen der geschäftlichen und technischen Ressourcen: In diesem Schritt werden die geschäftlichen Ressourcen (rechtlich relevante Dokumente und Marketingmaterial) und die technischen Ressourcen für die zugeordnete Lösung (in diesem Fall: die virtuellen Computer sowie angefügte Datenträger) erstellt. 

3. Erstellen der SKU: In diesem Schritt werden die dem Angebot zugeordneten SKUs erstellt und übermittelt.  Für jedes Image, das Sie veröffentlichen möchten, ist eine eindeutige SKU erforderlich. 
 
4. Zertifizieren und Veröffentlichen des Angebots: Nach Fertigstellung des Angebots und der technischen Ressourcen kann das Angebot übermittelt werden. Im Rahmen des daraufhin beginnenden Veröffentlichungsprozesses wird die Lösung getestet, überprüft, zertifiziert und anschließend im Marketplace live geschaltet.  

## <a name="next-steps"></a>Nächste Schritte

Bevor Sie diese Schritte ausführen, müssen die [technischen und geschäftlichen Anforderungen](./cpp-prerequisites.md) für die Veröffentlichung eines virtuellen Computers im Microsoft Azure Marketplace erfüllt sein. 
