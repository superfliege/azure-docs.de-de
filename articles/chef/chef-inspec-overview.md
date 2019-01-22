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
ms.openlocfilehash: a5de2ca04a193f97a2a65a043f744abb8e0ea758
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54359220"
---
# <a name="use-inspec-for-compliance-automation-of-your-azure-infrastructure"></a>Verwenden Sie InSpec für die Automatisierung der Konformität der Azure-Infrastruktur
[InSpec](https://www.chef.io/inspec/) ist die Open Source-Sprache von Chef zum Beschreiben von Sicherheits- und Complianceregeln, die von Softwareentwicklern, Operatoren und Sicherheitstechnikern gemeinsam genutzt werden können. InSpec vergleicht den tatsächlichen Status Ihrer Infrastruktur mit dem gewünschten Status, den Sie in einfach zu lesendem und zu schreibendem InSpec-Code ausdrücken. InSpec erkennt Verstöße und zeigt Ergebnisse in Form eines Berichts an, jedoch haben Sie die Kontrolle über die Wiederherstellung.

Sie können InSpec verwenden, um den Zustand von Ressourcen und-Ressourcengruppen innerhalb eines Abonnements zu überprüfen, einschließlich virtueller Computer, Netzwerkkonfigurationen, Azure Active Directory-Einstellungen und mehr.

Dieser Artikel beschreibt die Vorteile der Verwendung von InSpec, um Sicherheit und Konformität in Azure zu vereinfachen.

## <a name="make-compliance-easy-to-understand-and-assess"></a>Erleichtern Sie Verständnis und Bewertung von Konformität
In Tabellen oder Word-Dokumenten niedergelegte Compliancedokumentationen lassen bei Anforderungen Spielraum für Interpretationen. Mit InSpec transformieren Sie Ihre Anforderungen in versionierten, ausführbaren, von Menschen lesbaren Code. Code ersetzt Unterhaltungen darüber, was bewertet werden sollte, zugunsten konkreter Tests mit klaren Absichten.

## <a name="detect-fleet-wide-issues-and-prioritize-their-remediation"></a>Erkennen flottenweiter Probleme und Priorisieren der Wiederherstellung
Der InSpec-Erkennungsmodus ohne Agent ermöglicht Ihnen – je nach Umfang – die schnelle Bewertung Ihres Gefährdungsgrads. Integrierte Metadaten für die Bewertung von Auswirkung/Schweregrad erleichtern Ihnen, zu bestimmen, auf welche Bereiche sich die Wiederherstellung konzentrieren soll. Sie können auch schnell Regeln als Reaktion auf neue Sicherheitsrisiken oder Bestimmungen schreiben und diese sofort einführen.

## <a name="satisfy-audits"></a>Erfüllen der Anforderungen von Überwachungen+
Mit InSpec können Sie jederzeit Überwachungsfragen beantworten – nicht nur in vordefinierten Intervallen, z.B. vierteljährlich oder jährlich. Durch die kontinuierliche Ausführung von InSpec-Tests treten Sie in einen Überwachungszyklus ein, wobei Sie Ihren genauen Compliancestand -und verlauf kennen, anstatt durch die Ergebnisse eines Auditors überrascht zu werden.

## <a name="next-steps"></a>Nächste Schritte

* Testen von InSpec in der Azure Cloud Shell[![Starten der Cloud Shell](https://shell.azure.com/images/launchcloudshell.png "Starten der Cloud Shell")](https://shell.azure.com)
