---
title: Voraussetzungen für ein Azure-Anwendungsangebot | Microsoft-Dokumentation
description: Die Voraussetzungen für das Veröffentlichen eines Azure-Anwendungsangebots im Azure Marketplace.
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
ms.date: 12/06/2018
ms.author: pbutlerm
ms.openlocfilehash: 1fe847bd0cdceec7eccab8218ccf787d8f4366ba
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53195820"
---
# <a name="azure-application-prerequisites"></a>Voraussetzungen für eine Azure-Anwendung

In diesem Artikel sind die technischen und geschäftlichen Voraussetzungen beschrieben, die erfüllt sein müssen, damit ein verwaltetes Anwendungsangebot im Azure Marketplace veröffentlicht werden kann.

## <a name="technical-requirements"></a>Technische Anforderungen

Zu den technischen Anforderungen gehören die folgenden Elemente:

*   Azure Resource Manager-Vorlagen: Weitere Informationen finden Sie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates). In diesem Artikel wird die Struktur einer Azure Resource Manager-Vorlage beschrieben. Er zeigt die verschiedenen Abschnitte einer Vorlage und die Eigenschaften, die in diesen Abschnitten verfügbar sind. Die Vorlage besteht aus JSON-Code und Ausdrücken, mit denen Sie Werte für Ihre Bereitstellung erstellen können. 
* Azure-Schnellstartvorlagen.<br> Weitere Informationen finden Sie unter

  * [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/). Steigern Sie Ihre Produktivität, indem Sie Azure-Ressourcen über den Azure Resource Manager und mithilfe von Vorlagen bereitstellen, die von der Community entwickelt wurden. Mit dem Azure-Ressourcen-Manager können Sie Ihre Anwendungen mit einer deklarativen Vorlage bereitstellen. In einer Vorlage können Sie mehrere Dienste zusammen mit ihren Abhängigkeiten bereitstellen. Sie verwenden die gleiche Vorlage, um Ihre Anwendung während jeder Phase des Anwendungslebenszyklus wiederholt bereitzustellen.
  * [GitHub: Azure Resource Manager Quickstart Templates](https://github.com/azure/azure-quickstart-templates). Dieses Repository enthält alle derzeit verfügbaren Azure Resource Manager-Vorlagen, die von der Community bereitgestellt werden. Ein durchsuchbarer Vorlagenindex wird unter https://azure.microsoft.com/en-us/documentation/templates/ verwaltet.
* Erstellen von Benutzeroberflächendefinitionen<br>
Weitere Informationen finden Sie unter [Erstellen einer Benutzeroberfläche im Azure-Portal für die verwaltete Anwendung](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview). In diesem Artikel werden die grundlegenden Konzepte der Datei „createUiDefinition.json“ vorgestellt. Das Azure-Portal verwendet diese Datei zum Generieren der Benutzeroberfläche zum Erstellen einer verwalteten Anwendung.

## <a name="business-requirements"></a>Geschäftliche Anforderungen

Die geschäftlichen Anforderungen umfassen die folgenden verfahrenstechnischen, vertraglichen und rechtlichen Verpflichtungen:

* Sie müssen ein registrierter Cloud Marketplace-Herausgeber sein. Sollten Sie sich nicht registriert haben, führen Sie die Schritte des Artikels „Weg zum Cloud Marketplace-Herausgeber“ aus.

>[!NOTE]
>Sie sollten dasselbe Microsoft Developer Center-Registrierungskonto für die Anmeldung beim Cloud-Partnerportal verwenden. Sie sollten für Ihre Azure Marketplace-Angebote nur ein Microsoft-Konto verwenden. Dieses Konto sollte nicht speziell für einzelne Dienste oder Angebote vorgesehen sein.

* Der Standort Ihres Unternehmens (oder der Niederlassung) muss sich in einem vom Azure Marketplace unterstützten Verkäuferland befinden. Eine aktuelle Länderliste finden Sie in den [Teilnahmerichtlinien für Microsoft Azure Marketplace](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).
* Ihr Produkt muss auf eine Weise lizenziert sein, die mit den vom Azure Marketplace unterstützten Abrechnungsmodellen kompatibel ist. Weitere Informationen finden Sie unter [Kommerzielle Transaktionen ‎in Azure Marketplace: Möglichkeiten und Überlegungen](https://docs.microsoft.com/azure/marketplace/marketplace-commercial-transaction-capabilities-and-considerations).
* Sie sind dafür zuständig, technischen Support für Kunden in einer wirtschaftlich vertretbaren Weise zur Verfügung zu stellen. Der Support kann kostenlos, kostenpflichtig oder communitybasiert sein.
* Sie sind für die Lizenzierung Ihrer Software und sämtlicher Abhängigkeiten von Drittanbietersoftware verantwortlich.
* Sie müssen Inhalte bereitstellen, die die notwendigen Kriterien erfüllen, damit Ihr Angebot im Azure Marketplace und im Azure-Portal aufgelistet wird.
* Sie müssen den Bedingungen der Teilnahmerichtlinien für Microsoft Azure Marketplace und der Herausgebervereinbarung zustimmen.
* Sie müssen sich an die Nutzungsbedingungen für die Microsoft Azure-Website, an die Datenschutzerklärung von Microsoft und an die Microsoft Azure Certified-Programmvereinbarung halten.

## <a name="publishing-requirements"></a>Veröffentlichungsbedingungen

Um ein neues Azure-Anwendungsangebot zu veröffentlichen, müssen die folgenden Voraussetzungen erfüllt sein:

* Ihre Metadaten müssen einsatzbereit sein. In der folgenden Liste (nicht vollständig) ist ein Beispiel für diese Metadaten gezeigt:
  * Titel
  * Beschreibung (im HTML-Format)
  * Ein Logobild (im PNG-Format) und in den folgenden festen Bildgrößen: 40 x 40 Pixel, 90 x 90 Pixel, 115 x 115 Pixel und 255 x 115 Pixel.
* Nutzungsbedingungen und Datenschutzrichtlinie
* Dokumentation
* Supportkontakte

## <a name="next-steps"></a>Nächste Schritte

[Erstellen eines Azure-Anwendungsangebots](./cpp-create-offer.md) 
