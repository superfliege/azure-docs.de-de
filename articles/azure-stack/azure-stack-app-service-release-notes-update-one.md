---
title: App Service in Azure Stack – Update 1 | Microsoft-Dokumentation
description: Erfahren Sie, was in Update 1 für App Service in Azure Stack enthalten ist, welche bekannten Probleme es gibt und wo das Update heruntergeladen werden kann.
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2018
ms.author: anwestg
ms.reviewer: brenduns
ms.openlocfilehash: 0c33c8fdefbb27ba8414e58bed1b42ee7aaba88a
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="app-service-on-azure-stack-update-one-release-notes"></a>App Service in Azure Stack – Versionshinweise zu Update 1

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Diese Versionshinweise beschreiben die Verbesserungen und Fehlerbehebungen in Update 1 für Azure App Service in Azure Stack und bekannte Probleme. Die bekannten Probleme sind in Probleme unterteilt, die sich direkt auf die Bereitstellung und den Updateprozess beziehen, und in Probleme mit dem Build (nach der Installation).

> [!IMPORTANT]
> Wenden Sie Update 1802 auf Ihr integriertes Azure Stack-System an, oder stellen Sie das aktuelle Azure Stack-Development Kit vor der Bereitstellung von Azure App Service bereit.
>
>

## <a name="build-reference"></a>Buildreferenz

Die Buildnummer von Update 1 für App Service in Azure Stack ist **69.0.13698.9**.

### <a name="prerequisites"></a>Voraussetzungen

> [!IMPORTANT]
> Azure App Service in Azure Stack erfordert jetzt aufgrund von Verbesserungen der Art, in der SSO für Kudu jetzt in Azure App Service behandelt wird, ein [Platzhalterzertifikat mit drei Antragstellern](azure-stack-app-service-before-you-get-started.md#get-certificates).  Der neue Antragsteller ist ** *.sso.appservice.<region>. <domainname>.<extension>**
>
>

Lesen Sie die Dokumentation [Vor den ersten Schritten mit App Service in Azure Stack](azure-stack-app-service-before-you-get-started.md), bevor Sie mit der Bereitstellung beginnen.

### <a name="new-features-and-fixes"></a>Neue Features und Fehlerbehebungen

Update 1 für Azure App Service in Azure Stack enthält die folgenden Verbesserungen und Fehlerbehebungen:

- **Hohe Verfügbarkeit von Azure App Service**: Das Azure Stack-Update 1802 ermöglicht die Fehlerdomänen übergreifende Bereitstellung von Workloads.  Daher kann die App Service-Infrastruktur fehlertolerant sein, da sie Fehlerdomänen übergreifend bereitgestellt wird.  Standardmäßig haben alle neuen Bereitstellungen von Azure App Service diese Funktion, doch lesen Sie zu Bereitstellungen, die vor Anwendung des Azure Stack-Updates 1802 durchgeführt wurden, die Dokumentation [Fehlerdomänen übergreifendes Neuverteilen von Azure App Service in Azure Stack](azure-stack-app-service-fault-domain-update.md).

- **In vorhandenem virtuellem Netzwerk bereitstellen**: Kunden können jetzt App Service in Azure Stack in einem vorhandenen virtuellen Netzwerk bereitstellen.  Die Bereitstellung in einem vorhandenen virtuellen Netzwerk ermöglicht Kunden, eine Verbindung mit der SQL Server- und Dateiserverinstanz – für Azure App Service erforderlich – über private Ports herzustellen.  Während der Bereitstellung können die Kunden wählen, ob sie die Bereitstellung in einem vorhandenen virtuellen Netzwerk wünschen, doch vor der Bereitstellung [müssen sie Subnetze für die Verwendung durch App Service](azure-stack-app-service-before-you-get-started.md#virtual-network) erstellen.

- Updates für **App Service-Mandanten, Admin, Functions-Portale und Kudu-Tools**.  Mit Azure Stack-Portal-SDK-Version konsistent.

- **Updates für folgende Anwendungsframeworks und Tools**:
    - Unterstützung für **.Net Core 2.0** wurde hinzugefügt.
    - Hinzugefügte **Node.JS**-Versionen:
        - 6.11.2
        - 6.11.5
        - 7.10.1
        - 8.0.0
        - 8.1.4
        - 8.4.0
        - 8.5.0
        - 8.7.0
        - 8.8.1
        - 8.9.0
    - Hinzugefügte **NPM**-Versionen:
        - 3.10.10
        - 4.2.0
        - 5.0.0
        - 5.0.3
        - 5.3.0
        - 5.4.2
        - 5.5.1
    - Hinzugefügte **PHP**-Updates:
        - 5.6.32
        - 7.0.26 (x86 und x64)
        - 7.1.12 (x86 und x64)
    - **Git für Windows** wurde auf v2.14.1 aktualisiert.
    - **Mercurial** wurde auf v4.5.0 aktualisiert.

  - Unterstützung für das **Nur HTTPS**-Feature wurde innerhalb des Features „Benutzerdefinierte Domäne“ im App Service-Mandantenportal hinzugefügt. 

  - Überprüfung der Speicherverbindung in der Auswahl von benutzerdefiniertem Speicher für Azure Functions wurde hinzugefügt. 

#### <a name="fixes"></a>Fehlerbehebungen

- Beim Erstellen eines Offlinebereitstellungspakets erhalten Kunden keine „Zugriff verweigert“-Fehlermeldung mehr, wenn sie den Ordner vom App Service-Installationsprogramm aus öffnen.

- Probleme beim Arbeiten im Feature „Benutzerdefinierte Domänen“ im App Service-Mandantenportal wurden gelöst.

- Es wird verhindert, dass Kunden reservierte Administratornamen während des Setups verwenden.

- App Service-Bereitstellung mit **in die Domäne eingebundenem** Dateiserver wurde ermöglicht.

- Der Abruf des Azure Stack-Stammzertifikats im Skript und jetzt das Überprüfen des Stammzertifikats im App Service-Installationsprogramm wurden verbessert.

- Der Fehler, dass beim Löschen eines Abonnements, dass Ressourcen im Microsoft.Web-Namespace enthielt, ein falscher Status an den Azure Resource Manager zurückgegeben wurde, wurde behoben.

### <a name="known-issues-with-the-deployment-process"></a>Bekannte Probleme mit dem Bereitsellungsprozess

- Es sind keine Probleme bei der Bereitstellung von Update 1 für Azure App Service in Azure Stack bekannt.

### <a name="known-issues-with-the-update-process"></a>Bekannte Probleme mit dem Updateprozess

- Es sind keine Probleme beim Aktualisieren mit Update 1 für Azure App Service in Azure Stack bekannt.

### <a name="known-issues-post-installation"></a>Bekannte Probleme (nach der Installation)

- Es sind keine Probleme bei der Installation von Update 1 für Azure App Service in Azure Stack bekannt.

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Bekannte Probleme von Cloudadministratoren, die Azure App Service in Azure Stack betreiben

Lesen Sie die Dokumentation in den Versionshinweisen [Azure Stack-Update 1802](azure-stack-update-1802.md).

## <a name="see-also"></a>Weitere Informationen

- Einen Überblick über Azure App Service finden Sie unter [Übersicht über App Service in Azure Stack](azure-stack-app-service-overview.md).
- Weitere Informationen zum Vorbereiten der Bereitstellung von App Service in Azure Stack finden Sie unter [Vor den ersten Schritten mit App Service in Azure Stack](azure-stack-app-service-before-you-get-started.md).
