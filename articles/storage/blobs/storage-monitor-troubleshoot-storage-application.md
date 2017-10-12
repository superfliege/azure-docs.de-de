---
title: "Überwachung und Problembehandlung für eine Cloudspeicheranwendung in Azure | Microsoft-Dokumentation"
description: "Sie können Diagnosetools, Metriken und Warnungen für die Problembehandlung und Überwachung von Cloudanwendungen verwenden."
services: storage
documentationcenter: 
author: georgewallace
manager: timlt
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 09/19/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: db88c331f79d83e0124519f8b6dbb34514b456dd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-and-troubleshoot-a-cloud-storage-application"></a>Überwachung und Problembehandlung für eine Cloudspeicheranwendung

Dieses Tutorial ist der vierte und letzte Teil einer Serie. Sie erfahren, wie Sie Überwachung und Problembehandlung für eine Cloudspeicheranwendung durchführen.

Im vierten Teil der Serie lernen Sie Folgendes:

> [!div class="checklist"]
> * Aktivieren von Protokollierung und Metriken
> * Aktivieren von Warnungen für Autorisierungsfehler
> * Ausführen von Testdatenverkehr mit falschen SAS-Token
> * Herunterladen und Analysieren von Protokollen

Die [Azure Storage-Analyse](../common/storage-analytics.md) stellt die Protokollierungs- und Metrikdaten für ein Speicherkonto bereit. Diese Daten bieten Einblicke in die Integrität Ihres Speicherkontos. Bevor Sie Ihr Speicherkonto analysieren können, müssen Sie die Datensammlung einrichten. Hierzu müssen Sie die Protokollierung aktivieren, Metriken konfigurieren und Warnungen aktivieren.

Protokolle und Metriken für Speicherkonten werden über die Registerkarte **Diagnose** im Azure-Portal aktiviert. Es gibt zwei Arten von Metriken: Bei **aggregierten** Metriken werden Eingang/Ausgang, Verfügbarkeit, Latenz sowie Erfolgsprozentwerte erfasst. Diese Metriken werden für die Blob-, Warteschlangen-, Tabellen- und Dateidienste aggregiert. Bei **Pro API**-Metriken wird der gleichen Satz Metriken für jeden Speichervorgang in der Azure Storage-Dienst-API erfasst. Die Speicherprotokollierung ermöglicht es Ihnen, Details zu erfolgreichen und fehlerhaften Anforderungen in Ihrem Speicherkonto aufzuzeichnen. Anhand dieser Protokolle können Sie Informationen zu Lese-, Schreib- und Löschvorgängen für Ihre Tabellen, Warteschlangen und Blobs in Azure anzeigen. Außerdem können Sie die Ursachen für Anforderungsfehler wie Zeitüberschreitungen, Drosselung und Autorisierungsfehler sehen.

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com)

## <a name="turn-on-logging-and-metrics"></a>Aktivieren von Protokollierung und Metriken

Wählen Sie im linken Menü **Ressourcengruppen**, **myResourceGroup** und dann Ihr Speicherkonto in der Ressourcenliste aus.

Legen Sie unter **Diagnose** die Option **Status** auf **Ein** fest. Stellen Sie sicher, dass die Optionen **Aggregierte Blobmetriken**, **Blobmetriken pro API** und **Blobprotokolle** aktiviert sind.

Wenn Sie fertig sind, klicken Sie auf **Speichern**.

![Diagnosebereich](media/storage-monitor-troubleshoot-storage-application/figure1.png)

## <a name="enable-alerts"></a>Aktivieren von Warnungen

Warnungen bieten die Möglichkeit, Administratoren per E-Mail zu benachrichtigen oder einen Webhook auszulösen, wenn eine Metrik einen Schwellenwert überschreitet. In diesem Beispiel aktivieren Sie eine Warnung für die Metrik `SASClientOtherError`.

### <a name="navigate-to-the-storage-account-in-the-azure-portal"></a>Navigieren Sie im Azure-Portal zum Speicherkonto.

Wählen Sie im linken Menü **Ressourcengruppen**, **myResourceGroup** und dann Ihr Speicherkonto in der Ressourcenliste aus.

Wählen Sie im Abschnitt **Überwachung** die Option **Warnungsregeln** aus.

Wählen Sie **+ Warnung hinzufügen** aus, und geben Sie unter **Warnungsregel hinzufügen** die erforderlichen Informationen ein. Wählen Sie in der Dropdownliste **Metrik** die Option `SASClientOtherError` aus.

![Diagnosebereich](media/storage-monitor-troubleshoot-storage-application/figure2.png)

## <a name="simulate-an-error"></a>Simulieren eines Fehlers

Zum Simulieren einer gültigen Warnung können Sie ein nicht vorhandenes Blob aus Ihrem Speicherkonto anfordern. Ersetzen Sie hierzu den Wert `<incorrect-blob-name>` durch einen Wert, der nicht vorhanden ist. Führen Sie das folgende Codebeispiel einige Male aus, um ungültige Blobanforderungen zu simulieren.

```azurecli-interactive
sasToken=$(az storage blob generate-sas \
    --account-name <storage-account-name> \
    --account-key <storage-account-key> \
    --container-name <container> \
    --name <incorrect-blob-name> \
    --permissions r \
    --expiry `date --date="next day" +%Y-%m-%d` \
    --output tsv)

curl https://<storage-account-name>.blob.core.windows.net/<container>/<incorrect-blob-name>?$sasToken
```

Die folgende Abbildung zeigt eine Beispielwarnung für den simulierten Fehler auf Grundlage des oben beschriebenen Beispiels.

 ![Beispiel für Warnung](media/storage-monitor-troubleshoot-storage-application/alert.png)

## <a name="download-and-view-logs"></a>Herunterladen und Anzeigen von Protokollen

In Speicherprotokollen werden Daten in einem Satz von Blobs in einem Blobcontainer mit dem Namen **$logs** in Ihrem Speicherkonto gespeichert. Dieser Container wird nicht angezeigt, wenn Sie die Blobcontainer in Ihrem Konto auflisten. Sie können seinen Inhalt aber anzeigen, wenn Sie direkt darauf zugreifen.

In diesem Szenario verwenden Sie [Microsoft Message Analyzer](http://technet.microsoft.com/library/jj649776.aspx) für die Interaktion mit Ihrem Azure Storage-Konto.

### <a name="download-microsoft-message-analyzer"></a>Herunterladen von Microsoft Message Analyzer

Laden Sie [Microsoft Message Analyzer](https://www.microsoft.com/download/details.aspx?id=44226) herunter, und installieren Sie die Anwendung.

Starten Sie die Anwendung, und wählen Sie **File** > **Open** > **From Other File Sources** (Datei > Öffnen > Aus anderen Dateiquellen) aus.

Wählen Sie im Dialogfeld **File Selector** (Dateiauswahl) die Option **+ Add Azure Connection** (+ Azure-Verbindung hinzufügen) aus. Geben Sie den **Speicherkontonamen** und den **Kontoschlüssel** ein, und klicken Sie auf **OK**.

![Microsoft Message Analyzer – Dialogfeld zum Hinzufügen einer Azure Storage-Verbindung](media/storage-monitor-troubleshoot-storage-application/figure3.png)

Wenn die Verbindung hergestellt wurde, erweitern Sie die Container in der Speicherstrukturansicht, um die Protokollblobs anzuzeigen. Wählen Sie das aktuelle Protokoll aus, und klicken Sie auf **OK**.

![Microsoft Message Analyzer – Dialogfeld zum Hinzufügen einer Azure Storage-Verbindung](media/storage-monitor-troubleshoot-storage-application/figure4.png)

Klicken Sie im Dialogfeld **New Session** (Neue Sitzung) auf **Start**, um das Protokoll anzuzeigen.

Wenn das Protokoll geöffnet wird, können Sie die Speicherereignisse anzeigen. Wie Sie in der folgenden Abbildung sehen können, wurde `SASClientOtherError` für das Speicherkonto ausgelöst. Weitere Informationen zur Speicherprotokollierung finden Sie unter [Speicheranalyse](../common/storage-analytics.md).

![Microsoft Message Analyzer – Anzeigen von Ereignissen](media/storage-monitor-troubleshoot-storage-application/figure5.png)

[Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) ist ein weiteres Tool für die Interaktion mit Ihren Speicherkonten, einschließlich des Containers **$logs** und der darin enthaltenen Protokolle.

## <a name="next-steps"></a>Nächste Schritte

Im vierten und letzten Teil der Serie wurden Überwachung und Problembehandlung für Speicherkonten behandelt, u.a.:

> [!div class="checklist"]
> * Aktivieren von Protokollierung und Metriken
> * Aktivieren von Warnungen für Autorisierungsfehler
> * Ausführen von Testdatenverkehr mit falschen SAS-Token
> * Herunterladen und Analysieren von Protokollen

Unter diesem Link finden Sie vordefinierte Speicherbeispiele.

> [!div class="nextstepaction"]
> [Beispielskripts für Azure Storage](storage-samples-blobs-cli.md)