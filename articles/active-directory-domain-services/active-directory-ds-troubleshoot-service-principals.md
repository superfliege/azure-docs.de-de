---
title: 'Azure Active Directory Domain Services: Beheben von Problemen bei der Konfiguration des Dienstprinzipals | Microsoft-Dokumentation'
description: "Beheben von Problemen bei der Konfiguration des Dienstprinzipals für Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: f168870c-b43a-4dd6-a13f-5cfadc5edf2c
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: ergreenl
ms.openlocfilehash: e6144a4018f7fbe7dbf7b4693e3f41884e4a80a2
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshooting-service-principal-configuration"></a>Azure AD Domain Services: Beheben von Problemen bei der Konfiguration des Dienstprinzipals

Microsoft verwendet zum Bereitstellen, Verwalten und Aktualisieren Ihrer Domäne verschiedene [Dienstprinzipale](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-application-objects) für die Kommunikation mit Ihrem Verzeichnis. Wenn einer davon falsch konfiguriert oder gelöscht wird, kann dies zu einer Unterbrechung Ihres Diensts führen.

## <a name="aadds102-service-principal-not-found"></a>AADDS102: Dienstprinzipal nicht gefunden

**Warnmeldung:**

*Ein für den ordnungsgemäßen Betrieb von Azure AD Domain Services erforderlicher Dienstprinzipal wurde aus Ihrem Azure AD-Verzeichnis gelöscht. Diese Konfiguration wirkt sich darauf aus, wie Microsoft Ihre verwaltete Domäne überwachen, verwalten, patchen und synchronisieren kann.*

Dienstprinzipale sind Anwendungen, die Microsoft zum Verwalten und Aktualisieren Ihrer verwalteten Domäne verwendet. Wenn diese gelöscht werden, kann Microsoft Ihre Domäne nicht mehr im Betrieb halten. Ermitteln Sie anhand der folgenden Schritte, welche Dienstprinzipale neu erstellt werden müssen.

1. Navigieren Sie im Azure-Portal zur Seite [Unternehmensanwendungen – Alle Anwendungen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps).
2. Wählen Sie in der Dropdownliste „Anzeigen“ die Option **Alle Anwendungen** aus, und klicken Sie auf **Anwenden**.
3. Nutzen Sie die folgende Tabelle für die Suche der einzelnen Anwendungs-IDs, indem Sie die ID in das Suchfeld einfügen und die EINGABETASTE drücken. Wenn keine Suchergebnisse angezeigt werden, müssen Sie den Dienstprinzipal mithilfe der Schritte in der Spalte „Lösung“ neu erstellen.

| Anwendungs-ID | Lösung |
| :--- | :--- | :--- |
| 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [Erneutes Erstellen eines fehlenden Dienstprinzipals mit PowerShell](#recreate-a-missing-service-principal-with-powershell) |
| 443155a6-77f3-45e3-882b-22b3a8d431fb | [Erneutes Registrieren beim Namespace „Microsoft.AAD“](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| abba844e-bc0e-44b0-947a-dc74e5d09022  | [Erneutes Registrieren beim Namespace „Microsoft.AAD“](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [Dienstprinzipale mit Selbstkorrektur](#service-principals-that-self-correct) |

### <a name="recreate-a-missing-service-principal-with-powershell"></a>Erneutes Erstellen eines Dienstprinzipals mit PowerShell

*Befolgen Sie diese Schritte für fehlende IDs: 2565bd9d-da50-47d4-8b85-4c97f669dc36*.

**Problembehandlung**:

Sie benötigen Azure AD PowerShell, um diese Schritte durchzuführen. Weitere Informationen zum Installieren von Azure AD PowerShell finden Sie [in diesem Artikel](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Um dieses Problem zu beheben, geben Sie die folgenden Befehle in einem PowerShell-Fenster ein:
1. Install-Module AzureAD
2. Import-Module AzureAD
3. Überprüfen Sie, ob der für Azure AD Domain Services erforderliche Dienstprinzipal in Ihrem Verzeichnis fehlt, indem Sie den folgenden PowerShell-Befehl ausführen:
      ```PowerShell
      Get-AzureAdServicePrincipal -filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
      ```
4. Erstellen Sie den Dienstprinzipal, indem Sie den folgenden PowerShell-Befehl eingeben:
     ```PowerShell
     New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
     ```
5. Nachdem Sie den fehlenden Dienstprinzipal erstellt haben, warten Sie zwei Stunden, und überprüfen Sie dann die Integrität Ihrer Domäne.


### <a name="re-register-to-the-microsoft-aad-namespace-using-the-azure-portal"></a>Erneutes Registrieren beim Namespace „Microsoft.AAD“ über das Azure-Portal

*Befolgen Sie diese Schritte für fehlende IDs: 443155a6-77f3-45e3-882b-22b3a8d431fb und abba844e-bc0e-44b0-947a-dc74e5d09022*.

**Problembehandlung**:

Stellen Sie Domain Services anhand der folgenden Schritte in Ihrem Verzeichnis wieder her:

1. Navigieren Sie im Azure-Portal zur Seite [Abonnements](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
2. Wählen Sie das Abonnement in der Tabelle, die Ihrer verwalteten Domäne zugeordnet ist, aus.
3. Wählen Sie links auf der Navigationsleiste **Ressourcenanbieter** aus.
4. Suchen Sie in der Tabelle nach „Microsoft.AAD“, und klicken Sie auf **Erneut registrieren**.
5. Um zu überprüfen, ob die Warnung aufgelöst wurde, zeigen Sie die Warnungsseite zur Integrität in zwei Stunden erneut an.


### <a name="service-principals-that-self-correct"></a>Dienstprinzipale mit Selbstkorrektur

*Befolgen Sie diese Schritte für fehlende IDs: d87dcbc6-a371-462e-88e3-28ad15ec4e64*

**Problembehandlung**:

Microsoft kann ermitteln, wenn bestimmte Dienstprinzipale fehlen, falsch konfiguriert sind oder gelöscht wurden. Um den Dienst schnell zu korrigieren, erstellt Microsoft die Dienstprinzipale selbst neu. Überprüfen Sie die Integrität Ihrer Domäne nach zwei Stunden erneut, um sicherzustellen, dass der Prinzipal neu erstellt wurde.

## <a name="contact-us"></a>So erreichen Sie uns
Wenden Sie sich an das Produktteam der Azure Active Directory Domain Services, um [Feedback zu geben oder Support zu erhalten](active-directory-ds-contact-us.md).
