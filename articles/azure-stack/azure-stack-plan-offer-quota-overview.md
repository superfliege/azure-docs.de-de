---
title: "Azure Stack – Übersicht über Pläne, Angebote, Kontingente und Abonnements | Microsoft-Dokumentation"
description: "Cloudbetreiber möchten verstehen, wie es sich mit Azure Stack-Plänen, -Angeboten, -Kontingenten und -Abonnements verhält."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 8/22/2017
ms.author: erikje
ms.openlocfilehash: 083ca2f0a06625810d2f90a682ba0b3110032e60
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="plan-offer-quota-and-subscription-overview"></a>Übersicht über Pläne, Angebote, Kontingente und Abonnements

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Mit [Azure Stack](azure-stack-poc.md) können Sie viele verschiedene Dienste bereitstellen, z.B. virtuelle Computer, SQL Server-Datenbanken, SharePoint, Exchange und sogar [Azure Marketplace-Elemente](azure-stack-marketplace-azure-items.md). Als Azure Stack-Bediener verwenden Sie Pläne, Angebote und Kontingente, um diese Dienste in Azure Stack zu konfigurieren und bereitzustellen.

Angebote enthalten mindestens einen Plan, und jeder Plan enthält mindestens einen Dienst. Indem Sie Pläne erstellen und in unterschiedlichen Angeboten kombinieren, können Sie Folgendes steuern:
- Dienste und Ressourcen, auf die Benutzer zugreifen können
- Menge der Ressourcen, die von Benutzern genutzt werden können
- Regionen, die Zugriff auf Ressourcen haben

Beim Bereitstellen eines Diensts führen Sie diese allgemeinen Schritte aus:

1. Fügen Sie einen Dienst hinzu, den Sie für Ihre Benutzer bereitstellen möchten.
2. Erstellen Sie einen Plan, der mindestens einen Dienst enthält. Beim Erstellen eines Plans können Sie Kontingente auswählen oder erstellen, mit denen die Ressourceneinschränkungen der einzelnen Dienste des Plans definiert werden.
3. Erstellen Sie ein Angebot, das mindestens einen Plan enthält (z.B. Basispläne und optionale Add-On-Pläne).

Nachdem Sie das Angebot erstellt haben, kann es von Ihren Benutzern abonniert werden, um auf die entsprechenden Dienste und Ressourcen zuzugreifen. Benutzer können beliebig viele Angebote abonnieren. Das folgende Diagramm enthält ein einfaches Beispiel für einen Benutzer, der zwei Angebote abonniert hat. Jedes Angebot verfügt über einen oder zwei Pläne, und jeder Plan ermöglicht den Zugriff auf diese Dienste.

![](media/azure-stack-key-features/image4.png)

## <a name="plans"></a>Pläne

Pläne sind Gruppen von einem oder mehreren Diensten. Als Azure Stack-Bediener [erstellen Sie Pläne](azure-stack-create-plan.md), die Sie Ihren Benutzern anbieten können. Im Gegenzug abonnieren Ihre Benutzer die Angebote, um die darin enthaltenen Pläne und Dienste zu verwenden. Stellen Sie beim Erstellen von Plänen sicher, dass Sie Ihre Kontingente festlegen und Ihre Basispläne definieren. Außerdem können Sie erwägen, optionale Add-On-Pläne einzubinden.

### <a name="quotas"></a>Kontingente

Als Hilfe beim Verwalten Ihrer Cloudkapazität wählen oder erstellen Sie ein Kontingent für jeden Dienst eines Plans. Mit Kontingenten werden die Obergrenzen von Ressourcen definiert, die ein Benutzerabonnement bereitstellen oder nutzen kann. Beispielsweise kann ein Benutzer für ein Kontingent ggf. bis zu fünf virtuelle Computer erstellen. Mit Kontingenten können verschiedene Ressourcen beschränkt werden, z.B. für virtuelle Computer, RAM und CPU.

Kontingente können nach Region konfiguriert werden. Beispielsweise kann für einen Plan mit Computediensten in Region A ein Kontingent von zwei virtuellen Computern, 4 GB RAM und 10 CPU-Kernen gelten. Im Azure Stack Development Kit ist nur eine Region verfügbar (mit dem Namen *local*).

### <a name="base-plan"></a>Basisplan

Beim Erstellen eines Angebots kann der Dienstadministrator einen Basisplan einbinden. Diese Basispläne sind standardmäßig enthalten, wenn ein Benutzer dieses Angebot abonniert. Wenn Benutzer ein Angebot abonnieren, haben sie Zugriff auf alle Ressourcenanbieter, die in diesen Basisplänen angegeben sind (mit den entsprechenden Kontingenten).

### <a name="add-on-plans"></a>Add-On-Pläne

Sie können in ein Angebot auch optionale Add-On-Pläne einschließen. Add-On-Pläne sind nicht standardmäßig im Abonnement enthalten. Add-On-Pläne sind zusätzliche Pläne (mit Kontingenten) in einem Angebot, die ein Abonnent seinen Abonnements hinzufügen kann. Beispielsweise können Sie einen Basisplan mit eingeschränkten Ressourcen für eine Testversion und einen Add-On-Plan mit umfassenderen Ressourcen für Kunden anbieten, die sich für die Nutzung des Diensts entscheiden.

## <a name="offers"></a>Angebote

Angebote sind Gruppen mit mindestens einem Plan, die Sie erstellen, damit Benutzer sie abonnieren können. Beispielsweise kann Angebot „Alpha“ den Plan A mit einem Satz von Computediensten und Plan B mit einem Satz von Speicher- und Netzwerkdiensten enthalten. 

Beim [Erstellen eines Angebots](azure-stack-create-offer.md) müssen Sie mindestens einen Basisplan einschließen, aber Sie können auch Add-On-Pläne erstellen, die Benutzer ihrem Abonnement hinzufügen können.


## <a name="subscriptions"></a>Abonnements

Über ein Abonnement können Benutzer Ihre Angebote wahrnehmen. Wenn Sie ein Azure Stack-Bediener bei einem Dienstanbieter sind, erwerben Benutzer (Mandanten) Ihre Dienste, indem sie Ihre Angebote abonnieren. Wenn Sie ein Azure Stack-Bediener in einer Organisation sind, können Ihre Benutzer (Mitarbeiter) die von Ihnen angebotenen Dienste abonnieren, ohne dafür zu zahlen. Jede Kombination aus einem Benutzer und einem Angebot stellt ein eindeutiges Abonnement dar. Daher kann ein Benutzer über Abonnements für mehrere Angebote verfügen, aber jedes Abonnement gilt nur für ein Angebot. Pläne, Angebote und Kontingente gelten jeweils nur für ein eindeutiges Abonnement. Die gemeinsame Nutzung für mehrere Abonnements ist nicht möglich. Jede Ressource, die von einem Benutzer erstellt wird, ist einem Abonnement zugeordnet.


### <a name="default-provider-subscription"></a>Standardabonnement des Anbieters

Das Standardabonnement des Anbieters wird automatisch erstellt, wenn Sie das Azure Stack Development Kit bereitstellen. Dieses Abonnement kann zum Verwalten von Azure Stack, zum Bereitstellen weiterer Ressourcenanbieter sowie zum Erstellen von Plänen und Angeboten für Benutzer verwendet werden. Aus Sicherheits- und Lizenzierungsgründen sollte es nicht eingesetzt werden, um Kundenworkloads und -anwendungen auszuführen. 

## <a name="next-steps"></a>Nächste Schritte

[Erstellen von Plänen](azure-stack-create-plan.md)
