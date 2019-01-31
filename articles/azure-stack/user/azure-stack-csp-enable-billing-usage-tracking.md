---
title: Wie Sie einem Cloud-Dienstanbieter ermöglichen, Ihr Azure Stack-Abonnement zu verwalten | Microsoft-Dokumentation
description: Aktivieren Sie den Dienstanbieter, um auf ein Abonnement in Azure Stack zuzugreifen.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 01/19/2019
ms.openlocfilehash: 5d7398853e20702aef450e2532784f0dca7aac57
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55246601"
---
# <a name="enable-a-cloud-service-provider-to-manage-your-azure-stack-subscription"></a>Wie Sie einem Cloud-Dienstanbieter ermöglichen, Ihr Azure Stack-Abonnement zu verwalten

*Anwendungsbereich: Integrierte Azure Stack-Systeme*

Wenn Sie Azure Stack mit einem Cloud-Dienstanbieter (Cloud Service Provider, CSP) verwenden, können Sie Ihr eigenes Abonnement für den Zugriff auf Ressourcen in Azure und in Azure Stack verwalten. Sie können Ihr Abonnement auch vom Anbieter verwalten lassen. In diesem Artikel erfahren Sie Folgendes:

* Gewähren Sie Ihrem Dienstanbieter Zugriff auf Ihr Abonnement.
* Stellen Sie sicher, dass der Dienstanbieter, den Dienst verwalten kann.

> [!NOTE]
> Wenn der Cloud-Dienstanbieter Ihr Konto nicht verwaltet und Sie die folgenden Schritte überspringen, kann der Cloud-Dienstanbieter Ihr Azure Stack-Abonnement nicht für Sie verwalten.

## <a name="manage-your-subscription-with-a-cloud-service-provider"></a>Verwalten Ihres Abonnements mit einem Cloud-Dienstanbieter

Fügen Sie den CSP Ihrem Abonnement als **Benutzer** hinzu.

1. Fügen Sie Ihren CSP als Gastbenutzer mit der Benutzerrolle Ihrem Mandantenverzeichnis hinzu. Die Schritte zum Hinzufügen eines Benutzers finden Sie unter [Hinzufügen neuer Benutzer in Azure Active Directory](../../active-directory/add-users-azure-active-directory.md).
2. Der CSP erstellt das lokale Azure Stack-Abonnement für Sie. Sie können mit der Verwendung von Azure Stack beginnen.
3. Ihr CSP sollte eine Ressource in Ihrem Abonnement erstellen, um sicherzustellen, dass er auch Ihre Ressourcen verwalten kann. Zum Beispiel müsste das [Erstellen eines virtuellen Windows-Computers mit dem Azure Stack-Portal](azure-stack-quick-windows-portal.md) möglich sein.

## <a name="enable-the-cloud-service-provider-to-manage-your-subscription-using-rbac-rights"></a>Wie Sie dem Cloud-Dienstanbieter ermöglichen, Ihr Abonnement mithilfe von RBAC-Rechten zu verwalten

Fügen Sie den CSP Ihrem Abonnement als **Besitzer** hinzu.

1. Fügen Sie Ihren CSP Ihrem Mandantenverzeichnis als Gastbenutzer hinzu. Die Schritte zum Hinzufügen eines Benutzers finden Sie unter [Hinzufügen neuer Benutzer in Azure Active Directory](../../active-directory/add-users-azure-active-directory.md).
2. Fügen Sie dem CSP-Gastbenutzer die Rolle **Besitzer** hinzu. Die Schritte zum Hinzufügen des CSP-Benutzers zu Ihrem Abonnement finden Sie unter [Verwalten des Zugriffs auf Ihre Azure-Abonnementressourcen mithilfe der rollenbasierten Zugriffssteuerung](../../role-based-access-control/role-assignments-portal.md). Der CSP erstellt das lokale Azure Stack-Abonnement für Sie. Sie können mit der Verwendung von Azure Stack beginnen.
3. Ihr CSP sollte eine Ressource in Ihrem Abonnement erstellen, um sicherzustellen, dass er Ihre Ressourcen verwalten kann.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Abrufen von Ressourcennutzungsinformationen aus Azure Stack finden Sie unter [Verbrauch und Abrechnung in Azure Stack](../azure-stack-billing-and-chargeback.md).
