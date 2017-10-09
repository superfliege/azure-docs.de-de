---
title: Was ist Azure Stack? | Microsoft Docs
description: "Azure Stack ermöglicht die Ausführung von Azure-Diensten in Ihrem Datencenter."
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 09/25/2017
ms.author: helaw
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 950ba44c0b7eb80c9b0a3c69a9fca03cd244576d
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="what-is-azure-stack"></a>Was ist Azure Stack?

Microsoft Azure Stack ist eine Hybrid Cloud-Plattform, mit der Sie Azure-Dienste über das Rechenzentrum Ihrer Organisation bereitstellen können.  Azure Stack soll in gängigen Szenarien (etwa in Edge-Umgebungen und getrennten Umgebungen) neue Möglichkeiten für Ihre modernen Anwendungen eröffnen und zur Erfüllung spezifischer Sicherheits- und Konformitätsanforderungen beitragen.  Azure Stack wird in zwei auf Ihre Anforderungen abgestimmten Bereitstellungsoptionen angeboten.

## <a name="azure-stack-integrated-systems"></a>Integrierte Azure Stack-Systeme
Integrierte Azure Stack-Systeme werden im Rahmen einer Partnerschaft zwischen Microsoft und [Hardwarepartnern](https://azure.microsoft.com/overview/azure-stack/integrated-systems/) angeboten. Dadurch entsteht eine ausgewogene Lösung mit cloudbasierten Innovationen und komfortabler Verwaltung.  Bei Azure Stack handelt es sich um ein integriertes System mit Hardware und Software. Somit bietet die Plattform genau das richtige Maß an Flexibilität und Kontrolle bei gleichzeitiger Bereitstellung von cloudbasierten Innovationen.  Integrierte Azure Stack-Systeme haben eine Größe von vier bis zwölf Knoten, und der Support wird vom Hardwarepartner und von Microsoft gemeinsam bereitgestellt.  Mit integrierten Azure Stack-Systemen ermöglichen Sie neue Szenarien für Ihre Produktionsworkloads.    

## <a name="azure-stack-development-kit"></a>Azure Stack Development Kit
Microsoft Azure Stack Development Kit ist eine Einzelknotenbereitstellung von Azure Stack, mit der Sie Azure Stack auswerten und näher kennenlernen können.  Sie können auch Azure Stack Development Kit als Entwicklerumgebung verwenden, in der Sie anhand von mit Azure konsistenten APIs und Tools Entwicklungsarbeiten durchführen können.  Azure Stack Development Kit ist nicht für die Verwendung als Produktionsumgebung konzipiert.

Für das Azure Stack Development Kit gelten die folgenden Einschränkungen:
* Das Azure Stack Development Kit ist einem einzigen Azure Active Directory- oder AD FS-Identitätsanbieter (Active Directory Federation Services, Active Directory-Verbunddienste) zugeordnet. Sie können mehrere Benutzer in diesem Verzeichnis erstellen und jedem Benutzer Abonnements zuweisen.
* Da alle Komponenten auf einem einzigen Computer bereitgestellt werden, stehen für Mandantenressourcen nur begrenzte physische Ressourcen zur Verfügung. Diese Konfiguration ist nicht für die Beurteilung von Skalierung oder Leistung vorgesehen.
* Netzwerkszenarios sind aufgrund der Anforderungen an einen einzelnen Host mit einer NIC beschränkt.  

## <a name="next-steps"></a>Nächste Schritte
[Wichtige Features und Konzepte](azure-stack-key-features.md)

[Azure Stack: Eine Erweiterung von Azure (pdf)](https://azure.microsoft.com/en-us/resources/azure-stack-an-extension-of-azure/)


