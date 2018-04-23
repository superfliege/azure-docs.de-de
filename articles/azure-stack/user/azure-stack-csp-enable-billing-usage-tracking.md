---
title: Wie Sie einem Cloud-Dienstanbieter ermöglichen, Ihr Azure Stack-Abonnement zu verwalten | Microsoft-Dokumentation
description: Aktivieren Sie den Dienstanbieter, um auf ein Abonnement in Azure Stack zuzugreifen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: 26ba68be6d4932da77befaf7c968525393c0a033
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="enable-a-cloud-service-provider-to-manage-your-azure-stack-subscription"></a>Wie Sie einem Cloud-Dienstanbieter ermöglichen, Ihr Azure Stack-Abonnement zu verwalten

*Gilt für: Integrierte Azure Stack-Systeme*

Wenn Sie Azure Stack mit einem Cloud-Dienstanbieter (Cloud Service Provider, CSP) verwenden, kann Ihr Zugriff auf Ressourcen in Ihrem Azure-Abonnement und in Azure Stack vom Anbieter verwaltet werden. Alternativ können Sie Ihr eigenes Abonnement verwalten. Dieser Artikel behandelt, wie Sie Ihrem Dienstanbieter ermöglichen können, in Ihrem Auftrag auf Ihr Abonnement zuzugreifen, oder sicherstellen, dass der Dienstanbieter Ihren Dienst verwalten kann.

> [!Note]  
>  Wenn die folgenden Schritte übersprungen werden und der CSP nicht bereits Ihr Konto verwaltet, kann der CSP Ihr Azure Stack-Abonnement nicht in Ihrem Auftrag verwalten.

## <a name="manage-your-subscription-with-a-cloud-service-provider"></a>Verwalten Ihres Abonnements mit einem Cloud-Dienstanbieter

1. Fügen Sie Ihren CSP als Gastbenutzer mit der Benutzerrolle Ihrem Mandantenverzeichnis hinzu.  Schritte zum Hinzufügen eines Benutzers finden Sie unter [Schnellstart: Hinzufügen neuer Benutzer in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).
2. Der CSP erstellt dann das lokale Azure Stack-Abonnement für Sie.
3. Sie können mit der Verwendung von Azure Stack beginnen.
3. Ihr CSP sollte dann eine Ressource in Ihrem Abonnement erstellen, um sicherzustellen, dass er auch Ihre Ressourcen verwalten kann. Zum Beispiel müsste das [Erstellen eines virtuellen Windows-Computers mit dem Azure Stack-Portal](azure-stack-quick-windows-portal.md) möglich sein.

## <a name="enable-the-cloud-service-provider-to-manage-your-subscription-using-rbac-rights"></a>Wie Sie dem Cloud-Dienstanbieter ermöglichen, Ihr Abonnement mithilfe von RBAC-Rechten zu verwalten

Fügen Sie den CSP Ihrem Abonnement als Besitzer hinzu. 

1. Fügen Sie Ihren CSP als Gastbenutzer mit der Besitzerrolle Ihrem Mandantenverzeichnis hinzu.  Schritte zum Hinzufügen eines Benutzers finden Sie unter [Schnellstart: Hinzufügen neuer Benutzer in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).
2. Fügen Sie die Besitzerrolle dem CSP-Gastbenutzer hinzu. Schritte zum Hinzufügen des CSP-Benutzers zu Ihrem Abonnement finden Sie unter [Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten des Zugriffs auf Ihre Azure-Abonnementressourcen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).
3. Der CSP erstellt dann das lokale Azure Stack-Abonnement für Sie.
4. Sie können mit der Verwendung von Azure Stack beginnen.
5. Ihr CSP sollte dann eine Ressource in Ihrem Abonnement erstellen, um sicherzustellen, dass er Ihre Ressourcen verwalten kann. 

## <a name="next-steps"></a>Nächste Schritte

  - Weitere Informationen zum Abrufen von Ressourcennutzungsinformationen aus Azure Stack finden Sie unter [Verbrauch und Abrechnung in Azure Stack](../azure-stack-billing-and-chargeback.md).
