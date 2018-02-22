---
title: "Veröffentlichen von benutzerdefinierten Marketplace-Elementen in Azure Stack (Cloudbetreiber) | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie als Azure Stack-Betreiber ein benutzerdefiniertes Marketplace-Element in Azure Stack veröffentlichen."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 60871cbb-eed2-433c-a76d-d605c7aec06c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: c791708e11b7e9e8bbe046f06233d948d4632c90
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="the-azure-stack-marketplace-overview"></a>Der Azure Stack-Marketplace – Übersicht

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Der Marketplace ist eine Sammlung von Diensten, Anwendungen und Ressourcen, die für die Azure Stack angepasst wurden, wie Netzwerke, virtuelle Computer, Speicher usw. Benutzer nutzen den Marketplace zum Erstellen neuer Ressourcen sowie zum Bereitstellen neuer Anwendungen. In gewisser Weise ist er ein Katalog, in dem Benutzer die gewünschten Elemente suchen und auswählen können. Benutzer müssen ein Angebot abonnieren, das ihnen Zugriff auf das Marketplace-Element gewährt, um dieses zu verwenden.

Als Azure Stack-Betreiber entscheiden Sie, welche Elemente Sie zum Marketplace hinzufügen (veröffentlichen). Sie können z.B. Datenbanken, App Services usw. veröffentlichen. Dadurch sind sie für alle Benutzer sichtbar. Sie können die von Ihnen erstellten benutzerdefinierten Elemente veröffentlichen. Oder Sie veröffentlichen Elemente aus einer immer größer werdenden [Liste von Azure Marketplace-Elementen](azure-stack-marketplace-azure-items.md). Wenn Sie ein Element im Marketplace veröffentlichen, wird es Benutzern innerhalb von fünf Minuten angezeigt.

Klicken Sie zum Öffnen des Marketplace auf **Neu**.

![](media/azure-stack-publish-custom-marketplace-item/image1.png)

## <a name="marketplace-items"></a>Marketplace-Elemente
Azure Stack-Marketplace-Elemente sind Dienste, Anwendungen oder Ressourcen, die die Benutzer herunterladen und verwenden können. Alle Azure Stack-Marketplace-Elemente sind für alle Benutzer sichtbar.

Jedes Marketplace-Element verfügt über:

* Eine Azure-Ressourcen-Manager-Vorlage für die Ressourcenbereitstellung
* Metadaten wie Zeichenfolgen, Symbole und andere Marketingmaterialien
* Formatierungsinformationen zum Anzeigen des Elements im Portal

Jedes im Marketplace veröffentlichte Element verwendet ein Format namens Azure Gallery Package (AZPKG). Fügen Sie Bereitstellungs- oder Laufzeitressoucen (wie Code, ZIP-Dateien mit Software oder VM-Images) separat zu Azure Stack hinzu und nicht als Teil des Marketplace-Elements. 

## <a name="next-steps"></a>Nächste Schritte
[Erstellen und Veröffentlichen eines Marketplace-Elements](azure-stack-create-and-publish-marketplace-item.md)

