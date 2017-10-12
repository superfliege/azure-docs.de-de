---
title: "Übersicht zum Verwalten von Updates in Azure Stack | Microsoft-Dokumentation"
description: "Informationen zur Updateverwaltung für integrierte Azure Stack-Systeme."
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: 9b0781f4-2cd5-4619-a9b1-59182b4a6e43
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: twooley
ms.openlocfilehash: 3d0d5ea6cc3f3cc7bc0550b83dabbf0ae6af8a27
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="manage-updates-in-azure-stack-overview"></a>Übersicht zum Verwalten von Updates in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme*

Microsoft wird in regelmäßigen Abständen Updatepakete für integrierte Azure Stack-Systeme veröffentlichen. Jedes Release von Microsoft-Softwareupdates ist in einem einzelnen Updatepaket gebündelt. Als Azure Stack-Operator können Sie problemlos über das Administratorportal diese Updatepakete importieren, installieren und ihren Installationsstatus überwachen. 

Ihr Originalgerätehersteller-Hardwareanbieter (Original Equipment Manufacturer, OEM) veröffentlicht auch Updates, z.B. Treiber- und Firmwareupdates. Diese Updates werden als separate Pakete von Ihrem OEM-Hardwareanbieter bereitgestellt und von Microsoft-Updates getrennt verwaltet.

Um den Support für Ihr System aufrechtzuerhalten, müssen Sie sicherstellen, dass Azure Stack stets bis zu einer bestimmten Version aktualisiert ist. Machen Sie sich unbedingt mit der [Azure Stack servicing policy](azure-stack-servicing-policy.md) (Azure Stack-Wartungsrichtlinie) vertraut.

> [!NOTE]
> Sie können Azure Stack-Updatepakete nicht auf Azure Stack Development Kit anwenden. Die Updatepakete sind für integrierte Systeme vorgesehen.

## <a name="the-update-resource-provider"></a>Der Updateressourcenanbieter

Azure Stack umfasst einen Updateressourcenanbieter, der die Anwendung von Microsoft-Softwareupdates orchestriert. Dieser Ressourcenanbieter stellt sicher, dass Updates alle physischen Hosts, Service Fabric-Anwendungen und Laufzeiten sowie alle virtuellen Computer der Infrastruktur und die ihnen zugeordneten Dienste übergreifend angewandt werden.

Bei der Installation der Updates können Sie einfach den allgemeinen Status anzeigen, während der Updateprozess die verschiedenen Subsysteme in Azure Stack (z.B. physische Hosts und virtuelle Computer der Infrastruktur) erfasst.

## <a name="plan-for-updates"></a>Planen für Updates

Es wird dringend empfohlen, dass Sie die Benutzer über alle Wartungsvorgänge unterrichten und normale Wartungsfenster so weit wie möglich außerhalb der Geschäftszeiten planen. Wartungsvorgänge können sowohl Mandantenworkloads als auch Portalvorgänge beeinträchtigen.

## <a name="using-the-update-tile-to-manage-updates"></a>Verwenden der Update-Kachel zum Verwalten von Updates
Das Verwalten von Updates über das Administratorportal ist ein einfacher Vorgang. Ein Azure Stack-Operator kann zu folgenden Zwecken zur Update-Kachel im Dashboard navigieren:

- Anzeigen wichtiger Informationen wie z.B. der aktuellen Version.
- Installieren von Updates und Überwachen des Status.
- Überprüfen des Updateverlaufs der zuvor installierten Updates.
 
## <a name="determine-the-current-version"></a>Bestimmen der aktuellen Version

Die Update-Kachel zeigt die aktuelle Version von Azure Stack. Sie gelangen mithilfe einer der folgenden Methoden zum Administratorportal:

- Zeigen Sie auf dem Dashboard die aktuelle Version auf der **Update**-Kachel an.
 
   ![Kachel „Updates“ auf dem Standarddashboard](./media/azure-stack-updates/image1.png)
 
- Klicken Sie auf der Kachel **Regionsverwaltung** auf den Namen der Region. Zeigen Sie die aktuelle Version auf der **Update**-Kachel an.

## <a name="next-steps"></a>Nächste Schritte

- [Azure Stack-Wartungsrichtlinie](azure-stack-servicing-policy.md) 
- [Regionsverwaltung in Azure Stack](azure-stack-region-management.md)     


