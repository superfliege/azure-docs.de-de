---
title: Verwenden Sie InSpec für die Automatisierung der Konformität der Azure-Infrastruktur
description: Erfahren Sie, wie Sie mit InSpec Probleme in Ihren Azure-Bereitstellungen erkennen.
keywords: Azure, Chef, Devops, virtuelle Computer, Übersicht, Automatisieren, InSpec
ms.service: virtual-machines-linux
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: 9256a4daf6564761553b495e559805a46e4eae32
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54050726"
---
# <a name="use-inspec-for-compliance-automation-of-your-azure-infrastructure"></a>Verwenden Sie InSpec für die Automatisierung der Konformität der Azure-Infrastruktur
[InSpec](https://www.chef.io/inspec/) ist ein kostenloses Open-Source-Framework zum Testen und Überwachen Ihrer Anwendungen und Infrastruktur. InSpec vergleicht den tatsächlichen Status Ihres Systems mit dem gewünschten Status, den Sie in einfach zu lesendem und zu schreibendem InSpec-Code ausdrücken. InSpec erkennt Verstöße und zeigt Ergebnisse in Form eines Berichts an, jedoch haben Sie die Kontrolle über die Wiederherstellung. Sie können mit InSpec den Status Ihrer in Azure ausgeführten virtuellen Computer überprüfen. Sie können mit InSpec auch den Status von Ressourcen und Ressourcengruppen innerhalb eines Abonnements überprüfen.

Dieser Artikel beschreibt die Vorteile der Verwendung von InSpec, um Sicherheit und Konformität in Azure zu vereinfachen.

## <a name="make-compliance-easy-to-understand-and-assess"></a>Erleichtern Sie Verständnis und Bewertung von Konformität
Mit InSpec transformieren Sie Ihre Anforderungen in versionierten, ausführbaren, von Menschen lesbaren Code. Dadurch können Sie die Tests in zusammensetzbaren Profilen organisieren, wo Sie Ausnahmen nach Bedarf definieren anpassen.

## <a name="detect-fleet-wide-issues-and-prioritize-their-remediation"></a>Erkennen flottenweiter Probleme und Priorisieren der Wiederherstellung
Der InSpec-Erkennungsmodus ohne Agent ermöglicht Ihnen – nach Bedarf – schnellen Zugriff auf Ihrer Ebene. Integrierte Metadaten für die Bewertung von Auswirkung/Schweregrad erleichtern Ihnen, zu bestimmen, auf welche Bereiche sich die Wiederherstellung konzentrieren soll.

## <a name="inspect-machines-data-and-new-saas-apis"></a>Untersuchen von Computern, Daten und neuen SaaS-APIs
Die InSpec-Funktionen zur Cloud-API-Konformität ermöglichen Ihnen, undifferenzierte und differenzierte Assertionen über Ihre Cloudkonformität durchzuführen und fortlaufend darüber zu berichten.

## <a name="satisfy-audits"></a>Erfüllen der Anforderungen von Überwachungen+
Mit InSpec können Sie jederzeit Überwachungsfragen beantworten – nicht nur in vordefinierten Intervallen, z.B. vierteljährlich oder jährlich. Mit InSpec können Sie beim Eintritt in einen Überwachungszyklus Ihren genauen Konformitätsstand kennen, anstatt durch die Ergebnisse eines Auditors überrascht zu werden.

## <a name="reduce-ambiguity-and-miscommunication-regarding-rules"></a>Reduzieren von Mehrdeutigkeit und Fehlkommunikation bezüglich der Regeln
Dokumente lassen bei Konfigurationen und Prozessen einen Interpretationsspielraum zu. Ausführbarer Code entfernt Unterhaltungen darüber, was festgestellt werden sollte, zugunsten konkreter Tests mit klarer Absicht.

## <a name="keep-up-with-rapidly-changing-threat-and-compliance-landscapes"></a>Schritthalten mit sich schnell ändernden Bedrohungs- und Konformitätsgegebenheiten
Mit InSpec können Sie Erkennungscode am selben Tag schreiben und veröffentlichen sowie neue Regeln in schneller Reaktion auf neue Vorschriften schreiben. Dies bedeutet, dass Änderungen von Bedrohungen oder Vorschriften nicht mehr Notfällen gleichen.

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen eines virtuellen Windows-Computers in Azure mithilfe von Chef](/azure/virtual-machines/windows/chef-automation)