---
title: Reaktivieren deaktivierter Azure Access Control Service (ACS)-Namespaces
description: Erfahren Sie, wie Sie Ihre Azure Access Control Service (ACS)-Namespaces finden und aktivieren können, und fordern Sie eine Erweiterung an, um die Aktivierung bis zum 4. Februar 2019 beizubehalten.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/07/2018
ms.author: celested
ms.reviewer: jlu
ms.custom: aaddev
ms.openlocfilehash: 298afda7549690a9ea0314bff63a714be50a33b9
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51019881"
---
# <a name="how-to-reactivate-disabled-access-control-service-namespaces"></a>Vorgehensweise: Reaktivieren deaktivierter Access Control Service-Namespaces

Im November 2017 haben wir bekanntgegeben, dass Microsoft Azure Access Control Service (ACS), ein Dienst von Azure Active Directory (Azure AD), wird am 7. November 2018 eingestellt wird.

Wir haben 12 Monate, 9 Monate, 6 Monate, 3 Monate, 1 Monat, 2 Wochen, 1 Woche und 1 Tag vor der Einstellung am 7. November 2018 mehrere E-Mails bezüglich der Einstellung von ACS an die Verwaltungs-E-Mail der ACS-Abonnements gesendet.

Am 3. Oktober 2018 gaben wir (per E-Mail und [einem Blogbeitrag](https://azure.microsoft.com/blog/one-month-retirement-notice-access-control-service/)) ein Verlängerungsangebot für Kunden bekannt, die ihre Migration nicht vor dem 7. November 2018 abschließen können. Die Ankündigung enthielt auch Anweisungen zum Anfordern der Verlängerung.

## <a name="why-your-namespace-is-disabled"></a>Darum wird Ihr Namespace deaktiviert

Wenn Sie sich nicht für die Verlängerung entschieden haben, werden wir ab dem 7. November 2018 ACS-Namespaces deaktivieren. Wenn Sie die Mitteilungen verpasst haben und sich trotzdem für die Verlängerung bis zum 4. Februar 2019 entscheiden möchten, folgen Sie den Anweisungen in den folgenden Abschnitten.

> [!NOTE]
> Sie müssen ein Administrator des Abonnements sein, um die PowerShell-Befehle auszuführen und eine Verlängerung anzufordern.

## <a name="find-and-enable-your-acs-namespaces"></a>Suchen und Aktivieren Ihrer ACS-Namespaces

Sie können ACS PowerShell verwenden, um alle Ihre ACS-Namespaces aufzulisten und deaktivierte zu reaktivieren.

1. Herunterladen und Installieren von ACS PowerShell:
    1. Wechseln Sie zum PowerShell-Katalog, und laden Sie [Acs.Namespaces](https://www.powershellgallery.com/packages/Acs.Namespaces/1.0.2) herunter.
    1. Installieren des Moduls:

        ```powershell
        Install-Module -Name Acs.Namespaces
        ```

    1. Rufen Sie eine Liste mit den möglichen Befehlen ab:

        ```powershell
        Get-Command -Module Acs.Namespaces
        ```

        Führen Sie Folgendes aus, um Hilfe zu einem bestimmten Befehl zu erhalten:

        ```
        Get-Help [Command-Name] -Full
        ```
    
        Hierbei steht `[Command-Name]` für den Namen des ACS-Befehls.
1. Listen Sie Ihre verfügbaren Azure-Abonnements auf, indem Sie das Cmdlet **Get-AcsSubscription** verwenden.
1. Listen Sie Ihre ACS-Namespaces auf, indem Sie das Cmdlet **Get-AcsNamespace** verwenden.
1. Bestätigen Sie, dass die Namespaces deaktiviert sind, indem Sie bestätigen, dass `State` `Disabled` ist.

    [![Bestätigen, dass die Namespaces deaktiviert sind](./media/howto-reactivate-disabled-acs-namespaces/confirm-disabled-namespace.png)](./media/howto-reactivate-disabled-acs-namespaces/confirm-disabled-namespace.png#lightbox)

    Sie können auch `nslookup {your-namespace}.accesscontrol.windows.net` verwenden, um zu überprüfen, ob die Domäne noch aktiv ist.

1. Aktivieren Sie Ihre ACS-Namespaces, indem Sie das Cmdlet **Enable-AcsNamespace** verwenden.

    Nachdem Sie Ihre Namespaces aktiviert haben, können Sie eine Verlängerung beantragen, damit die Namespaces nicht vor dem 4. Februar 2019 wieder deaktiviert werden. Nach diesem Datum tritt bei allen Anforderungen an ACS ein Fehler auf.

## <a name="request-an-extension"></a>Anfordern einer Verlängerung

1. Navigieren Sie zum Verwaltungsportal Ihres ACS-Namespace, indem Sie zu `https://{your-namespace}.accesscontrol.windows.net` navigieren.
1. Wählen Sie die Schaltfläche **Bedingungen lesen**, um die [aktualisierten Nutzungsbedingungen](https://azure.microsoft.com/support/legal/access-control/) zu lesen, die Sie zu einer Seite mit den aktualisierten Nutzungsbedingungen führen.

    [![Auswählen der Schaltfläche „Bedingungen lesen“](./media/howto-reactivate-disabled-acs-namespaces/read-terms-button-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/read-terms-button-expanded.png#lightbox)

1. Wählen Sie **Verlängerung anfordern** im Banner am oberen Rand der Seite. Die Schaltfläche wird erst aktiviert werden, nachdem Sie die [aktualisierten Nutzungsbedingungen](https://azure.microsoft.com/support/legal/access-control/) gelesen haben.

    [![Auswählen der Schaltfläche „Verlängerung anfordern“](./media/howto-reactivate-disabled-acs-namespaces/request-extension-button-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/request-extension-button-expanded.png#lightbox)

1. Nachdem die Verlängerungsanforderung registriert wurde, wird die Seite mit einem neuen Banner oben auf der Seite aktualisiert.

    [![Aktualisierte Seite mit aktualisiertem Banner](./media/howto-reactivate-disabled-acs-namespaces/updated-banner-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/updated-banner-expanded.png#lightbox)

## <a name="help-and-support"></a>Hilfe und Support

- Wenn Sie nach dem Befolgen dieser Anleitung auf Probleme stoßen, wenden Sie sich an den [Azure-Support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).
- Wenn Sie Fragen oder Feedback zur Einstellung von ACS haben, kontaktieren Sie uns unter acsfeedback@microsoft.com.

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die Informationen zur ACS-Einstellung in [Vorgehensweise: Migrieren von Azure Access Control Service](active-directory-acs-migration.md).
