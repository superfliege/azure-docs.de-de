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
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.custom: mvc
ms.openlocfilehash: b2b81938eafe104369e52e72f9958e2adf2cca6f
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49345219"
---
# <a name="what-is-azure-stack"></a>Was ist Azure Stack?

Microsoft Azure Stack ist eine Hybrid Cloud-Plattform, die das Bereitstellen von Azure-Diensten in Ihrem Datencenter ermöglicht. Diese Plattform bietet flexible Unterstützung für sich verändernde Geschäftsanforderungen. Azure Stack kann für Ihre modernen Anwendungen wie etwa Edge-Umgebungen und getrennte Umgebungen neue Möglichkeiten eröffnen und zur Erfüllung spezifischer Sicherheits- und Konformitätsanforderungen beitragen.

Azure Stack wird in zwei auf Ihre Anforderungen abgestimmten Bereitstellungsoptionen angeboten.

## <a name="azure-stack-integrated-systems"></a>Integrierte Azure Stack-Systeme
Integrierte Azure Stack-Systeme werden im Rahmen einer Partnerschaft zwischen Microsoft und [Hardwarepartnern](https://azure.microsoft.com/overview/azure-stack/integrated-systems/) angeboten. Dadurch entsteht eine Lösung mit cloudbasierten Innovationen und komfortabler Computeverwaltung. Als integriertes Hardware- und Softwaresystem bietet Ihnen Azure Stack nicht nur genau die Flexibilität und Kontrolle, die Sie benötigen, sondern auch die Möglichkeit zur Nutzung innovativer Cloudfeatures. Integrierte Azure Stack-Systeme haben eine Größe von vier bis zwölf Knoten, und der Support wird vom Hardwarepartner und von Microsoft gemeinsam bereitgestellt.  Mit integrierten Azure Stack-Systemen ermöglichen Sie neue Szenarien und stellen neue Lösungen für Ihre Produktionsworkloads bereit.

## <a name="azure-stack-development-kit"></a>Azure Stack Development Kit

Das [Azure Stack Development Kit (ASDK)](.\asdk\asdk-what-is.md) von Microsoft ist eine Einzelknotenbereitstellung von Azure Stack, mit der Sie Azure Stack evaluieren und kennenlernen können.  Sie können das ASDK auch für Entwicklungsarbeiten zum Erstellen von Apps mit APIs und Tools verwenden, die mit Azure konsistent sind.

>[!Note]
>Das ASDK ist nicht für die Verwendung in einer Produktionsumgebung vorgesehen.

Für das ASDK gelten folgende Einschränkungen:

* Das ASDK wird einem einzelnen Identitätsanbieter zugeordnet. Dabei handelt es sich entweder um Azure Active Directory (Azure AD) oder um Active Directory-Verbunddienste (Active Directory Federation Services, AD FS). Sie können mehrere Benutzer in diesem Verzeichnis erstellen und jedem Benutzer Abonnements zuweisen.
* Da Azure Stack-Komponenten auf einem Hostcomputer bereitgestellt werden, sind für Mandantenressourcen die verfügbaren physischen Ressourcen begrenzt. Diese Konfiguration ist nicht für die Beurteilung von Skalierung oder Leistung vorgesehen.
* Netzwerkszenarien sind aufgrund der Bereitstellungsanforderungen für Einzelhosts und NICs eingeschränkt.

## <a name="next-steps"></a>Nächste Schritte

[Wichtige Features und Konzepte](azure-stack-key-features.md)

[Azure Stack: Eine Erweiterung von Azure (PDF)](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/)
