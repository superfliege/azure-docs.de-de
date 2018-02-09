---
title: "Entscheidungen zu nicht mit Azure verbundenen Bereitstellungen für in Azure Stack integrierte Systeme | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Entscheidungen bei der Bereitstellungsplanung für nicht mit Azure verbundenen Azure Stack-Bereitstellungen mit mehreren Knoten treffen."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: afea4b23aca322321b4096cdfdc9d30b087a2f30
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="azure-disconnected-deployment-planning-decisions-for-azure-stack-integrated-systems"></a>Planungsentscheidungen zu nicht mit Azure verbundenen Bereitstellungen für in Azure Stack integrierte Systeme
Nachdem Sie entschieden haben, [wie Sie Azure Stack in Ihre Hybrid Cloud-Umgebung integrieren](azure-stack-deployment-decisions.md), können Sie Ihre Azure Stack-Bereitstellungsentscheidungen abschließen.

Bei der Bereitstellungsoption ohne Verbindung mit Azure können Sie Azure Stack auch ohne Verbindung mit dem Internet bereitstellen und nutzen. Bei einer nicht mit Azure verbundenen Bereitstellung sind Sie jedoch auf einen AD FS-Identitätsspeicher und das kapazitätsbasierte Abrechnungsmodell beschränkt. 

Wählen Sie diese Option in den folgenden Fällen:
- Es gelten Sicherheits- oder andere Einschränkungen, die es erforderlich machen, Azure Stack in einer Umgebung bereitzustellen, die nicht mit dem Internet verbunden ist.
- Sie möchten verhindern, dass Daten (einschließlich Nutzungsdaten) an Azure gesendet werden.
- Sie möchten Azure Stack als reine private Cloudlösung nutzen, die im Intranet Ihres Unternehmens bereitgestellt wird, und interessieren sich nicht für Hybridszenarien.

> [!TIP]
> Mitunter wird diese Art von Umgebung auch mit einem U-Boot verglichen.

Eine nicht mit Azure verbundene Bereitstellung bedeutet nicht unbedingt, dass Sie später Ihre Azure Stack-Instanz nicht mit Azure für hybride VM-Mandantenszenarien verbinden können. Es bedeutet, dass Sie während der Bereitstellung keine Verbindung mit Azure haben bzw. Azure Active Directory nicht als Identitätsspeicher verwenden möchten. Wenn Sie jedoch nach der Bereitstellung eine Verbindung mit Azure wünschen, und zwar unabhängig davon, welchen Identitätsspeicher Sie verwenden möchten, sollten Sie die Bereitstellungsoption „Mit Azure verbinden“ wählen. 

## <a name="features-that-are-impaired-or-unavailable-in-disconnected-deployments"></a>Funktionen, die in von Azure getrennten Bereitstellungen eingeschränkt oder nicht verfügbar sind 
Azure Stack wurde so konzipiert, dass die Lösung am besten funktioniert, wenn sie mit Azure verbunden ist. Daher ist es wichtig zu beachten, dass es einige Features und Funktionen gibt, die entweder beeinträchtigt sind oder im von Azure getrennten Modus nicht verfügbar sind. 

|Feature|Beeinträchtigung im von Azure getrennten Modus|
|-----|-----|
|VM-Bereitstellung mit DSC-Erweiterung zum Konfigurieren von VMs nach der Bereitstellung|Eingeschränkt: DSC-Erweiterung sucht das neueste WMF im Internet.|
|VM-Bereitstellung mit Docker-Erweiterung zum Ausführen von Docker-Befehlen|Eingeschränkt: Docker durchsucht das Internet nach der neuesten Version, und diese Prüfung schlägt fehl.|
|Dokumentationslinks im Azure Stack-Portal|Nicht verfügbar: Links, z.B. Feedback geben, Hilfe, Schnellstart usw., die eine Internet-URL verwenden, funktionieren nicht.|
|Informationen zur Behebung von Warnungen, die auf eine online verfügbare Anleitung verweisen|Nicht verfügbar: Links zu einer Behebung von Warnungen, die eine Internet-URL verwenden, funktionieren nicht.|
|Marketplace-Syndikation: Die Möglichkeit, Katalogpakete direkt in Azure Marketplace auszuwählen und hinzuzufügen|Nicht verfügbar: Hierfür sind eine Verbindung mit Azure und ein Azure Active Directory-Konto erforderlich.|
|Verwenden von Azure Active Directory-Verbundkonten zur Verwaltung einer Azure Stack-Bereitstellung|Nicht verfügbar: Hierfür ist eine Verbindung mit Azure erforderlich. Stattdessen muss AD FS mit einer lokalen Active Directory-Instanz verwendet werden.|
|Ressourcenanbieter wie WebApps und SQL|Nicht verfügbar: Für Ressourcenanbieter wie WebApps und SQL ist Internetzugriff auf den Inhalt erforderlich.|
|Befehlszeilenschnittstelle (Command Line Interface, CLI)|Beeinträchtigt: Die CLI bietet weniger Funktionalität in Bezug auf die Authentifizierung und Bereitstellung von Dienstprinzipalen.|
|Visual Studio: Cloud Discovery|Beeinträchtigt: Cloud Discovery ermittelt entweder andere Clouds oder funktioniert überhaupt nicht.|
|Visual Studio: AD FS|Beeinträchtigt: Nur Visual Studio Enterprise unterstützt AD FS.
Telemetrie|Nicht verfügbar: Telemetriedaten für Azure Stack sowie alle Katalogpakete von Drittanbietern, die von Telemetriedaten abhängig sind.|
|Zertifikate|Nicht verfügbar: Im Kontext von HTTPS ist eine Internetverbindung für die Dienste Zertifikatsperrliste (Certificate Revocation List, CRL) und Online Certificate Status-Protokoll (OSCP) erforderlich.|
|Key Vault|Beeinträchtigt: Ein typischer Anwendungsfall für Key Vault ist das Lesen von Geheimnissen durch eine Anwendung zur Laufzeit. Dazu benötigt die Anwendung einen Dienstprinzipal im Verzeichnis. In Azure Active Directory ist es normalen Benutzern (die keine Administratoren sind) standardmäßig erlaubt, Dienstprinzipale hinzuzufügen. In AD (bei Verwenden von AD FS) dürfen sie das nicht. Dies stellt ein Hindernis für die Benutzer dar, da stets ein Verzeichnisadministrator benötigt wird, um eine Anwendung hinzuzufügen.| 

## <a name="learn-more"></a>Weitere Informationen
- Informationen zu Anwendungsfällen, zum Erwerb, zu Partnern und zu OEM-Hardwareanbietern finden Sie auf der Produktseite von [Azure Stack](https://azure.microsoft.com/overview/azure-stack/).
- Informationen zur Roadmap und geografischen Verfügbarkeit für integrierte Azure Stack-Systeme finden Sie im Whitepaper: [Azure Stack: Eine Erweiterung von Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
- Weitere Informationen zu Microsoft Azure Stack-Paketen und -Preisen finden Sie im Datenblatt zu [Paketerstellung und Preisen](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf). 

## <a name="next-steps"></a>Nächste Schritte
[Rechenzentrums-Netzwerkintegration](azure-stack-network.md)