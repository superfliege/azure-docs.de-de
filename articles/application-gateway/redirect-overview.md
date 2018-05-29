---
title: Übersicht über die Umleitung in Azure Application Gateway
description: Hier finden Sie Informationen zur Umleitungsfunktion in Azure Application Gateway.
services: application-gateway
documentationcenter: na
author: amsriva
manager: jpconnock
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 3/19/2018
ms.author: amsriva
ms.openlocfilehash: 65c631ca9beb5eab5d8fe2b7e71daa0cf3b768fa
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
ms.locfileid: "33204376"
---
# <a name="application-gateway-redirect-overview"></a>Übersicht über die Umleitung in Application Gateway

Ein typisches Szenario vieler Webanwendungen ist die Unterstützung der automatischen Umleitung von HTTP zu HTTPS, um sicherzustellen, dass die gesamte Kommunikation zwischen einer Anwendung und ihren Benutzern über einen verschlüsselten Pfad stattfindet. In der Vergangenheit haben Kunden unter anderem dedizierte Back-End-Pools erstellt, die einzig dazu dienten, eingehende HTTP-Anforderungen zu HTTPS umzuleiten.

Application Gateway unterstützt nun die Umleitung von Datenverkehr am Gateway. Dies vereinfacht die Anwendungskonfiguration, optimiert die Ressourcennutzung und ermöglicht neue Umleitungsszenarien (einschließlich globale und pfadbasierte Umleitung). Die Application Gateway-Umleitung ist nicht auf HTTP zu HTTPS beschränkt. Vielmehr handelt es sich hierbei um einen allgemeinen Umleitungsmechanismus, mit dem Datenverkehr, der an einem Listener empfangen wird, in Application Gateway an einen anderen Listener umgeleitet werden kann. Auch die Umleitung externer Websites wird unterstützt.

Die Application Gateway-Umleitung bietet Folgendes:

-  **Globale Umleitung**

   Umleitungen von einem Listener zu einem anderen Listener des Gateways. Dies ermöglicht HTTP-zu-HTTPS-Umleitungen auf einer Website.
- **Pfadbasierte Umleitung**

   Bei dieser Art von Umleitung kann HTTP nur für einen bestimmten Websitebereich zu HTTPS umgeleitet werden – etwa in einem durch /cart/* gekennzeichneten Einkaufswagenbereich.
- **Umleitung an eine externe Website**

![Umleitung](./media/redirect-overview/redirect.png)

Aufgrund dieser Änderung müssen Kunden ein neues Umleitungskonfigurationsobjekt erstellen, das den Ziellistener oder die externe Website angibt, an den bzw. an die die Umleitung erfolgen soll. Das Konfigurationselement unterstützt auch Optionen zum Anfügen von URI-Pfad und Abfragezeichenfolge an die umgeleitete URL. Sie können auch auswählen, ob es sich um eine vorübergehende Umleitung (HTTP-Statuscode 302) oder um eine dauerhafte Umleitung (HTTP-Statuscode 301) handelt. Die erstellte Umleitungskonfiguration wird mittels einer neuen Regel an den Quelllistener angefügt. Bei Verwendung einer einfachen Regel wird die Umleitungskonfiguration einem Quelllistener zugeordnet und fungiert als globale Umleitung. Wenn eine pfadbasierte Regel verwendet wird, wird die Umleitungskonfiguration in der URL-Pfadzuordnung definiert. Daher gilt sie nur für den bestimmten Pfadbereich einer Site.

### <a name="next-steps"></a>Nächste Schritte

[Konfigurieren der Umleitung für Application Gateway mit PowerShell](tutorial-url-redirect-powershell.md)
