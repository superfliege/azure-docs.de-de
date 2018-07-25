---
title: Grundlegendes zu Azure Stack-Sicherheitsmaßnahmen | Microsoft-Dokumentation
description: Als Dienstadministrator erhalten Sie Informationen zu Sicherheitsmaßnahmen, die auf Azure Stack angewendet werden
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: mabrigg
ms.openlocfilehash: a3bd314a1df3c45c76b2e3a5acb31c1474d0fdf5
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39011252"
---
# <a name="azure-stack-infrastructure-security-posture"></a>Sicherheitsstatus der Azure Stack-Infrastruktur

*Gilt für: Integrierte Azure Stack-Systeme*

Sicherheitsüberlegungen und Konformitätsbestimmungen gehören zu den wichtigsten Beweggründen für die Verwendung von Hybrid Clouds. Azure Stack wurde für diese Szenarien konzipiert, und es ist wichtig, bei der Einführung von Azure Stack über die bereits vorhandenen Sicherheitsmaßnahmen informiert zu sein.

In Azure Stack sind zwei Sicherheitsstatusebenen vorhanden. Die erste Ebene ist die Azure Stack-Infrastruktur, die sämtliche Komponenten von den Hardwarekomponenten bis hin zum Azure Resource Manager umfasst. Die erste Ebene beinhaltet das Administrator- und Mandantenportal. Die zweite Ebene besteht aus den Workloads, die von Mandanten erstellt, bereitgestellt und verwaltet werden. Die zweite Ebene beinhaltet Elemente wie virtuelle Computer und App Service-Websites.

## <a name="security-approach"></a>Sicherheitsansatz

Der Sicherheitsstatus für Azure Stack wurde für die Abwehr moderner Bedrohungen und die Erfüllung der Anforderungen der wichtigsten Compliancestandards konzipiert. Daraus resultiert, dass der Sicherheitsstatus der Azure Stack-Infrastruktur auf zwei Säulen ruht:

 - **Von einer Sicherheitsverletzung ausgehen**  
Von der Annahme ausgehend, dass bereits eine systemseitige Sicherheitsverletzung vorliegt, konzentrieren wir uns auf das *Erkennen und Beschränken der Auswirkungen von Sicherheitsverletzungen*, anstatt nur zu versuchen, Angriffe zu verhindern. 
 - **Standardmäßig verstärkt**  
Da die Infrastruktur auf klar definierter Hardware und Software ausgeführt wird, *aktiviert, konfiguriert und überprüft Azure Stack standardmäßig alle Sicherheitsfeatures*.

Da Azure Stack als integriertes System geliefert wird, wird der Sicherheitsstatus der Azure Stack-Infrastruktur von Microsoft definiert. Genau wie in Azure sind Mandanten dafür verantwortlich, den Sicherheitsstatus ihrer Mandantenworkloads zu definieren. Dieses Dokument vermittelt grundlegende Kenntnisse über den Sicherheitsstatus der Azure Stack-Infrastruktur.

## <a name="data-at-rest-encryption"></a>Verschlüsselung für ruhende Daten
Alle Azure Stack-Infrastruktur- und Mandantendaten sind im Ruhezustand mit Bitlocker verschlüsselt. Diese Verschlüsselung bietet Schutz vor physischem Verlust bzw. Diebstahl von Azure Stack-Speicherkomponenten. 

## <a name="data-in-transit-encryption"></a>Verschlüsselung von Daten während der Übertragung
Die Azure Stack-Infrastrukturkomponenten kommunizieren mithilfe von Kanälen, die mit TLS 1.2 verschlüsselt sind. Verschlüsselungszertifikate werden von der Infrastruktur selbst verwaltet. 

Alle externen Infrastrukturendpunkte, z.B. die REST-Endpunkte oder das Azure Stack-Portal, unterstützen TLS 1.2 für die sichere Kommunikation. Verschlüsselungszertifikate, die entweder von einem Drittanbieter oder der Zertifizierungsstelle Ihres Unternehmens ausgestellt werden, müssen für diese Endpunkte bereitgestellt werden. 

Für diese externen Endpunkte können zwar selbstsignierte Zertifikate verwendet werden, doch Microsoft rät dringend von deren Verwendung ab. 

## <a name="secret-management"></a>Verwaltung von Geheimnissen
In der Azure Stack-Infrastruktur werden zahlreiche Geheimnisse eingesetzt, wie z.B. Kennwörter. Die meisten von ihnen werden automatisch gedreht, da sie gruppenverwaltete Dienstkonten sind, die alle 24 Stunden gedreht werden.

Die verbleibenden Geheimnisse, die keine gruppenverwalteten Dienstkonten sind, können manuell mit einem Skript im privilegierten Endpunkt gedreht werden.

## <a name="code-integrity"></a>Codeintegrität
Azure Stack nutzt die neuesten Sicherheitsfeatures von Windows Server 2016. Eines davon ist Windows Defender Device Guard. Dieses Tool bietet Anwendungswhitelists und stellt sicher, dass nur autorisierter Code innerhalb der Azure Stack-Infrastruktur ausgeführt wird. 

Autorisierter Code ist entweder von Microsoft oder dem OEM-Partner signiert und in der Liste der zulässigen Software enthalten, die in einer von Microsoft definierten Richtlinie angegeben ist. Anders gesagt kann nur Software ausgeführt werden, die zur Ausführung in der Azure Stack-Infrastruktur genehmigt wurde. Jeder Versuch, nicht autorisierten Code auszuführen, wird blockiert, und eine Überwachung wird generiert.

Die Device Guard-Richtlinie verhindert auch die Ausführung von Drittanbieter-Agents oder -Software in der Azure Stack-Infrastruktur.

## <a name="credential-guard"></a>Credential Guard
Ein anderes Windows Server 2016-Sicherheitsfeature in Azure Stack ist Windows Defender Credential Guard. Dieses Tool schützt Anmeldeinformationen der Azure Stack-Infrastruktur vor Pass-the-Hash- und Pass-the-Ticket-Angriffen.

## <a name="antimalware"></a>Antimalware
Jede Komponente in Azure Stack (Hyper-V-Hosts und virtuelle Computer) wird mit Windows Defender Antivirus geschützt.

In Szenarien mit bestehender Verbindung werden mehrmals täglich Updates für Antivirendefinitionen und -programme angewendet. In Szenarien mit getrennter Verbindung werden Updates für Antischadsoftware im Rahmen der monatlichen Azure Stack-Updates angewendet. Weitere Informationen finden Sie unter [Aktualisieren von Windows Defender Antivirus in Azure Stack](azure-stack-security-av.md).

## <a name="constrained-administration-model"></a>Eingeschränktes Verwaltungsmodell
Die Verwaltung in Azure Stack wird mit drei Einstiegspunkten gesteuert, von denen jeder einen bestimmten Zweck hat: 
1. Das [Administratorportal](azure-stack-manage-portals.md) bietet eine Point-and-Click-Benutzeroberfläche für tägliche Verwaltungsvorgänge.
2. Azure Resource Manager macht alle Verwaltungsvorgänge des Administratorportals über eine REST-API verfügbar, die von PowerShell und Azure CLI verwendet wird. 
3. Für bestimmte Low-Level-Vorgänge wie Rechenzentrumsintegration oder Unterstützungsszenarien macht Azure Stack einen als [privilegierten Endpunkt](azure-stack-privileged-endpoint.md) bezeichneten PowerShell-Endpunkt verfügbar. Dieser Endpunkt macht nur einen zugelassenen Satz von Cmdlets verfügbar und wird gründlich überwacht.

## <a name="network-controls"></a>Netzwerksteuerungen
Die Azure Stack-Infrastruktur umfasst mehrere Ebenen der Zugriffssteuerungsliste (Access Control List, ACL) für das Netzwerk. Die ACLs verhindern unbefugten Zugriff auf die Infrastrukturkomponenten und begrenzen die Infrastrukturkommunikation auf die Pfade, die für ihr Funktionieren erforderlich sind. 

Netzwerk-ACLs werden in drei Ebenen durchgesetzt:
1.  Top-of Rack-Switches
2.  Softwaredefiniertes Netzwerk
3.  Host- und VM-Betriebssystemfirewalls

## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie, wie Sie Ihre Geheimnisse in Azure Stack rotieren.](azure-stack-rotate-secrets.md)
