---
title: Häufig gestellte Fragen zu Azure App Configuration | Microsoft-Dokumentation
description: Häufig gestellte Fragen zu Azure App Configuration
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 3181609bf34a04de4e31b73429f9bc5fa3fe3408
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2019
ms.locfileid: "65411852"
---
# <a name="azure-app-configuration-faq"></a>Häufig gestellte Fragen zu Azure App Configuration

Dieser Artikel behandelt häufig gestellte Fragen zu Azure App Configuration.

## <a name="how-is-app-configuration-different-from-key-vault"></a>Wie unterscheidet sich App Configuration von Key Vault?

App Configuration ist auf einen bestimmten Satz von Anwendungsfällen ausgelegt: Es hilft Entwicklern dabei, Anwendungseinstellungen zu verwalten und die Verfügbarkeit von Funktionen zu steuern. Es zielt darauf ab, viele der Aufgaben, aus denen die Arbeit mit komplexen Konfigurationsdaten besteht, zu vereinfachen. Es unterstützt hierarchische Namespaces, Bezeichnungen, umfangreiche Abfragen, Batchabruf sowie spezialisierte Verwaltungsvorgänge und Benutzeroberflächen. App Configuration stellt eine Ergänzung zu Key Vault dar, und die beiden sollten in den meisten Anwendungsbereitstellungen nebeneinander verwendet werden.

## <a name="should-i-store-secrets-in-app-configuration"></a>Sollte ich Geheimnisse in App Configuration speichern?

Zwar bietet App Configuration verstärkte Sicherheit, doch Key Vault ist weiterhin der beste Ort zum Speichern von Anwendungsgeheimnissen. Er bietet Verschlüsselung auf Hardwareebene, granulare Zugriffsrichtlinien und Verwaltungsvorgänge, z. B. Zertifikatrotation.

## <a name="does-app-configuration-encrypt-my-data"></a>Verschlüsselt App Configuration meine Daten?

Ja. App Configuration verschlüsselt alle Schlüsselwerte, die es enthält, sowie die Netzwerkkommunikation. Schlüsselnamen werden als Indizes zum Abrufen von Konfigurationsdaten verwendet und werden nicht verschlüsselt.

## <a name="does-app-configuration-support-azure-virtual-network-vnet"></a>Unterstützt App Configuration Azure Virtual Network (VNET)?

Bisher nicht. VNET-Unterstützung ist für die allgemeine Verfügbarkeit geplant.

## <a name="how-should-i-store-configurations-for-multiple-environments-test-staging-production-and-so-on"></a>Wie sollte ich Konfigurationen für mehrere Umgebungen (Test, Staging, Produktion usw.) speichern?

Zurzeit können Sie auf Speicherebene (also pro Speicher) kontrollieren, wer Zugriff auf App Configuration hat. Sie sollten für jede Umgebung, die andere Berechtigungen erfordert, einen gesonderten Speicher verwenden. Dieser Ansatz bietet Ihnen die beste Sicherheitsisolierung.

## <a name="what-are-the-recommended-ways-to-use-app-configuration"></a>Welche Methoden werden für die Verwendung von App Configuration empfohlen?

Siehe unter [Bewährte Methoden](./howto-best-practices.md).

## <a name="how-much-does-app-configuration-cost"></a>Wie viel kostet App Configuration?

Der Dienst ist während der öffentlichen Vorschauphase kostenlos.

## <a name="how-can-i-report-an-issue-or-give-a-suggestion"></a>Wie kann ich ein Problem melden oder einen Vorschlag machen?

Sie erreichen uns direkt auf [GitHub](https://github.com/Azure/AppConfiguration/issues).

## <a name="next-steps"></a>Nächste Schritte

* [Informationen zu Azure App Configuration](./overview.md)
