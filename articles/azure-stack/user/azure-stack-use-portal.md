---
title: Verwenden des Azure Stack-Portals | Microsoft-Dokumentation
description: Informationen zum Zugriff auf das Benutzerportal in Azure Stack und zu dessen Verwendung.
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: 5aa00123-5b87-45e0-a671-4165e66bfbc6
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: twooley
ms.openlocfilehash: 63c270affca31d3db7e03116f5e287d8569b0dae
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="using-the-azure-stack-portal"></a>Verwenden des Azure Stack-Portals

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Als Nutzer der Azure Stack-Dienste können Sie im Azure Stack-Portal öffentliche Angebote abonnieren und die Dienste nutzen, die über diese Angebote zur Verfügung gestellt werden. Wenn Sie das Azure-Portal bereits verwendet haben, sind Sie mit der Benutzeroberfläche schon vertraut.

## <a name="access-the-portal"></a>Zugriff auf das Portal

Ihr Azure Stack-Operator (ein Dienstanbieter oder ein Administrator in Ihrer Organisation) teilt Ihnen die richtige URL für den Zugriff auf das Portal mit. 

- Für ein integriertes System variiert die URL basierend auf der Region Ihres Operators und dem externen Domänenamen und hat das Format https://portal.&lt;*Region*&gt;.&lt; *FQDN*&gt;.
- Wenn Sie das Azure Stack Development Kit verwenden, lautet die Portaladresse „https://portal.local.azurestack.external“.

![Screenshot des Azure Stack-Benutzerportals](media/azure-stack-use-portal/UserPortal.png)

## <a name="customize-the-dashboard"></a>Anpassen des Dashboards

Das Dashboard enthält eine Reihe von Standardkacheln. Klicken Sie auf **Dashboard bearbeiten**, um das Standarddashboard anzupassen, oder auf **Neues Dashboard**, um benutzerdefinierte Dashboards hinzuzufügen. Sie können dem Dashboard einfach Kacheln hinzufügen. Klicken Sie beispielsweise auf **Neu**, klicken Sie mit der rechten Maustaste auf **Compute**, und klicken Sie dann auf **An Dashboard anheften**.

## <a name="create-subscription-and-browse-available-resources"></a>Erstellen von Abonnements und Durchsuchen verfügbarer Ressourcen
 
Wenn Sie noch kein Abonnement haben, müssen Sie zunächst ein Angebot abonnieren. Danach können Sie nach verfügbaren Ressourcen suchen. Führen Sie zum Durchsuchen und Erstellen von Ressourcen einen der folgenden Schritte aus:

- Klicken Sie auf dem Dashboard auf die Kachel **Marketplace**. 
- Klicken Sie auf der Kachel **Alle Ressourcen** auf **Ressourcen erstellen**.
- Klicken Sie im linken Navigationsbereich auf **Neu**.

## <a name="learn-how-to-use-available-services"></a>Erlernen der Verwendung verfügbarer Dienste

Wenn Sie Anleitungen zur Verwendung verfügbarer Dienste wünschen, stehen Ihnen möglicherweise unterschiedliche Optionen zur Verfügung.

- Ihr Unternehmen oder Ihr Dienstanbieter stellt möglicherweise eigene Dokumentationen bereit. Dies gilt insbesondere, wenn sie angepasste Dienste oder Apps anbieten.
- Für Drittanbieter-Apps sind eigene Dokumentationen erforderlich.
- Für Azure-konsistente Dienste sollten Sie zuerst die Azure Stack-Dokumentation lesen. Um auf die Azure Stack-Benutzerdokumentation zuzugreifen, klicken Sie auf das Symbol „Hilfe“ und dann auf **Hilfe und Support**.
 
    ![Screenshot der „Hilfe und Support“-Option in der Benutzeroberfläche](media/azure-stack-use-portal/HelpAndSupport.png)

    Insbesondere sollten Sie zum Einstieg die folgenden Artikel lesen:

    - [Wichtige Überlegungen: Verwenden von Diensten oder Erstellen von Apps für Azure Stack](azure-stack-considerations.md)
    - Im Abschnitt „Verwenden der Dienste“ der Dokumentation ist jeder Azure-konsistente Dienst aufgeführt. Für jeden Dienst gibt es ein Thema „Überlegungen“, in dem die Unterschiede zwischen dem Angebot des Diensts in Azure und dem Angebot des gleichen Diensts in Azure Stack beschrieben werden. Ein Beispiel hierfür ist [Considerations for Virtual Machines in Azure Stack](azure-stack-vm-considerations.md) (Überlegungen zu VMs in Azure Stack). Der Abschnitt „Verwenden der Dienste“ enthält möglicherweise andere, für Azure Stack individuelle Informationen. 
     
      Sie können die Azure-Dokumentation als allgemeine Referenz für einen Dienst verwenden, aber Sie müssen sich dieser Unterschiede bewusst sein. Beachten Sie, dass die Dokumentationslinks auf der Kachel **Schnellstarttutorials** auf Azure-Dokumentation verweisen.

## <a name="get-support"></a>Support

Wenn Sie weitere Unterstützung benötigen, wenden Sie sich an Ihr Unternehmen oder Ihren Dienstanbieter. 

Bei Verwendung des Azure Stack Development Kit können Sie nur das [Azure Stack-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) als Support nutzen.

## <a name="next-steps"></a>Nächste Schritte

[Wichtige Überlegungen: Verwenden von Diensten oder Erstellen von Apps für Azure Stack](azure-stack-considerations.md)
