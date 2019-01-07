---
title: Konfiguration einer Testversion für ein Azure-SaaS-Anwendungsangebot | Microsoft-Dokumentation
description: Konfigurieren der Testversion für ein SaaS-Anwendungsangebot im Azure Marketplace.
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
ms.date: 12/04/2018
ms.author: pbutlerm
ms.openlocfilehash: 9d6b72a994b034b0bbc735e5cca2ce987a02af61
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53196100"
---
# <a name="saas-application-test-drive-tab"></a>Registerkarte „Testversion“ für SaaS-Anwendungen

Über die Registerkarte „Testversion“ können Sie eine Testversion für Ihre Kunden bereitstellen.

## <a name="test-drive-benefits"></a>Vorteile einer Testversion

Das Erstellen einer Testversion für Ihre Kunden ist eine bewährte Methode, um sicherzustellen, dass sie ruhigen Gewissens kaufen können. Unter den verfügbaren Testoptionen ist „Testversion“ die wirksamste Option, um aussichtsreiche Leads zu gewinnen und eine vermehrte Umwandlung dieser Leads zu erreichen.

Eine Testversion bietet Kunden einen praktischen, selbstgesteuerten Test der wichtigsten Funktionen und Vorteile Ihres Produkts, die in einem realitätsnahen Implementierungsszenario veranschaulicht werden.

## <a name="how-a-test-drive-works"></a>So funktioniert eine Testversion

Ein potenzieller Kunde hat Ihre Anwendung im Marketplace entdeckt. Der Kunde meldet sich an und stimmt den Nutzungsbedingungen zu. An diesem Punkt empfängt der Kunde Ihre vorkonfigurierte Umgebung, die er für eine feste Anzahl von Stunden testen kann, und Sie empfangen einen aussichtsreichen Lead, den Sie nachverfolgen können. Weitere Informationen finden Sie unter [Was ist die Testversion?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/what-is-test-drive)

## <a name="publishing-steps"></a>Veröffentlichungsschritte

Die Hauptveröffentlichungsschritte für ein Hinzufügen einer Testversion sind:

1. Definieren Ihres Testversionszenarios
2. Erstellen und/oder Ändern Ihrer Resource Manager-Vorlage
3. Erstellen Sie Ihrer schrittweisen Bedienungsanleitung für die Testversion
4. Erneutes Veröffentlichen Ihres Angebots

## <a name="setting-up-a-test-drive"></a>Einrichten einer Testversion

Es sind vier verschiedene Testversionstypen verfügbar, von denen jeder auf dem Typ des Produkts, des Szenarios und des Marketplace basiert, das oder den Sie nutzen.
|  **Typ**          |  **Beschreibung**  |  **Einrichtungsanweisungen**  |
|  ---------------   |  ---------------  |  ---------------  |
|     Azure Resource Manager               |    Eine Azure Resource Manager-Testversion ist eine Bereitstellungsvorlage, die alle Azure-Ressourcen enthält, aus denen eine vom Herausgeber erstellte Lösung besteht. Produkte, die für diesen Testversionstyp geeignet sind, sind Produkte, in denen nur Azure-Ressourcen verwendet werden.               |       [Azure Resource Manager-Testversion](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/azure-resource-manager-test-drive)            |
|       Gehostet             |       Eine gehostete Testversion vereinfacht die Einrichtung dadurch, dass Microsoft das Hosting und die Wartung des Diensts übernimmt, der die Bereitstellung und Aufhebung der Bereitstellung der Testversion durchführt.             |         [Gehostete Testversion](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/hosted-test-drive)          |
|      Logik-App              |       Eine Logik-App-Testversion ist eine Bereitstellungsvorlage, die für alle komplexen Lösungsarchitekturen vorgesehen ist. Alle Dynamics-Anwendungen oder benutzerdefinierten Produkte sollten mit diesem Testversionstyp genutzt werden.            |      [Testversion für Logik-Apps](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/logic-app-test-drive)             |
|       Power BI             |         Eine Power BI-Testversion besteht aus einem eingebetteten Link zu einem kundenspezifischen Dashboard. Für jedes Produkt, mit dem eine interaktive Power BI-Visualisierung demonstriert werden soll, sollte dieser Testversionstyp verwendet werden. Sie müssen lediglich Ihre eingebettete Power BI-URL hochladen.          |        [Power BI-Testversion](#Power-BI-test-drive)           |

### <a name="power-bi-test-drive"></a>Power BI-Testversion

Gehen Sie wie folgt vor, um eine Testversion zu konfigurieren.

1. Wählen Sie unter „Neues Angebot“ die Option **Testversion** aus.
2. Wählen Sie unter „Testversion“ die Option **Ja** aus.

   ![Testversion aktivieren](./media/saas-enable-test-drive.png)

   Wenn Sie eine Testversion aktiviert haben, werden die Formulare „Details“ und „Technische Konfiguration“ angezeigt, die in der nächsten Bildschirmaufnahme zu sehen sind.

   ![Formular für Konfiguration einer Testversion](./media/saas-test-drive-yes.png)

3. Geben Sie unter **Details** Informationen in die folgenden Felder ein:
  
   - Beschreibung: Beschreiben Sie Ihre Testversion und die Aktionen, die Benutzer mit ihr ausführen können. Sie können einfache HTML-Tags verwenden, um diese Beschreibung zu formatieren.
   - Benutzerhandbuch: Laden Sie ein Benutzerhandbuch-Dokument hoch, das die Kunden verwenden können, wenn sie die Testversion nutzen. Dieses Handbuch muss als PDF-Datei vorliegen.
   - Demovideo für Testversion (optional): Sie können ein Video (YouTube oder Vimeo) für Ihre Kunden bereitstellen, das diese sich vor dem Nutzen der Testversion ansehen können. Geben Sie eine URL zum Video an.

4. Geben Sie unter **Technische Konfiguration** Informationen in die folgenden Felder ein:

   - Typ der Testversion: Wählen Sie **Power BI** in der Dropdownliste aus.
   - Link zu Power BI-Dashboard: Geben Sie einen Link zum Dashboard ein.

5. Wenn Sie die Konfiguration der Testversion abgeschlossen haben, wählen Sie **Speichern** aus.


## <a name="next-steps"></a>Nächste Schritte

[Registerkarte „Storefront Details“ (Storefrontdetails)](./cpp-storefront-tab.md)