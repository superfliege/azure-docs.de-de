---
title: 'Tutorial: Erstellen und Verwalten von exportierten Daten aus Azure Cost Management | Microsoft-Dokumentation'
description: Dieser Artikel erläutert, wie Sie aus Azure Cost Management exportierte Daten erstellen und verwalten können, um sie in externen Systemen zu verwenden.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 7f93a225db845840545b761d812f5a8a81f76f91
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913562"
---
# <a name="tutorial-create-and-manage-exported-data"></a>Tutorial: Erstellen und Verwalten von exportierten Daten

Wenn Sie das Tutorial zur Kostenanalyse gelesen haben, dann sind Sie bereits mit dem manuellen Herunterladen Ihrer Daten zum Cost Management vertraut. Sie können jedoch eine sich täglich wiederholende Aufgabe erstellen, die Ihre Cost Management-Daten täglich automatisch in den Azure Storage exportiert. Die Daten werden im CSV-Format exportiert und enthalten alle Informationen, die von Cost Management gesammelt wurden. Sie können dann die exportierten Daten in Azure Storage mit externen Systemen verwenden und mit Ihren eigenen benutzerdefinierten Daten kombinieren. Darüber hinaus können Sie Ihre exportierten Daten in einem externen System wie einem Dashboard oder einem anderen Finanzsystem verwenden.

Die Beispiele in diesem Tutorial zeigen Ihnen, wie Sie Ihre Cost Management-Daten exportieren und dann überprüfen, ob die Daten erfolgreich exportiert wurden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines täglichen Exports
> * Überprüfen, ob Daten gesammelt wurden

## <a name="prerequisites"></a>Voraussetzungen

Der Datenexport steht allen [EA-Kunden (Enterprise Agreement)](https://azure.microsoft.com/pricing/enterprise-agreement/) zur Verfügung. Die folgenden Azure-Berechtigungen werden pro Abonnement für den Datenexport nach Benutzer und Gruppe unterstützt:

- Besitzer – kann geplante Exporte für ein Abonnement erstellen, ändern oder löschen.
- Mitwirkende – kann eigene geplante Exporte erstellen, ändern oder löschen. Kann den Namen der von anderen Personen erstellten Exporte ändern.
- Leser – kann Exporte planen, für die er die Berechtigung hat.

Für Azure Storage-Konten:
- Um das konfigurierte Speicherkonto zu ändern, sind unabhängig von den Berechtigungen für den Export Schreibberechtigungen erforderlich.
- Ihr Azure Storage-Konto muss für Blob oder File Storage konfiguriert werden.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure
Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com/) beim Azure-Portal an.

## <a name="create-a-daily-export"></a>Erstellen eines täglichen Exports

Cost Management + Abrechnung &gt; Cost Management &gt; Abonnement oder eine Ressourcengruppe in einem Abonnement auswählen &gt; Export &gt; **Hinzufügen**.

Geben Sie einen Namen für den Export ein und geben Sie das Abonnement, das Azure Storage-Konto, den Container und das Dateispeicherverzeichnis oder den Blob-Container an, und klicken Sie dann auf **Erstellen**.

![Neuer Export](./media/tutorial-export-acm-data/new-export01.png)

Ihr neuer Export wird in der Liste der Exporte angezeigt. Standardmäßig sind neue Exporte aktiviert und werden täglich ausgeführt. Wenn Sie einen geplanten Export deaktivieren oder löschen möchten, klicken Sie auf ein beliebiges Element in der Liste und anschließend entweder auf **Deaktivieren** oder **Löschen**.

Zunächst kann es ein bis zwei Stunden dauern, bis der Export ausgeführt wird. Es kann jedoch bis zu vier Stunden dauern, bis die Daten in exportierten Dateien angezeigt werden.

## <a name="verify-that-data-is-collected"></a>Überprüfen, ob Daten gesammelt wurden

Sie können ganz einfach überprüfen, ob Ihre Cost Management-Daten erfasst werden, und die exportierte CSV-Datei mit dem Azure Storage-Explorer anzeigen.

Klicken Sie in der Exportliste auf den Namen des Speicherkontos. Klicken Sie auf der Seite des Speicherkontos auf „Im Explorer öffnen“. Wenn ein Bestätigungsdialogfeld angezeigt wird, klicken Sie auf **Ja**, um die Datei in Azure Storage-Explorer zu öffnen.

![Speicherkontoseite](./media/tutorial-export-acm-data/storage-account-page.png)

Navigieren Sie im Storage-Explorer zu dem Container, den Sie öffnen möchten, und wählen Sie den Ordner aus, der dem aktuellen Monat entspricht. Es wird eine Liste der CSV-Dateien angezeigt. Wählen Sie eine Datei aus, und klicken Sie anschließend auf **Öffnen**.

![Storage-Explorer](./media/tutorial-export-acm-data/storage-explorer.png)

Die Datei wird mit dem Programm oder der Anwendung geöffnet, die zum Öffnen von CSV-Dateierweiterungen ausgewählt ist. Hier sehen Sie ein Beispiel in Excel.

![Beispiel für exportierte Daten](./media/tutorial-export-acm-data/example-export-data.png)

## <a name="access-exported-data-from-other-systems"></a>Zugreifen auf exportierte Daten über andere Systeme

Einer der Gründe für den Export Ihrer Daten aus Cost Management ist der Zugriff auf die Daten über externe Systeme. Sie können ein Dashboard oder ein anderes Finanzsystem verwenden. Derartige Systeme unterscheiden sich stark, sodass es nicht praktikabel wäre, ein Beispiel zu zeigen.  Sie können jedoch mit dem Zugriff auf Ihre Daten aus Ihren Anwendungen unter [Einführung in Azure Storage](../storage/common/storage-introduction.md) beginnen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines täglichen Exports
> * Überprüfen, ob Daten gesammelt wurden

Fahren Sie mit dem nächsten Tutorial fort, um sich Möglichkeiten der Optimierung und Steigerung der Effizienz anzuschauen, indem ungenutzte oder nicht ausreichend genutzte Ressourcen ermittelt werden.

> [!div class="nextstepaction"]
> [Prüfen und Reagieren auf Empfehlungen zur Optimierung](tutorial-acm-opt-recommendations.md)
