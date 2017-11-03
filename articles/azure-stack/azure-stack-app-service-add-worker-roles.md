---
title: "Horizontales Hochskalieren von Workerrollen in App Services – Azure Stack | Microsoft-Dokumentation"
description: "Ausführliche Anleitung zum Skalieren von Azure Stack App Services"
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: slinehan
editor: anwestg
ms.assetid: 3cbe87bd-8ae2-47dc-a367-51e67ed4b3c0
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: anwestg
ms.openlocfilehash: 30ab325488684a26a6ef442e7c8241526a66aa4c
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2017
---
# <a name="app-service-on-azure-stack-add-more-infrastructure-or-worker-roles"></a>App Service für Azure Stack: Hinzufügen von weiteren Infrastruktur- oder Workerrollen

Dieses Dokument enthält Anleitungen zum Skalieren der Infrastruktur- und Workerrollen für App Service in Azure Stack. Es werden die Schritte zum Erstellen von zusätzlichen Workerrollen aufgeführt, um Anwendungen beliebiger Größe zu unterstützen.

> [!NOTE]
> Wenn Ihre Azure Stack-Umgebung nicht mehr als 96 GB RAM aufweist, können beim Hinzufügen von zusätzlicher Kapazität Schwierigkeiten auftreten.

Standardmäßig unterstützt App Service in Azure Stack kostenlose und gemeinsam genutzte Workerebenen. Um andere Workerebenen hinzufügen zu können, müssen Sie weitere Workerrollen hinzufügen.

Wenn Sie sich nicht sicher sind, welche Elemente mit der App Service-Standardversion bei der Azure Stack-Installation bereitgestellt wurden, helfen Ihnen die zusätzlichen Informationen unter [Übersicht über App Service in Azure Stack](azure-stack-app-service-overview.md) weiter.

Azure App Service in Azure Stack stellt alle Rollen mithilfe von VM-Skalierungsgruppen bereit und nutzt somit die Skalierungsfunktionen dieser Workload. Aus diesem Grund erfolgt die gesamte Skalierung der Workerebenen über den App Service-Administrator.

Direktes Hinzufügen von zusätzlichen Workern über den Administrator des App Service-Ressourcenanbieters

1. Melden Sie sich beim Azure Stack-Verwaltungsportal als Dienstadministrator an.

2. Navigieren Sie zu **App Services**.

    ![](media/azure-stack-app-service-add-worker-roles/image01.png)
  
3. Klicken Sie auf **Rollen**. Eine Übersicht mit allen bereitgestellten App Service-Rollen wird angezeigt.

4. Klicken Sie mit der rechten Maustaste auf die Zeile des Typs, den Sie skalieren möchten, und klicken Sie dann auf **ScaleSet**.

    ![](media/azure-stack-app-service-add-worker-roles/image02.png)
  
5. Klicken Sie auf **Skalierung**, wählen Sie die Anzahl der Instanzen aus, auf die skaliert werden soll, und klicken Sie dann auf **Speichern**.
    
    ![](media/azure-stack-app-service-add-worker-roles/image03.png)

6. App Service in Azure Stack fügt jetzt die zusätzlichen VMs hinzu, konfiguriert sie, installiert die gesamte erforderliche Software und kennzeichnet sie als bereit, wenn dieser Prozess abgeschlossen ist. Dieser Prozess kann ca. 80 Minuten dauern.

7. Sie können den Bereitschaftsstatus der neuen Rollen überwachen, indem Sie die Worker auf dem Blatt **Rollen** anzeigen.

Nachdem die Worker vollständig bereitgestellt wurden und einsatzbereit sind, werden sie für die Benutzer verfügbar gemacht, damit diese ihre Workloads darauf bereitstellen können. Unten sehen Sie ein Beispiel für die verschiedenen Tarife, die standardmäßig verfügbar sind. Die Option zum Auswählen des entsprechenden Tarifs ist nicht verfügbar, wenn für eine bestimmte Workerebene keine Worker vorhanden sind.

![](media/azure-stack-app-service-add-worker-roles/image04.png)

>[!NOTE]
> Um die Rollen „Management“, „Front-End“ oder „Herausgeber“ horizontal hochzuskalieren, müssen Sie die entsprechende VM-Skalierungsgruppe horizontal hochskalieren. Wir fügen die Möglichkeit, diese Rollen über die App Service-Verwaltung horizontal hochzuskalieren, in einer zukünftigen Version hinzu.

Um die Rollen „Management“, „Front-End“ oder „Herausgeber“ horizontal hochzuskalieren, befolgen Sie die gleichen Schritte und wählen dabei den entsprechenden Rollentyp aus. Controller werden nicht als Skalierungsgruppen bereitgestellt. Aus diesem Grund sollten zwei Controller zum Installationszeitpunkt für alle Produktionsbereitstellungen bereitgestellt werden.

### <a name="next-steps"></a>Nächste Schritte

[Konfigurieren von Bereitstellungsquellen](azure-stack-app-service-configure-deployment-sources.md)
