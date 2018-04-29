---
title: Was ist Azure Stack? | Microsoft-Dokumentation
description: Azure Stack ermöglicht die Ausführung von Azure-Diensten in Ihrem Datencenter.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/22/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.custom: mvc
ms.openlocfilehash: d54f392eddfcca99e7fe0b037b9efd0a4e90433c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="what-is-azure-stack"></a>Was ist Azure Stack?

Microsoft Azure Stack ist eine Hybrid Cloud-Plattform, mit der Sie Azure-Dienste über das Rechenzentrum Ihrer Organisation bereitstellen können.  Azure Stack soll in gängigen Szenarien (etwa in Edge-Umgebungen und getrennten Umgebungen) neue Möglichkeiten für Ihre modernen Anwendungen eröffnen und zur Erfüllung spezifischer Sicherheits- und Konformitätsanforderungen beitragen.  Azure Stack wird in zwei auf Ihre Anforderungen abgestimmten Bereitstellungsoptionen angeboten.

## <a name="azure-stack-integrated-systems"></a>Integrierte Azure Stack-Systeme
Integrierte Azure Stack-Systeme werden im Rahmen einer Partnerschaft zwischen Microsoft und [Hardwarepartnern](https://azure.microsoft.com/overview/azure-stack/integrated-systems/) angeboten. Dadurch entsteht eine ausgewogene Lösung mit cloudbasierten Innovationen und komfortabler Verwaltung.  Bei Azure Stack handelt es sich um ein integriertes System mit Hardware und Software. Somit bietet die Plattform genau das richtige Maß an Flexibilität und Kontrolle bei gleichzeitiger Bereitstellung von cloudbasierten Innovationen.  Integrierte Azure Stack-Systeme haben eine Größe von vier bis zwölf Knoten, und der Support wird vom Hardwarepartner und von Microsoft gemeinsam bereitgestellt.  Mit integrierten Azure Stack-Systemen ermöglichen Sie neue Szenarien für Ihre Produktionsworkloads.    

## <a name="azure-stack-development-kit"></a>Azure Stack Development Kit
[Azure Stack Development Kit (ASDK)](.\asdk\asdk-what-is.md) von Microsoft ist eine Einzelknotenbereitstellung von Azure Stack, mit der Sie Azure Stack evaluieren und kennenlernen können.  Sie können das ASDK auch für Entwicklungsarbeiten mit APIs und Tools verwenden, die mit Azure konsistent sind. Das ASDK ist nicht für die Verwendung in einer Produktionsumgebung vorgesehen.

Für das ASDK gelten folgende Einschränkungen:
* Das ASDK wird einem einzelnen Identitätsanbieter zugeordnet. Dabei handelt es sich entweder um Azure Active Directory (Azure AD) oder um Active Directory-Verbunddienste (Active Directory Federation Services, AD FS). Sie können mehrere Benutzer in diesem Verzeichnis erstellen und jedem Benutzer Abonnements zuweisen.
* Da alle Komponenten auf einem einzigen Computer bereitgestellt werden, stehen für Mandantenressourcen nur begrenzte physische Ressourcen zur Verfügung. Diese Konfiguration ist nicht für die Beurteilung von Skalierung oder Leistung vorgesehen.
* Netzwerkszenarios sind aufgrund der Anforderungen an einen einzelnen Host mit einer NIC beschränkt.  

## <a name="next-steps"></a>Nächste Schritte
[Wichtige Features und Konzepte](azure-stack-key-features.md)

[Azure Stack: Eine Erweiterung von Azure (PDF)](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/)

