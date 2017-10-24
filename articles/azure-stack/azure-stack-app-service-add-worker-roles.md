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
ms.openlocfilehash: dbb6d956a615482e42745296c94b2c07c0086653
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="app-service-on-azure-stack-add-more-worker-roles"></a>App Service in Azure Stack: Hinzufügen von weiteren Workerrollen

Dieses Dokument enthält eine Anleitung zum Skalieren der Workerrollen für App Service in Azure Stack. Es werden die Schritte zum Erstellen von zusätzlichen Workerrollen aufgeführt, um Anwendungen beliebiger Größe zu unterstützen.

> [!NOTE]
> Wenn Ihre Azure Stack POC-Umgebung nicht mehr als 96 GB RAM aufweist, können beim Hinzufügen von zusätzlicher Kapazität Schwierigkeiten auftreten.

Standardmäßig unterstützt App Service in Azure Stack kostenlose und gemeinsam genutzte Workerebenen. Um andere Workerebenen hinzufügen zu können, müssen Sie weitere Workerrollen hinzufügen.

Wenn Sie sich nicht sicher sind, welche Elemente mit der App Service-Standardversion bei der Azure Stack-Installation bereitgestellt wurden, helfen Ihnen die zusätzlichen Informationen unter [Übersicht über App Service in Azure Stack](azure-stack-app-service-overview.md) weiter.

Azure App Service in Azure Stack stellt alle Rollen mithilfe von VM-Skalierungsgruppen bereit und nutzt somit die Skalierungsfunktionen dieser Workload. Aus diesem Grund erfolgt die gesamte Skalierung der Workerebenen über den App Service-Administrator.

Direktes Hinzufügen von zusätzlichen Workern über den Administrator des App Service-Ressourcenanbieters

1. Melden Sie sich beim Azure Stack-Verwaltungsportal als Dienstadministrator an.
2. Navigieren Sie zu **App Services**.

  ![](media/azure-stack-app-service-add-worker-roles/image01.png)
3. Klicken Sie auf **Rollen**. Eine Übersicht mit allen bereitgestellten App Service-Rollen wird angezeigt.
4. Klicken Sie auf **Instanzen skalieren**.

  ![](media/azure-stack-app-service-add-worker-roles/image02.png)
5. Unter **Instanzen skalieren**:
    1. Wählen Sie den **Rollentyp** aus. In dieser Vorschauversion ist diese Option auf Webworker beschränkt.
    2. Wählen Sie die **Workerebene** aus, auf der Sie diesen Worker bereitstellen möchten. Die Standardoptionen lauten „Klein“, „Mittel“, „Groß“ und „Shared“ (Gemeinsam genutzt). Wenn Sie Ihre eigenen Workerebenen erstellt haben, können Sie diese ebenfalls auswählen.
    3. Geben Sie an, wie viele zusätzliche **Rolleninstanzen** Sie hinzufügen möchten.
    4. Klicken Sie auf **OK**, um die zusätzlichen Worker bereitzustellen.
    ![](media/azure-stack-app-service-add-worker-roles/image03.png)
6. App Service in Azure Stack fügt jetzt die zusätzlichen VMs hinzu, konfiguriert sie, installiert die gesamte erforderliche Software und kennzeichnet sie als bereit, wenn dieser Prozess abgeschlossen ist. Dieser Prozess kann ca. 80 Minuten dauern.
7. Sie können den Bereitschaftsstatus der neuen Worker auf dem Blatt **Rollen** überwachen.

Nachdem die Worker vollständig bereitgestellt wurden und einsatzbereit sind, werden sie für die Benutzer verfügbar gemacht, damit diese ihre Workloads darauf bereitstellen können. Unten sehen Sie ein Beispiel für die verschiedenen Tarife, die standardmäßig verfügbar sind. Die Option zum Auswählen des entsprechenden Tarifs ist nicht verfügbar, wenn für eine bestimmte Workerebene keine Worker vorhanden sind.
![](media/azure-stack-app-service-add-worker-roles/image04.png)

>[!NOTE]
> Um die Rollen „Management“, „Front-End“ oder „Herausgeber“ horizontal hochzuskalieren, müssen Sie die entsprechende VM-Skalierungsgruppe horizontal hochskalieren. Wir fügen die Möglichkeit, diese Rollen über die App Service-Verwaltung horizontal hochzuskalieren, in einer zukünftigen Version hinzu.

### <a name="next-steps"></a>Nächste Schritte

[Konfigurieren von Bereitstellungsquellen](azure-stack-app-service-configure-deployment-sources.md)
