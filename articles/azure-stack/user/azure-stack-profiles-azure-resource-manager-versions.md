---
title: Von Profilen in Azure Stack unterstützte API-Versionen von Ressourcenanbietern | Microsoft-Dokumentation
description: Hier erfahren Sie mehr zur Azure Resource Manager-Version, die von den Profilen in Azure Stack unterstützt werden.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: db01df21c95ee41197344cec719f1c2ab2dfc2ed
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="resource-provider-api-versions-supported-by-profiles-in-azure-stack"></a>Von Profilen in Azure Stack unterstützte API-Versionen von Ressourcenanbietern

Ein Azure-Ressourcenanbieter stellt Ressourcen zur Verfügung, die Sie über den Azure Resource Manager bereitstellen und verwalten können. Jeder Anbieter bietet Vorgänge für die Arbeit mit Ressourcen. Beispiele für häufig verwendete Ressourcenanbieter sind „Microsoft.Compute“ zum Bereitstellen virtueller Computer, „Microsoft.Storage“ zum Bereitstellen der Speicherkontoressourcen und „Microsoft.Web“ zum Bereitstellen von Ressourcen für Web-Apps. Weitere Informationen finden Sie unter [Ressourcenanbieter und -typen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services).

In der folgenden Tabelle wird für jeden Ressourcenanbieter die unterstützte Version der API-Version für Azure Stack angegeben, wenn Profile verwendet werden.

### <a name="microsoftauthorization"></a>Microsoft.Authorization

Mit der rollenbasierten Zugriffssteuerung werden die Aktionen verwaltet, die Benutzer in Ihrer Organisation für Ressourcen ausführen können. Diese Gruppe von Vorgängen ermöglicht das Definieren von Rollen, Zuweisen von Rollen zu Benutzern oder Gruppen und das Abrufen von Informationen zu Berechtigungen. Weitere Informationen finden Sie unter [Autorisierung](https://docs.microsoft.com/rest/api/authorization/).

| Ressourcentypen | API-Versionen |
|---------------------|--------------------|
| Sperren | 2017-04-01 |
| Vorgänge | 2015-07-01 |
| Berechtigungen | 2015-07-01 |
| Richtlinienzuweisungen | 2016-12-01 (2017-06-01-preview) |
| Richtliniendefinitionen | 2016-12-01 |
| Anbietervorgänge | 2015-07-01-preview |
| Rollenzuweisungen | 2015-07-01 |
| Rollendefinitionen | 2015-07-01 |

### <a name="microsoftcommerce"></a>Microsoft.Commerce

| Ressourcentyp | API-Version |
|----------------------------------|----------------------|
| Delegierte Anbieterabonnements | 2015-06-01 – Vorschauversion |
| Delegierte Nutzungsaggregate | 2015-06-01 – Vorschauversion |
| Geschätzter Ressourcenaufwand | 2015-06-01 – Vorschauversion |
| Vorgänge | 2015-06-01 – Vorschauversion |
| Nutzungsaggregate von Abonnenten | 2015-06-01 – Vorschauversion |
| Nutzungsaggregate | 2015-06-01 – Vorschauversion |

### <a name="microsoftcompute"></a>Microsoft.Compute

Die Azure Compute-APIs ermöglichen Ihnen den programmgesteuerten Zugriff auf virtuelle Computer und die zugehörigen unterstützenden Ressourcen. Weitere Informationen finden Sie unter [Azure Compute](https://docs.microsoft.com/rest/api/compute/).

| Ressourcentyp | API-Version |
|---------------------------------------------------------------|-------------|
| Verfügbarkeitsgruppen | 2016-03-30 |
| Standorte | 2016-03-30 |
| Speicherorte/Vorgänge | 2016-03-30 |
| Speicherorte/Herausgeber | 2016-03-30 |
| Speicherorte/Verwendungen | 2016-03-30 |
| Speicherorte/VM-Größen | 2016-03-30 |
| Vorgänge | 2016-03-30 |
| Virtual Machines | 2016-03-30 |
| Virtuelle Computer/Erweiterungen | 2016-03-30 |
| Skalierungsgruppen für virtuelle Computer | 2016-03-30 |
| VM-Skalierungsgruppen/Erweiterungen | 2016-03-30 |
| VM-Skalierungsgruppen/Netzwerkschnittstellen | 2016-03-30 |
| VM-Skalierungsgruppen/virtuelle Computer | 2016-03-30 |
| VM-Skalierungsgruppen/virtuelle Computer/Netzwerkschnittstellen | 2016-03-30 |

### <a name="microsoftgallery"></a>Microsoft.Gallery

| Ressourcentyp | API-Version |
|------------------|-------------|
| Zusammenstellung | 2015-04-01 |
| Inhalt der Zusammenstellung | 2015-04-01 |
| Auszug aus Zusammenstellung | 2015-04-01 |
| Katalogelemente | 2015-04-01 |
| Vorgänge | 2015-04-01 |
| Portal | 2015-04-01 |
| Suchen, | 2015-04-01 |
| Vorschlagen | 2015-04-01 |

### <a name="microsoftinsights"></a>Microsoft.Insights

| Ressourcentypen | API-Versionen |
|--------------------|--------------------|
| Warnungsregeln | 2016-03-01 |
| Ereigniskategorien | 2017-03-01-preview |
| Ereignistypen | 2017-03-01-preview |
| Metrikdefinitionen | 2016-03-01 |
| Vorgänge | 2015-04-01 |

### <a name="microsoftkeyvault"></a>Microsoft.KeyVault

Hiermit verwalten Sie Ihre Schlüsseltresore sowie die Schlüssel, Geheimnisse und Zertifikate in Ihren Schlüsseltresoren. Weitere Informationen finden Sie unter [Referenz zur REST-API für Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/).

| Ressourcentypen | API-Versionen |
|-------------------------|--------------|
| Vorgänge | 2016-10-01 |
| Tresore | 2016-10-01 |
| Tresore/Zugriffsrichtlinien | 2016-10-01 |
| Tresore/Geheimnisse | 2016-10-01 |

### <a name="microsoftkeyvaultadmin"></a>Microsoft.Keyvault.Admin

Hiermit verwalten Sie Ihre Schlüsseltresore sowie die Schlüssel, Geheimnisse und Zertifikate in Ihren Schlüsseltresoren. Weitere Informationen finden Sie unter [Referenz zur REST-API für Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/).

| Ressourcentypen | API-Versionen |
|------------------|--------------------|
| Standorte | 2017-02-01-preview |
| Speicherorte/Kontingente | 2017-02-01-preview |

### <a name="microsoftnetwork"></a>Microsoft.Network

Das Ergebnis der Vorgangsaufrufe ist die Darstellung einer Liste verfügbarer Netzwerkcloudvorgänge. Weitere Informationen finden Sie unter [REST-API für Vorgänge](https://docs.microsoft.com/rest/api/operation/).

| Ressourcentypen | API-Versionen |
|---------------------------|--------------|
| Verbindungen | 2015-06-15 |
| DNS-Zonen | 2016-04-01 |
| Load Balancer | 2015-06-15 |
| Gateway des lokalen Netzwerks | 2015-06-15 |
| Standorte | 2016-04-01 |
| Speicherort/Vorgangsergebnisse | 2016-04-01 |
| Speicherorte/Vorgänge | 2016-04-01 |
| Speicherorte/Verwendungen | 2016-04-01 |
| Netzwerkschnittstellen | 2015-06-15 |
| Netzwerksicherheitsgruppen | 2015-06-15 |
| Vorgänge | 2015-06-15 |
| Öffentliche IP-Adresse | 2015-06-15 |
| Routingtabellen | 2015-06-15 |
| Gateway des virtuellen Netzwerks | 2015-06-15 |
| Virtuelle Netzwerke | 2015-06-15 |

### <a name="microsoftresources"></a>Microsoft.Resources

Mit Azure Resource Manager können Sie die Infrastruktur für Ihre Azure-Lösungen bereitstellen und verwalten. Sie organisieren zugehörige Ressourcen in Ressourcengruppen, und stellen Ihre Ressourcen mit JSON-Vorlagen bereit. Eine Einführung in die Bereitstellung und Verwaltung von Ressourcen mit dem Resource Manager finden Sie unter [Übersicht über Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

| Ressourcentypen | API-Versionen |
|-----------------------------------------|-------------------|
| Anwendungsregistrierungen | 2015-01-01 |
| Ressourcennamen prüfen | 2015-012016-09-01 |
| Delegierte Anbieter | 2015-01-01 |
| Delegierte Anbieter/Angebote | 2015-01-01 |
| Delegierte Anbieter/Angebote/geschätzter Preis | 2015-01-01 |
| Bereitstellungen | 2016-0209-01 |
| Bereitstellungen/Vorgänge | 2016-0209-01 |
| Metadaten zu Erweiterungen | 2015-01-01 |
| Links | 2015-012016-09-01 |
| Standorte | 2015-01-01 |
| Angebote | 2015-01-01 |
| Vorgänge | 2015-01-01 |
| Anbieter | 2015-012017-08-01 |
| Ressourcengruppen | 2015-012016-09-01 |
| angeben | 2015-012016-09-01 |
| Abonnements | 2015-012016-09-01 |
| Abonnements/Speicherort | 2015-012016-09-01 |
| Abonnements/Vorgangsergebnisse | 2015-012016-09-01 |
| Abonnements/Anbieter | 2015-012017-08-01 |
| Abonnements/Ressourcengruppen | 2015-012016-09-01 |
| Abonnements/Ressourcengruppen/Ressourcen | 2015-012016-09-01 |
| Abonnements/Ressourcen | 2015-012016-09-01 |
| Abonnements/Tagnamen | 2016-0609-01 |
| Abonnements/Tagnamen/Tagwerte | 2016-0609-01 |
| Mandanten | 2015-012017-08-01 |

### <a name="microsoftstorage"></a>Microsoft.Storage 

Mit dem Speicherressourcenanbieter (Storage Resource Provider, SRP) können Sie Ihr Speicherkonto und Ihre Schlüssel programmgesteuert verwalten. Weitere Informationen finden Sie unter [Referenz zur Ressourcenanbietern-REST-API für Azure Storage](https://docs.microsoft.com/rest/api/storagerp/).

| Ressourcentypen | API-Versionen |
|-------------------------|--------------|
| Dient zum Prüfen der Verfügbarkeit des Namens. | 2016-01-01 |
| Standorte | 2016-01-01 |
| Speicherorte/Kontingente | 2016-01-01 |
| Vorgänge | 2016-01-01 |
| Speicherkonten | 2016-01-01 |
| Verwendungen | 2016-01-01 |

## <a name="next-steps"></a>Nächste Schritte

* [Installieren von PowerShell für Azure Stack](azure-stack-powershell-install.md)
* [Konfigurieren der PowerShell-Umgebung des Azure Stack-Benutzers](azure-stack-powershell-configure-user.md)  
