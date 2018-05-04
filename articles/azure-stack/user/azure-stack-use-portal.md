---
title: Verwenden des Azure Stack-Portals | Microsoft-Dokumentation
description: Informationen zum Zugriff auf das Benutzerportal in Azure Stack und zu dessen Verwendung.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 5aa00123-5b87-45e0-a671-4165e66bfbc6
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2018
ms.author: mabrigg
ms.openlocfilehash: 279722cc53889cb0a261fcffde0c7e0f86be6dc5
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="using-the-azure-stack-portal"></a>Verwenden des Azure Stack-Portals

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Als Nutzer der Azure Stack-Dienste können Sie im Azure Stack-Portal öffentliche Angebote abonnieren und die Dienste nutzen, die über diese Angebote bereitgestellt werden. Wenn Sie das Azure-Portal bereits verwendet haben, sind Sie mit der Benutzeroberfläche schon vertraut.

## <a name="access-the-portal"></a>Zugriff auf das Portal

Ihr Azure Stack-Operator (ein Dienstanbieter oder ein Administrator in Ihrer Organisation) teilt Ihnen die richtige URL für den Zugriff auf das Portal mit.

- Bei einem integrierten System variiert die URL abhängig von der Region und dem externen Domänenamen des Betreibers und hat das Format https://portal.&lt;*Region*&gt;.&lt;*FQDN*&gt;.
- Wenn Sie das Azure Stack Development Kit verwenden, lautet die Portaladresse https://portal.local.azurestack.external.

![Screenshot des Azure Stack-Benutzerportals](media/azure-stack-use-portal/UserPortal.png)

## <a name="customize-the-dashboard"></a>Anpassen des Dashboards

Das Dashboard enthält eine Reihe von Standardkacheln. Klicken Sie auf **Dashboard bearbeiten**, um das Standarddashboard anzupassen, oder auf **Neues Dashboard**, um ein benutzerdefiniertes Dashboard zu erstellen. Sie können ein Dashboard einfach durch Hinzufügen oder Entfernen von Kacheln anpassen. Klicken Sie beispielsweise zum Hinzufügen der Kachel „Compute“ auf **Neu**. Klicken Sie mit der rechten Maustaste auf **Compute**, und klicken Sie dann auf **An Dashboard anheften**.

## <a name="create-subscription-and-browse-available-resources"></a>Erstellen von Abonnements und Durchsuchen verfügbarer Ressourcen
 
Wenn Sie noch kein Abonnement haben, müssen Sie zunächst ein Angebot abonnieren. Danach können Sie die verfügbaren Ressourcen durchsuchen. Führen Sie zum Durchsuchen und Erstellen von Ressourcen einen der folgenden Schritte aus:

- Klicken Sie auf dem Dashboard auf die Kachel **Marketplace**.
- Klicken Sie auf der Kachel **Alle Ressourcen** auf **Ressourcen erstellen**.
- Klicken Sie im linken Navigationsbereich auf **Neu**.

## <a name="learn-how-to-use-available-services"></a>Erlernen der Verwendung verfügbarer Dienste

Wenn Sie Anleitungen zur Verwendung verfügbarer Dienste wünschen, stehen Ihnen möglicherweise unterschiedliche Optionen zur Verfügung.

- Ihre Organisation oder Ihr Dienstanbieter stellt möglicherweise eine eigene Dokumentation bereit. Dies ist in der Regel der Fall, wenn angepasste Dienste oder Apps angeboten werden.
- Für Drittanbieter-Apps sind eigene Dokumentationen erforderlich.
- Für Azure-konsistente Dienste sollten Sie zuerst die Azure Stack-Dokumentation lesen. Um auf die Azure Stack-Benutzerdokumentation zuzugreifen, klicken Sie auf das Symbol „Hilfe“ und dann auf **Hilfe und Support**.
 
    ![Screenshot der „Hilfe und Support“-Option in der Benutzeroberfläche](media/azure-stack-use-portal/HelpAndSupport.png)

    Insbesondere sollten Sie zum Einstieg die folgenden Artikel lesen:

    - [Wichtige Überlegungen: Verwenden von Diensten oder Erstellen von Apps für Azure Stack](azure-stack-considerations.md)
    - In der Dokumentation befindet sich im Abschnitt „Verwenden der Dienste“ für jeden Dienst ein Artikel „Überlegungen“. Auf der Seite „Überlegungen“ werden die Unterschiede zwischen dem Angebot des Diensts in Azure und dem Angebot des gleichen Diensts in Azure Stack beschrieben. Ein Beispiel hierfür ist [Considerations for Virtual Machines in Azure Stack](azure-stack-vm-considerations.md) (Überlegungen zu VMs in Azure Stack). Der Abschnitt „Verwenden der Dienste“ enthält möglicherweise andere, für Azure Stack individuelle Informationen.
     
      Sie können die Azure-Dokumentation als allgemeine Referenz für einen Dienst verwenden, aber Sie müssen sich dieser Unterschiede bewusst sein. Beachten Sie, dass die Dokumentationslinks auf der Kachel **Schnellstarttutorials** auf Azure-Dokumentation verweisen.

## <a name="get-support"></a>Support

Wenn Sie weitere Unterstützung benötigen, wenden Sie sich an Ihr Unternehmen oder Ihren Dienstanbieter.

Bei Verwendung des Azure Stack Development Kit können Sie nur das [Azure Stack-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) als Support nutzen.

## <a name="next-steps"></a>Nächste Schritte

[Wichtige Überlegungen: Verwenden von Diensten oder Erstellen von Apps für Azure Stack](azure-stack-considerations.md)
