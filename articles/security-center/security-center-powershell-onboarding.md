---
title: Verwenden von PowerShell zum Integrieren von Azure Security Center und zum Schützen Ihres Netzwerks | Microsoft-Dokumentation
description: In diesem Dokument wird der Vorgang zum Integrieren von Azure Security Center mithilfe von PowerShell-Cmdlets erläutert.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: e400fcbf-f0a8-4e10-b571-5a0d0c3d0c67
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/2/2018
ms.author: rkarlin
ms.openlocfilehash: 756aadfb015ada8ea642e9e4893664eed3f6c9b2
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48042551"
---
# <a name="automate-onboarding-of-azure-security-center-using-powershell"></a>Automatisieren der Integration von Azure Security Center mithilfe von PowerShell

Das Azure Security Center-PowerShell-Modul ermöglicht den programmgesteuerten Schutz Ihrer Azure-Workloads.
Mithilfe von PowerShell können Sie Tasks automatisieren und menschliche Fehler bei manuellen Aufgaben vermeiden. Dies ist vor allem bei umfangreichen Bereitstellungen mit Dutzenden Abonnements und Hunderttausenden Ressourcen praktisch, die alle von Anfang an geschützt werden müssen.

Integrieren Sie Azure Security Center mithilfe von PowerShell, um die Integration und Verwaltung Ihrer Azure-Ressourcen programmgesteuert zu automatisieren und die erforderlichen Sicherheitskontrollen hinzuzufügen.

Dieser Artikel enthält ein PowerShell-Beispielskript, das zur abonnementübergreifenden Einführung von Security Center angepasst und verwendet werden kann. 

In diesem Beispiel aktivieren Sie Security Center für ein Abonnement mit der ID d07c0080-170c-4c24-861d-9c817742786c und wenden die empfohlenen Einstellungen für hohen Schutz an. Dazu implementieren Sie den Standard-Tarif von Security Center, der erweiterte Funktionen für Bedrohungsschutz und Erkennung bietet:

1. Legen Sie die [Schutzstufe „Standard“ von ASC](https://azure.microsoft.com/pricing/details/security-center/) fest. 
 
2. Legen Sie den Log Analytics-Arbeitsbereich fest, an den Microsoft Monitoring Agent die Daten sendet, die auf den mit dem Abonnement verknüpften virtuellen Computern erfasst werden. In diesem Beispiel wird ein vorhandener benutzerdefinierter Arbeitsbereich (myWorkspace) verwendet.

3. Aktivieren Sie die automatische Agent-Bereitstellung von Security Center, bei der auch [Microsoft Monitoring Agent bereitgestellt](security-center-enable-data-collection.md#auto-provision-mma) wird.

5. Legen Sie den [CISO der Organisation als Sicherheitskontakt für ASC-Warnungen und relevante Ereignisse](security-center-provide-security-contact-details.md) fest.

6. Weisen Sie die [Standardsicherheitsrichtlinien](security-center-azure-policy.md) von Security Center zu.

## <a name="prerequisites"></a>Voraussetzungen

Diese Schritte sollten vor der Ausführung der Security Center-Cmdlets ausgeführt werden:

1.  Führen Sie PowerShell als Administrator aus.
2.  Führen Sie die folgenden Befehle in PowerShell aus:
      
        Install-Module -Name PowerShellGet -Force
        Set-ExecutionPolicy -ExecutionPolicy AllSigned
        Import-Module PowerShellGet
6.  Starten Sie PowerShell neu.

7. Führen Sie in PowerShell die folgenden Befehle aus:

         Install-Module -Name AzureRM.Security -AllowPrerelease -Force

## <a name="onboard-security-center-using-powershell"></a>Integrieren von Security Center mithilfe von PowerShell

1.  Registrieren Sie Ihre Abonnements beim Security Center-Ressourcenanbieter:

        Set-AzureRmContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
        Register-AzureRmResourceProvider -ProviderNamespace 'Microsoft.Security' 

2.  Optional: Legen Sie die Abdeckungsebene (Tarif) der Abonnements fest. (Wird keine Angabe gemacht, wird als Tarif „Free“ festgelegt.)

        Set-AzureRmContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
        Set-AzureRmSecurityPricing -Name "default" -PricingTier "Standard"

3.  Konfigurieren Sie einen Log Analytics-Arbeitsbereich, an den die Agents Daten melden. Sie benötigen einen bereits erstellten Log Analytics-Arbeitsbereich, an den die virtuellen Computer im Abonnement Daten melden können. Sie können mehrere Abonnements festlegen, die an den gleichen Arbeitsbereich Daten melden. Wird keine Angabe gemacht, wird der Standardarbeitsbereich verwendet.

        Set-AzureRmSecurityWorkspaceSetting -Name "default" -Scope
        "/subscriptions/d07c0080-170c-4c24-861d-9c817742786c" -WorkspaceId"/subscriptions/d07c0080-170c-4c24-861d-9c817742786c/resourceGroups/myRg/providers/Microsoft.OperationalInsights/workspaces/myWorkspace"

4.  Installation mit automatischer Bereitstellung von Microsoft Monitoring Agent auf Ihren virtuellen Azure-Computern:
    
        Set-AzureRmContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
    
        Set-AzureRmSecurityAutoProvisioningSetting -Name "default" -EnableAutoProvision

    > [!NOTE]
    > Sie sollten die automatische Bereitstellung aktivieren, um sicherzustellen, dass Ihre virtuellen Azure-Computer automatisch mit Azure Security Center geschützt werden.
    >

5.  Optional: Es wird dringend empfohlen, die Sicherheitskontaktinformationen für die zu integrierenden Abonnements anzugeben. Die Sicherheitskontakte erhalten die von Security Center generierten Warnungen und Benachrichtigungen:

        Set-AzureRmSecurityContact -Name "default1" -Email "CISO@my-org.com" -Phone "2142754038" -AlertsAdmin -NotifyOnAlert 

6.  Weisen Sie die Security Center-Standardrichtlinieninitiative zu:

        Register-AzureRmResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
        $Policy = Get-AzureRmPolicySetDefinition -Name ' [Preview]: Enable Monitoring in Azure Security Center'
        New-AzureRmPolicyAssignment -Name 'ASC Default <d07c0080-170c-4c24-861d-9c817742786c>' -DisplayName 'Security Center Default <subscription ID>' -PolicySetDefinition $Policy -Scope '/subscriptions/d07c0080-170c-4c24-861d-9c817742786c'

Sie haben Azure Security Center nun mithilfe von PowerShell integriert.

Diese PowerShell-Cmdlets können nun mit Automatisierungsskripts verwendet werden, um Abonnements und Ressourcen programmgesteuert zu durchlaufen. Das spart Zeit und trägt dazu bei, menschliche Fehler zu vermeiden. Sie können dieses [Beispielskript](https://github.com/Microsoft/Azure-Security-Center/blob/master/quickstarts/ASC-Samples.ps1) als Referenz verwenden.






## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zum Automatisieren der Integration in Security Center mithilfe von PowerShell finden Sie im folgenden Artikel:

* [AzureRM.Security](https://www.powershellgallery.com/packages/AzureRM.Security/0.2.0-preview)

Weitere Informationen zu Security Center finden Sie in den folgenden Artikeln:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md) : Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
