---
title: Testversion für ein Azure-Anwendungsangebot | Microsoft-Dokumentation
description: Konfigurieren einer Testversion für ein Azure-Anwendungsangebot im Azure Marketplace.
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
ms.openlocfilehash: d51c7330acd37d8a861c136ea85a2ff456eb264e
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57446485"
---
# <a name="azure-applications-test-drive-tab"></a>Registerkarte „Testversion“ für Azure-Anwendungen

Über die Registerkarte „Testversion“ können Sie eine Testversion für Ihre Kunden bereitstellen.

## <a name="test-drive-benefits"></a>Vorteile einer Testversion

Das Erstellen einer Testversion für Ihre Kunden ist eine bewährte Methode, um sicherzustellen, dass sie ruhigen Gewissens kaufen können. Unter den verfügbaren Testoptionen ist „Testversion“ die wirksamste Option, um aussichtsreiche Leads zu gewinnen und eine vermehrte Umwandlung dieser Leads zu erreichen.

Diese Option bietet Kunden einen praktischen, selbstgesteuerten Test der wichtigsten Funktionen und Vorteile Ihres Produkts, die in einem realitätsnahen Implementierungsszenario veranschaulicht werden.

## <a name="how-a-test-drive-works"></a>So funktioniert eine Testversion

Ein potenzieller Kunde hat Ihre Anwendung im Marketplace entdeckt. Der Kunde meldet sich an und stimmt den Nutzungsbedingungen zu. An diesem Punkt empfängt der Kunde Ihre vorkonfigurierte Umgebung, die er für eine feste Anzahl von Stunden testen kann, und Sie empfangen einen aussichtsreichen Lead, den Sie nachverfolgen können. Weitere Informationen finden Sie unter [Was ist die Testversion?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)

## <a name="setting-up-a-test-drive"></a>Einrichten einer Testversion

Gehen Sie wie folgt vor, um eine Testversion zu aktivieren und zu konfigurieren.

### <a name="to-enable-a-test-drive"></a>So aktivieren Sie eine Testversion:

1. Wählen Sie unter **Neues Angebot** die Registerkarte **Testversion** aus.
2. Wählen Sie unter **Testversion** die Option **Ja** für **Testversion aktivieren** aus.

   ![Aktivieren einer Testversion](./media/managed-app-enable-testdrive.png)

### <a name="to-configure-a-test-drive"></a>So konfigurieren Sie eine Testversion:

Nachdem Sie eine kostenlose Testversion aktiviert haben, füllen Sie die folgenden Formulare aus, um die Testversion einzurichten:
  
 - Details
 - Technische Konfiguration
 - Abonnementdetails für Bereitstellung der Testversion

In der nächsten Bildschirmaufnahme sind alle Formulare für eine Testversion dargestellt. Mit einem Sternchen (*) gekennzeichnete Felder sind Pflichtfelder. 

![Konfigurieren einer Testversion](./media/managed-app-configure-testdrive.png)

In der folgenden Tabelle sind die Felder beschrieben, die zum Einrichten der Testversion für Ihre verwaltete Anwendung erforderlich sind.

|    **Feld**       |  **Beschreibung**  |
|  ---------------   |  ---------------  |
|      BESCHREIBUNG              |   Beschreiben Sie, wozu Ihre Testversion verwendet werden kann. Sie können einfache HTML-Tags verwenden, um diese Beschreibung zu formatieren. Zum Beispiel &lt;p&gt;, &lt;Em&gt;, &lt;Ul&gt;, &lt;li&gt;, &lt;Ol&gt; und Überschriften.                |
|    Benutzerhandbuch                |     Laden Sie ein Benutzerhandbuch hoch, über das Ihre Kunden die Benutzeroberfläche der Testversion durchlaufen können. Dieses Dokument muss eine PDF-Datei sein.              |
|         Demovideo für Testversion (optional)           |       Sie können eine Videoanleitung für Ihre Testversion bereitstellen. Ein Kunde kann sich dieses Video vor dem Nutzen der Testversion ansehen. Geben Sie die URL zu dem auf YouTube oder Vimeo befindlichen Video an. Wenn Sie **+ Video hinzufügen** auswählen, werden Sie aufgefordert, die folgenden Informationen bereitzustellen:<ul><li>NAME</li><li>URL</li><li>Vorschauminiatur (im PNG-Format, 533 x 324 Pixel)</li></ul>            |
|       Instanzen             |        Konfigurieren Sie die Anzahl der gewünschten Instanzen, die Regionen und wie schnell Kunden die Testversion erhalten können. Weitere Informationen finden Sie unter [Veröffentlichen einer Testversion](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive#how-to-publish-a-test-drive).           |
|       Dauer der Testversion (Stunden)             |       Geben Sie eine ganze Zahl für die Anzahl der Stunden ein. Der zulässige Bereich liegt zwischen 1 und 999.            |
|        ARM-Vorlage für Testversion            |        Laden Sie eine komprimierte Datei (ZIP-Datei) hoch, die Ihre Azure Resource Manager-Vorlagen für Ihre App enthält. Weitere Informationen finden Sie unter [Azure Resource Manager-Testversion](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive).            |
|        Zugriffsinformationen            |         Geben Sie die Informationen an, die Ihr Kunde nach dem Abrufen der Testversion für den Zugriff benötigt. Zum Beispiel eine URL für den Zugriff auf die Testversion und Anmeldeinformationen. . Sie können einfache HTML-Tags verwenden, um diese Beschreibung zu formatieren. Zum Beispiel &lt;p&gt;, &lt;Em&gt;, &lt;Ul&gt;, &lt;li&gt;, &lt;Ol&gt; und Überschriften.          |
|       Azure-Abonnement-ID             |       Diese gewährt Zugriff auf Azure-Dienste und das Azure-Portal. Im Abonnement werden die Ressourcenverwendung protokolliert und die Dienste abgerechnet. Wenn Sie noch kein separates Azure-Abonnement nur für Testversionen haben, erstellen Sie ein Abonnement.             |
|          Azure AD-Mandanten-ID          |        Geben Sie einen in Azure Active Directory vorhandenen Mandanten an, oder erstellen Sie einen Mandanten für diese Testversion.           |
|         Azure AD-App-ID           |       Erstellen und registrieren Sie eine neue Anwendung. Microsoft verwendet diese Anwendung, um Vorgänge in Ihrer Testversionsinstanz auszuführen.            |
|          Azure AD-App-Schlüssel          |         Erstellen Sie einen Authentifizierungsschlüssel für die App, und fügen Sie ihn in dieses Feld.          |

Nachdem Sie alle erforderlichen Informationen angegeben haben, wählen Sie **Speichern** aus, um das Einrichten der Testversion abzuschließen.

## <a name="next-steps"></a>Nächste Schritte

[Registerkarte „Marketplace“](./cpp-marketplace-tab.md)