---
title: Azure-Anwendungsangebot | Microsoft-Dokumentation
description: Übersicht über den Prozess zum Veröffentlichen eines Azure-Anwendungsangebots im Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: pbutlerm
ms.openlocfilehash: 63b7ee4e0d9cb9d0d1f26119fe73573b124d04e8
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53195947"
---
# <a name="azure-application-offer"></a>Azure-Anwendungsangebot

In diesem Abschnitt wird erläutert, wie Sie ein neues Azure-Anwendungsangebot im <a href="https://azuremarketplace.microsoft.com">Microsoft Azure Marketplace</a> veröffentlichen.
|    |    |
|-----------------------------------------------------------------|------------------------------------------|
| Jede Azure-Anwendung enthält eine Azure Resource Manager-Vorlage, die alle technischen Ressourcen definiert, die von der Anwendung verwendet werden. Dazu gehören in der Regel mindestens ein virtueller Computer sowie andere unterstützende Azure- oder webbasierte Dienste. | ![Symbol für Azure-Apps](./media/azureapp-icon1.png)  |

## <a name="benefits"></a>Vorteile

Das Listen Ihrer Anwendungen in einem Microsoft Marketplace bietet u.a. folgende Vorteile:

* Erreichen von 100 Millionen Azure Active Directory-Benutzer über Office 365 und Dynamics 365.

* Erweitern Ihres Vertriebsteams: Sie erreichen Geschäftsbenutzer auf der ganzen Welt und profitieren von einem Vertriebskanal, der Endbenutzer anspricht, beim Generieren von Leads hilft und die Kommunikation mit neuen Kunden in den unterschiedlichsten Branchen ermöglicht.

* Gewinnen von umsetzbaren Erkenntnissen: Wir informieren Sie darüber, wie Ihre App in AppSource abschneidet, was gut funktioniert und wie Sie Vertriebsverfahren weiter verbessern können.

## <a name="types-of-azure-applications"></a>Typen von Azure-Anwendungen

Es gibt zwei Typen von Azure-Anwendungen: eine verwaltete Anwendung und eine Lösungsvorlage. Obwohl beide ähnlich sind, gibt es jedoch einige wichtige Unterschiede.

### <a name="solution-template"></a>Lösungsvorlage

Lösungsvorlagen zählen zu den wichtigsten Optionen für die Veröffentlichung von Lösungen im Marketplace. Dieser Angebotstyp wird verwendet, wenn für eine Lösung neben einem einzelnen virtuellen Computer noch eine zusätzliche Bereitstellungs- und Konfigurationsautomatisierung erforderlich ist. Sie können das Bereitstellen mehrere virtueller Computer mithilfe einer Lösungsvorlage automatisieren. Dies schließt die Bereitstellung von Netzwerk- und Speicherressourcen ein, um komplexe IaaS-Lösungen zu implementieren. Eine Übersicht über Lösungsvorlagenanforderungen und das Abrechnungsmodell finden Sie unter [Azure-Anwendungen: Lösungsvorlagen](https://docs.microsoft.com/azure/marketplace/marketplace-solution-templates).

### <a name="managed-application"></a>Verwaltete Anwendung

Eine verwaltete Anwendung ähnelt bis auf einen wichtigen Unterschied einer Marketplace-Lösungsvorlage. Bei einer verwalteten Anwendung werden die Ressourcen in einer Ressourcengruppe bereitgestellt, die vom Herausgeber der App verwaltet wird. Die Ressourcengruppe befindet sich zwar im Abonnement des Verbrauchers, eine Identität im Mandanten des Herausgebers hat jedoch Zugriff auf die Ressourcengruppe. Als Herausgeber geben Sie die Kosten für den kontinuierlichen Support der Lösung an. Verwenden Sie verwaltete Azure-Anwendungen, um mühelos vollständig verwaltete, schlüsselfertige Anwendungen für Ihre Kunden zu erstellen und bereitzustellen.

Sie können verwaltete Anwendungen nicht nur im Marketplace, sondern auch in einem Dienstkatalog anbieten. Der Dienstkatalog ist ein interner Katalog mit freigegebenen Lösungen für Benutzer in einer Organisation. Sie verwenden den Katalog, um Organisationsstandards zu erfüllen, während Lösungen für Gruppen in einer Organisation angeboten werden. Mitarbeiter können über den Katalog komfortabel Anwendungen finden, die die IT-Abteilung empfohlen und freigegeben hat.

Weitere Informationen über die Vorteile und Typen von verwalteten Anwendungen finden Sie unter [Übersicht über verwaltete Azure-Anwendungen](https://docs.microsoft.com/azure/managed-applications/overview).

## <a name="publishing-overview"></a>Übersicht über die Veröffentlichung

Das folgende Video, [Building Solution Templates, and Managed Applications for the Azure Marketplace](https://channel9.msdn.com/Events/Build/2018/BRK3603) (Erstellen von Lösungsvorlagen und verwalteten Anwendungen für den Azure Marketplace), erläutert, wie Sie eine Azure Resource Manager-Vorlage erstellen, um eine Azure-Anwendungslösung zu definieren, und das Anwendungsangebot danach im Azure Marketplace veröffentlichen.

>[!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3603/player]

## <a name="publishing-process-workflow"></a>Workflow zum Veröffentlichungsvorgang

In der folgenden Darstellung ist der grundsätzliche Prozess zum Veröffentlichen eines Azure-Anwendungsangebots aufgeführt.

![Workflow für das Veröffentlichen eines Angebots](./media/new-offer-process.png)

## <a name="offer-components"></a>Angebotskomponenten

Dieser Abschnitt enthält eine Übersicht über Elemente für die Veröffentlichung eines verwalteten Anwendungsangebots und ist als Herausgeberleitfaden für den Azure Marketplace konzipiert. Eine Veröffentlichung ist in folgende Hauptbereiche unterteilt: 

* [Voraussetzungen](./cpp-prerequisites.md): Listet die technischen und geschäftlichen Anforderungen auf, die zum Erstellen oder Veröffentlichen eines verwalteten Anwendungsangebots erfüllt sein müssen. 
* [Erstellen des Angebots](./cpp-create-offer.md): Listet die Schritte auf, die zum Erstellen eines Eintrags für ein verwaltetes Anwendungsangebot über das Cloud-Partnerportal ausgeführt werden müssen. 
* [Veröffentlichen des Angebots](./cpp-publish-offer.md): Beschreibt, wie das Angebot zur Veröffentlichung im Azure Marketplace übermittelt werden muss.

## <a name="steps-in-the-publishing-process"></a>Schritte im Veröffentlichungsprozess

Die Veröffentlichung eines Azure-Anwendungsangebots umfasst ganz allgemein folgende Schritte:

1. Erstellen des Angebots – Geben Sie detaillierte Angebotsinformationen an. Hierzu zählen unter anderem die Angebotsbeschreibung, Marketingmaterial, Supportinformationen und Ressourcenspezifikationen.

2. Erstellen der geschäftlichen und technischen Ressourcen: In diesem Schritt werden die geschäftlichen Ressourcen (rechtlich relevante Dokumente und Marketingmaterial) und die technischen Ressourcen für die zugeordnete Lösung erstellt.

3. Erstellen der SKU – Erstellen Sie die dem Angebot zugeordneten SKUs. Für jedes Image, das Sie veröffentlichen möchten, ist eine eindeutige SKU erforderlich.

4. Zertifizieren und Veröffentlichen des Angebots – Nach Fertigstellung des Angebots und der technischen Ressourcen kann das Angebot übermittelt werden. Durch diese Übermittlung wird der Veröffentlichungsprozess gestartet. Im Rahmen dieses Prozesses wird die Lösung getestet, überprüft, zertifiziert und anschließend im Azure Marketplace live geschaltet.

## <a name="next-steps"></a>Nächste Schritte

Bevor Sie diese Schritte ausführen, müssen die [technischen und geschäftlichen Anforderungen](./cpp-prerequisites.md) für die Veröffentlichung einer verwalteten Anwendung im Microsoft Azure Marketplace erfüllt sein.