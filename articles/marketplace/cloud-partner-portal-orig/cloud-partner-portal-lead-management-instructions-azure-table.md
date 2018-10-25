---
title: Azure-Tabelle | Microsoft-Dokumentation
description: Konfigurieren der Leadverwaltung mit Azure-Tabellen
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
ms.date: 09/14/2018
ms.author: pbutlerm
ms.openlocfilehash: 60e3e3d81b07bf7ae681b5cef2d6d9681877a35f
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806671"
---
<a name="lead-management-instructions-for-azure-table"></a>Anweisungen für die Leadverwaltung für Azure-Tabellen
============================================

Dieser Artikel beschreibt, wie Sie eine Azure-Tabelle zum Speichern von Vertriebschancen (Leads) konfigurieren. Sie können in Azure-Tabellen Informationen zu Kunden speichern und anpassen.

## <a name="to-configure-azure-table"></a>So konfigurieren Sie eine Azure-Tabelle

1.  Wenn Sie nicht über ein Azure-Konto verfügen, können Sie ein [kostenloses Testkonto erstellen](https://azure.microsoft.com/pricing/free-trial/).

2.  Nachdem Ihr Azure-Konto aktiviert wurde, melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
3.  Erstellen Sie im Azure-Portal ein Speicherkonto. In der nächsten Screenshot wird das Erstellen eines Speicherkontos veranschaulicht. Weitere Informationen zu den Preisen für Storage finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

    ![Schritte zum Erstellen eines Azure Storage-Kontos](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragecreate.png)

4.  Kopieren Sie die Verbindungszeichenfolge des Speicherkontos für den Schlüssel, und fügen Sie sie im Cloud-Partnerportal im Feld **Verbindungszeichenfolge für Speicherkonto** ein. Ein Beispiel für eine Verbindungszeichenfolge ist `DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net `
    
    ![Azure-Speicherschlüssel](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragekeys.png)

Sie können die Daten in Ihrer Speichertabelle im [Azure Storage-Explorer](http://azurestorageexplorer.codeplex.com/) oder einem anderen Tool anzeigen. Sie können die Daten auch in eine Azure-Tabelle exportieren.
Daten.

## <a name="optional-to-use-azure-functions-with-an-azure-table"></a>**Optional:** So verwenden Sie Azure Functions mit einer Azure-Tabelle

Wenn Sie anpassen möchten, wie Sie Leads empfangen, verwenden Sie [Azure Functions](https://azure.microsoft.com/services/functions/) mit einer Azure-Tabelle. Der Azure Functions-Dienst ermöglicht das Automatisieren des Prozesses zum Generieren von Leads.

Die folgenden Schritte zeigen, wie Sie eine Azure-Funktion erstellen, die einen Timer verwendet. Die Funktion sucht alle fünf Minuten in der Azure-Tabelle nach neuen Datensätzen und verwendet dann den SendGrid-Dienst, um eine E-Mail-Benachrichtigung zu senden.


1.  [Erstellen](https://portal.azure.com/#create/SendGrid.SendGrid) Sie in Ihrem Azure-Abonnement ein kostenloses SendGrid-Dienstkonto.

    ![Erstellen eines SendGrid-Kontos](./media/cloud-partner-portal-lead-management-instructions-azure-table/createsendgrid.png)

2.  Erstellen eines SendGrid-API-Schlüssels 
    - Wählen Sie **Verwalten** aus, um zur Benutzeroberfläche von SendGrid zu wechseln.
    - Wählen Sie **Einstellungen**, **API-Schlüssel** aus, und erstellen Sie dann einen Schlüssel mit der Berechtigung „E-Mail senden“ -\> „Vollzugriff“.
    - Speichern Sie den API-Schlüssel.


    ![SendGrid-API-Schlüssel](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridkey.png)


3.  [Erstellen](https://portal.azure.com/#create/Microsoft.FunctionApp) Sie eine Azure-Funktionen-App mithilfe der Hostingplanoption „Verbrauchstarif“.

    ![Erstellen einer Azure Functions-App](./media/cloud-partner-portal-lead-management-instructions-azure-table/createfunction.png)


4.  Erstellen Sie eine neue Funktionsdefinition.

    ![Erstellen einer Azure-Funktionsdefinition](./media/cloud-partner-portal-lead-management-instructions-azure-table/createdefinition.png)
 

5.  Wenn Sie möchten, dass die Funktion zu einem bestimmten Zeitpunkt eine Aktualisierung sendet, wählen Sie **TimerTrigger-CSharp** als Startoption aus.

     ![Azure-Funktion mit uhrzeitabhängiger Triggeroption](./media/cloud-partner-portal-lead-management-instructions-azure-table/timetrigger.png)


6.  Ersetzen Sie den Code von „Develop“ durch folgendes Codebeispiel. Ersetzen Sie die E-Mail-Adressen durch die Adressen, die Sie für Absender und Empfänger verwenden möchten.

        #r "Microsoft.WindowsAzure.Storage"
        #r "SendGrid"
        using Microsoft.WindowsAzure.Storage.Table;
        using System;
        using SendGrid;
        using SendGrid.Helpers.Mail;
        public class MyRow : TableEntity
        {
            public string Name { get; set; }
        }
        public static void Run(TimerInfo myTimer, IQueryable<MyRow> inputTable, out Mail message, TraceWriter log)
        {
            // UTC datetime that is 5.5 minutes ago while the cron timer schedule is every 5 minutes
            DateTime dateFrom = DateTime.UtcNow.AddSeconds(-(5 * 60 + 30));
            var emailFrom = "YOUR EMAIL";
            var emailTo = "YOUR EMAIL";
            var emailSubject = "Azure Table Notification";
            // Look in the table for rows that were added recently
            var rowsList = inputTable.Where(r => r.Timestamp > dateFrom).ToList();
            // Check how many rows were added
            int rowsCount = rowsList.Count;
            if (rowsCount > 0)
            {
                log.Info($"Found {rowsCount} rows added since {dateFrom} UTC");
                // Configure the email message describing how many rows were added
                message = new Mail
                {
                    From = new Email(emailFrom),
                    Subject = emailSubject + " (" + rowsCount + " new rows)"
                };
                var personalization = new Personalization();
                personalization.AddTo(new Email(emailTo));
                message.AddPersonalization(personalization);
                var content = new Content
                {
                    Type = "text/plain",
                    Value = "Found " + rowsCount + " new rows added since " + dateFrom.ToString("yyyy-MM-dd HH:mm:ss") + " UTC"
                };
                message.AddContent(content);
            }
            else
            {
                // Do not send the email if no new rows were found
                message = null;
            }
        }

    ![Codeausschnitt für Azure-Funktion](./media/cloud-partner-portal-lead-management-instructions-azure-table/code.png)


7.  Wählen Sie **Integrieren** und **Eingaben** aus, um die Azure-Tabellenverbindung zu definieren.

    ![Option „Integrieren“ für Azure-Funktion](./media/cloud-partner-portal-lead-management-instructions-azure-table/integrate.png)


8.  Geben Sie den Tabellennamen ein, und definieren Sie die Verbindungszeichenfolge, indem Sie **Neu** auswählen.


    ![Verbindung mit Azure-Funktionen-Tabelle](./media/cloud-partner-portal-lead-management-instructions-azure-table/configtable.png)

9.  Definieren Sie nun die Ausgabe als SendGrid, und übernehmen Sie alle Standardeinstellungen.

    ![SendGrid-Ausgabe](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridoutput.png)

    ![Standardwerte für die SendGrid-Ausgabe](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridoutputdefaults.png)

10. Fügen Sie den SendGrid-API-Schlüssel den Einstellungen der Funktionen-App hinzu, indem Sie den Namen „SendGridApiKey“ und den Wert, den Sie aus „API-Schlüssel“ auf der SendGrid-Benutzeroberfläche abgerufen haben, verwenden.

    ![Verwalten von SendGrid](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridmanage.png)
    ![SendGrid-Schlüssel verwalten](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridmanagekey.png)

Wenn Sie mit dem Konfigurieren der Funktion fertig sind, sollte der Code im Abschnitt „Integrate“ wie im folgenden Beispiel aussehen.

    {
      "bindings": [
        {
          "name": "myTimer",
          "type": "timerTrigger",
          "direction": "in",
          "schedule": "0 */5 * * * *"
        },
        {
          "type": "table",
          "name": "inputTable",
          "tableName": "MarketplaceLeads",
          "take": 50,
          "connection": "yourstorageaccount_STORAGE",
          "direction": "in"
        },
        {
          "type": "sendGrid",
          "name": "message",
          "apiKey": "SendGridApiKey",
          "direction": "out"
        }
      ],
      "disabled": false
    }

11. Im letzte Schritt navigieren Sie zur Entwicklungsbenutzeroberfläche der Funktion und wählen dann **Ausführen** aus, um den Timer zu starten. Jetzt erhalten Sie jedes Mal, wenn ein neuer Lead eingeht, eine Benachrichtigung.
