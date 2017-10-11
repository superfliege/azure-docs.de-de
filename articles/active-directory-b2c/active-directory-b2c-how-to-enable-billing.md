---
title: "Verknüpfen Sie ein Azure-Abonnement mit Azure AD B2C | Microsoft Docs"
description: "Schrittweise Anleitung für die Abrechnung von Azure AD B2C-Mandanten in einem Azure-Abonnement zu aktivieren."
services: active-directory-b2c
documentationcenter: dev-center-name
author: rojasja
manager: mbaldwin
ms.service: active-directory-b2c
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/05/2016
ms.author: joroja
ms.openlocfilehash: 5b9955b2af7f20a79981315fa33a0eb5380a5465
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="linking-an-azure-subscription-to-an-azure-b2c-tenant-to-pay-for-usage-charges"></a>Verknüpfen ein Azure-Abonnement mit einem Azure B2C-Mandanten zum Bezahlen der Nutzungsgebühren

Laufende Nutzungsgebühren für Azure Active Directory B2C (oder Azure AD B2C) sind mit einem Azure-Abonnement in Rechnung gestellt. Es ist für den mandantenadministrator zum expliziten Verknüpfen des Azure AD B2C-Mandanten nach dem Erstellen des B2C-Mandantenverwaltungs selbst ein Azure-Abonnement erforderlich.  Dieser Link erfolgt durch Erstellen einer Azure AD "B2C-Mandanten" Ressource auf der Azure-Abonnement. Viele B2C-Mandanten können mit einem einzelnen Azure-Abonnement zusammen mit anderen Azure-Ressourcen (z. B. VMs, datenspeicherung, LogicApps) verknüpft werden


> [!IMPORTANT]
> Die neuesten Informationen zu Verwendung Abrechnungen und Preis für B2C an der folgenden Seite befindet: [Azure AD B2C-Preise](
https://azure.microsoft.com/pricing/details/active-directory-b2c/)

## <a name="step-1---create-an-azure-ad-b2c-tenant"></a>Schritt 1: Erstellen eines Azure AD B2C-Mandanten
B2C-Mandanten erstellen muss zuerst ausgeführt werden. Überspringen Sie diesen Schritt, wenn Sie Ihr Ziel B2C-Mandanten bereits erstellt haben. [Erste Schritte mit Azure AD B2C](active-directory-b2c-get-started.md)

## <a name="step-2---open-azure-portal-in-the-azure-ad-tenant-that-shows-your-azure-subscription"></a>Schritt 2 – öffnen Azure-Portal in Azure AD-Mandanten, die Ihrem Azure-Abonnement
Navigieren Sie zu der [Azure-Portal](https://portal.azure.com). Wechseln Sie zu den Azure AD-Mandanten, die dem Azure-Abonnement anzeigt, dass Sie verwenden möchten. Diese Azure AD-Mandanten unterscheidet sich von den B2C-Mandanten. Klicken Sie im Azure-Portal auf den Kontonamen in der oberen rechten Ecke des Dashboards, wählen Sie den Azure AD-Mandanten. Ein Azure-Abonnement ist erforderlich, um den Vorgang fortzusetzen. [Azure-Abonnement abschließen](https://account.windowsazure.com/signup?showCatalog=True)

![Wechseln zu Ihrem Azure AD-Mandanten](./media/active-directory-b2c-how-to-enable-billing/SelectAzureADTenant.png)

## <a name="step-3---create-a-b2c-tenant-resource-in-azure-marketplace"></a>Schritt 3: Erstellen einer Ressource B2C-Mandanten in Azure Marketplace
Öffnen Sie durch Klicken auf das Symbol "Marketplace", oder Auswählen der grüne "+" in der oberen linken Ecke des Dashboards Marketplace.  Suchen und Auswählen von Azure Active Directory B2C. Wählen Sie erstellen.

![Wählen Sie Marketplace](./media/active-directory-b2c-how-to-enable-billing/marketplace.png)

![Suchen von Active Directory B2C](./media/active-directory-b2c-how-to-enable-billing/searchb2c.png)

Die Azure AD B2C-Ressource erstellen Dialogfeld deckt die folgenden Parameter:

1. Azure AD B2C-Mandanten – einen Azure AD B2C-Mandanten in der Dropdownliste auswählen.  Zeigen nur berechtigte Azure AD B2C-Mandanten.  Geeignete B2C-Mandanten diese Bedingungen erfüllen: Sie sind der globale Administrator des B2C-Mandanten und B2C-Mandanten ist nicht mit einem Azure-Abonnement derzeit zugeordnet

2. Azure AD B2C-Ressourcenname - ist vorausgewählt, entsprechend den Domänennamen des B2C-Mandanten

3. Abonnement – ein aktives Azure-Abonnement in der Sie ein Administrator oder Co-Administrator sind.  Ein Azure-Abonnement können mehrere Azure AD B2C-Mandanten hinzugefügt werden

4. Speicherort der Ressourcengruppe und Ressourcengruppe – dieses Elements können Sie mehrere Azure-Ressourcen zu organisieren.  Diese Option hat keine Auswirkungen auf Ihre B2C-Mandanten Speicherort, Leistung oder Abrechnung status

5. Anheften an Dashboard für einfachste Zugriff auf Ihre Abrechnungsinformationen für B2C-Mandanten und die Einstellungen der B2C-Mandanten ![B2C-Ressource erstellen](./media/active-directory-b2c-how-to-enable-billing/createresourceb2c.png)

## <a name="step-4---manage-your-b2c-tenant-resources-optional"></a>Schritt 4: Verwalten von B2C-Mandanten (optional)
Nachdem die Bereitstellung abgeschlossen ist, wird eine neue "B2C-Mandanten"-Ressource wird in der Zielressourcengruppe erstellt und Azure-Abonnement verknüpft.  Eine neue Ressource vom Typ "B2C-Mandanten" hinzugefügte sollte angezeigt werden, zusammen mit anderen Azure-Ressourcen.

![Erstellen Sie B2C-Ressource](./media/active-directory-b2c-how-to-enable-billing/b2cresourcedashboard.png)

Durch Klicken auf die Ressource der B2C-Mandanten, können Sie
- Klicken Sie auf Abonnementname Abrechnungsinformationen überprüfen. Verwendung und Abrechnung finden Sie unter.
- Klicken Sie auf Azure AD B2C-Einstellungen, um eine neue Browserregisterkarte direkt in Ihrem B2C-Mandanten Blatt "Einstellungen" zu öffnen.
- Senden Sie eine Supportanfrage
- Verschieben Sie Ihre B2C-Mandanten-Ressource mit einem anderen Azure-Abonnement oder auf einer anderen Ressourcengruppe.  Diese Auswahl ändert sich, welche Azure-Abonnement Nutzungsgebühren empfängt.

![B2C ressourceneinstellungen](./media/active-directory-b2c-how-to-enable-billing/b2cresourcesettings.png)

## <a name="known-issues"></a>Bekannte Probleme
- Löschen von B2C-Mandanten. Wenn ein B2C-Mandanten erstellt wurde, gelöscht und neu erstellt, mit dem gleichen Domänennamen bitte auch löschen und Neuerstellen der Ressource "Verknüpfen" mit den gleichen Domänennamen an.  Diese Ressource "Verknüpfen" unter "Alle Ressourcen" finden Sie im Abonnement Mandanten über das Azure-Portal.
- Selbstauferlegte Einschränkungen für Land/Region Ressourcenspeicherort.  In seltenen Fällen kann kann ein Benutzer eine regionale Einschränkung für die Erstellung von Azure-Ressource eingerichtet haben.  Diese Einschränkung kann verhindern, dass die Erstellung von Links zwischen einem Azure-Abonnement und einem B2C-Mandanten. Um zu minimieren, weniger streng gehandhabt diese Einschränkung.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie diese Schritte für jede Ihrer B2C-Mandanten abgeschlossen haben, wird Ihr Azure-Abonnement in Übereinstimmung mit den Details Ihrer Azure direkte oder Enterprise Agreement in Rechnung gestellt.
- Überprüfen Sie die Nutzung und Abrechnung in Ihrem ausgewählten Azure-Abonnement
- Überprüfen Sie die detaillierte-Tag-von Verwendungsberichten mit der [Nutzungs-Reporting-API](active-directory-b2c-reference-usage-reporting-api.md)
