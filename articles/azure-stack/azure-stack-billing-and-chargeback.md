---
title: Kundenabrechnung und verbrauchsbasierte Kostenzuteilung in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Informationen zum Ressourcenverbrauch aus Azure Stack abrufen.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: sethm
ms.reviewer: alfredop
ms.openlocfilehash: fcfd5d4e9e2f30d769818df29cf8a76bd9113d4f
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2018
ms.locfileid: "45632409"
---
# <a name="usage-and-billing-in-azure-stack"></a>Verbrauch und Abrechnung in Azure Stack

Dieser Artikel beschreibt, wie Azure Stack-Benutzern die Ressourcennutzung in Rechnung gestellt wird. Sie erfahren, wie der Zugriff auf Abrechnungsinformationen für Analysen und verbrauchsbasierte Kostenzuteilung erfolgt.

Azure Stack erfasst und gruppiert Nutzungsdaten für verwendete Ressourcen. Azure Stack leitet dann diese Daten an Azure Commerce weiter. Azure Commerce stellt Ihnen die Azure Stack-Nutzung genauso in Rechnung wie die Azure-Nutzung.

Sie können auch Nutzungsdaten abrufen und mithilfe eines Abrechnungsadapters in Ihr eigenes System für die Abrechnung und verbrauchsbasierte Kostenzuteilung oder in ein Business Intelligence-Tool wie z.B. Microsoft Power BI exportieren.


## <a name="usage-pipeline"></a>Nutzungspipeline

Jeder Ressourcenanbieter in Azure Stack gibt Nutzungsdaten gemäß Ressourcennutzung aus. Der Nutzungsdienst aggregiert Nutzungsdaten in regelmäßigen Abständen (stündlich und täglich) und speichert sie in der Nutzungsdatenbank. Azure Stack-Operatoren und -Benutzer können über die Azure Stack-Ressourcennutzungs-APIs auf die gespeicherten Nutzungsdaten zugreifen. 

Wenn Sie [Ihre Azure Stack-Instanz bei Azure registriert haben](azure-stack-register.md), ist Azure Stack zum Senden der Nutzungsdaten an Azure Commerce konfiguriert. Nachdem die Daten in Azure hochgeladen wurden, können Sie über das Abrechnungsportal oder mithilfe der Ressourcennutzungs-APIs von Azure darauf zugreifen. Weitere Informationen dazu, welche Nutzungsdaten an Azure gemeldet werden, finden Sie im Artikel [Nutzungsdatenberichte](azure-stack-usage-reporting.md).  

Die folgende Abbildung zeigt die wichtigsten Komponenten in der Nutzungspipeline: 

![Nutzungspipeline](media\azure-stack-billing-and-chargeback\usagepipeline.png)

## <a name="what-usage-information-can-i-find-and-how"></a>Welche Nutzungsinformationen kann ich suchen, und wo finde ich sie?

Azure Stack-Ressourcenanbieter wie Compute-, Speicher- und Netzwerkressourcen generieren stündlich Nutzungsdaten für jedes Abonnement. Die Nutzungsdaten enthalten Informationen zur verwendeten Ressource, z.B. den Ressourcennamen, das verwendete Abonnement, die genutzte Menge usw. Weitere Informationen zu den Verbrauchseinheiten finden Sie im Artikel [Häufig gestellte Fragen zu Nutzungs-APIs](azure-stack-usage-related-faq.md).

Die gesammelten Nutzungsdaten werden [an Azure gemeldet](azure-stack-usage-reporting.md), um eine Rechnung zu generieren, die im Azure-Abrechnungsportal angezeigt werden kann. 

> [!NOTE]  
> Das Melden von Nutzungsdaten ist für Azure Stack Development Kit-Benutzer und Benutzer von integrierten Azure Stack-Systemen, die unter dem Kapazitätsmodell lizenziert sind, nicht erforderlich. Weitere Informationen zur Lizenzierung in Azure Stack finden Sie im Datenblatt zu [Paketerstellung und Preisen](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).

Das Azure-Abrechnungsportal zeigt Nutzungsdaten für kostenpflichtige Ressourcen. Zusätzlich zu den kostenpflichtigen Ressourcen erfasst Azure Stack Nutzungsdaten für eine umfassendere Palette an Ressourcen. Auf diese Daten können Sie in Ihrer Azure Stack-Umgebung über REST-APIs oder PowerShell zugreifen. Azure Stack-Operatoren können die Nutzungsdaten für alle Benutzerabonnements abrufen. Einzelne Benutzer können nur ihre Nutzungsdetails abrufen. 

## <a name="usage-reporting-for-multitenant-cloud-service-providers"></a>Nutzungsberichte für mehrinstanzenfähige Cloud-Dienstanbieter

Ein mehrinstanzenfähiger Cloud-Dienstanbieter (CSP) mit vielen Kunden, die Azure Stack nutzen, sollte einen separaten Nutzungsbericht für jeden Kunden erstellen, damit der Anbieter die Nutzung für verschiedene Azure-Abonnements in Rechnung stellen kann. 

Die Identitäten der einzelnen Kunden werden jeweils durch einen anderen Azure Active Directory (Azure AD)-Mandanten dargestellt. Azure Stack unterstützt das Zuweisen eines CSP-Abonnements an jeden Azure AD-Mandanten. Sie können der Azure Stack-Basisregistrierung Mandanten und deren Abonnements hinzufügen. Die Basisregistrierung erfolgt für alle Azure Stack-Instanzen. Wenn ein Abonnement für einen Mandanten nicht registriert ist, kann der Benutzer Azure Stack trotzdem verwenden, und seine Nutzungsdaten werden an das für die Basisregistrierung verwendete Abonnement gesendet. 


## <a name="next-steps"></a>Nächste Schritte

[Registrieren bei Azure Stack](azure-stack-registration.md)

[Melden von Azure Stack-Nutzungsdaten an Azure](azure-stack-usage-reporting.md)

[Ressourcennutzungs-API für Anbieter](azure-stack-provider-resource-api.md)

[Ressourcennutzungs-API für Mandanten](azure-stack-tenant-resource-usage-api.md)

[Häufig gestellte Fragen zur Nutzung](azure-stack-usage-related-faq.md)