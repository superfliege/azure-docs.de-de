---
title: Wie Sie einem Cloud-Dienstanbieter ermöglichen, Ihr Azure Stack-Abonnement zu verwalten | Microsoft-Dokumentation
description: Aktivieren Sie den Dienstanbieter, um auf ein Abonnement in Azure Stack zuzugreifen.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: brenduns
ms.reviewer: alfredop
ms.openlocfilehash: f309b86578f340040927735c067656158f3198fc
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41946484"
---
# <a name="enable-a-cloud-service-provider-to-manage-your-azure-stack-subscription"></a>Wie Sie einem Cloud-Dienstanbieter ermöglichen, Ihr Azure Stack-Abonnement zu verwalten

*Gilt für: Integrierte Azure Stack-Systeme*

Wenn Sie Azure Stack mit einem Cloud Service Provider (CSP) verwenden, können Sie Ihr eigenes Abonnement für den Zugriff auf Ressourcen in Azure und in Azure Stack verwalten. Sie können Ihr Abonnement auch vom Anbieter verwalten lassen. In diesem Artikel erfahren Sie Folgendes:

 * Gewähren Sie Ihrem Dienstanbieter Zugriff auf Ihr Abonnement.
 * Stellen Sie sicher, dass der Dienstanbieter, den Dienst verwalten kann.

> [!Note]
>  Wenn der CSP Ihr Konto nicht verwaltet und Sie die folgenden Schritte überspringen, kann der CSP Ihr Azure Stack-Abonnement nicht für Sie verwalten.

## <a name="manage-your-subscription-with-a-cloud-service-provider"></a>Verwalten Ihres Abonnements mit einem Cloud-Dienstanbieter

Fügen Sie den CSP Ihrem Abonnement als **Benutzer** hinzu.

1. Fügen Sie Ihren CSP als Gastbenutzer mit der Benutzerrolle Ihrem Mandantenverzeichnis hinzu.  Schritte zum Hinzufügen eines Benutzers finden Sie unter [Schnellstart: Hinzufügen neuer Benutzer in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).
2. Der CSP erstellt das lokale Azure Stack-Abonnement für Sie.
3. Sie können mit der Verwendung von Azure Stack beginnen.
4. Ihr CSP sollte eine Ressource in Ihrem Abonnement erstellen, um sicherzustellen, dass er auch Ihre Ressourcen verwalten kann. Zum Beispiel müsste das [Erstellen eines virtuellen Windows-Computers mit dem Azure Stack-Portal](azure-stack-quick-windows-portal.md) möglich sein.

## <a name="enable-the-cloud-service-provider-to-manage-your-subscription-using-rbac-rights"></a>Wie Sie dem Cloud-Dienstanbieter ermöglichen, Ihr Abonnement mithilfe von RBAC-Rechten zu verwalten

Fügen Sie den CSP Ihrem Abonnement als **Besitzer** hinzu.

1. Fügen Sie Ihren CSP Ihrem Mandantenverzeichnis als Gastbenutzer hinzu.  Schritte zum Hinzufügen eines Benutzers finden Sie unter [Schnellstart: Hinzufügen neuer Benutzer in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).
2. Fügen Sie die Besitzerrolle dem CSP-Gastbenutzer hinzu. Schritte zum Hinzufügen des CSP-Benutzers zu Ihrem Abonnement finden Sie unter [Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten des Zugriffs auf Ihre Azure-Abonnementressourcen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).
3. Der CSP erstellt das lokale Azure Stack-Abonnement für Sie.
4. Sie können mit der Verwendung von Azure Stack beginnen.
5. Ihr CSP sollte eine Ressource in Ihrem Abonnement erstellen, um sicherzustellen, dass er Ihre Ressourcen verwalten kann.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Abrufen von Ressourcennutzungsinformationen aus Azure Stack finden Sie unter [Verbrauch und Abrechnung in Azure Stack](../azure-stack-billing-and-chargeback.md).
