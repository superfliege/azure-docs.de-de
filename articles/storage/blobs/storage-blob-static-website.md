---
title: Hosten von statischen Websites in Azure Storage
description: Das Hosten von statischen Websites in Azure Storage bietet eine kostengünstige, skalierbare Lösung zum Hosten von modernen Webanwendungen.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 10/19/18
ms.author: tamram
ms.component: blobs
ms.openlocfilehash: 0663f569edd0dec47949053b6ecf7fe49fa24a17
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2019
ms.locfileid: "54229301"
---
# <a name="static-website-hosting-in-azure-storage"></a>Hosten von statischen Websites in Azure Storage
Mit Azure Storage GPv2-Konten können Sie statische Inhalte (HTML-, CSS-, JavaScript- und Bilddateien) direkt über einen Speichercontainer mit dem Namen *$web* bereitstellen. Durch das Hosten in Azure Storage können Sie serverlose Architekturen, wie [Azure Functions](/azure/azure-functions/functions-overview) und andere PaaS-Dienste, verwenden.

Im Unterschied zum Hosten statischer Websites werden dynamische Websites, die von serverseitigem Code abhängig sind, am besten mit [Azure App Service](/azure/app-service/overview) gehostet.

## <a name="how-does-it-work"></a>Wie funktioniert dies?
Wenn Sie das Hosten statischer Websites in Ihrem Speicherkonto aktivieren, wählen Sie den Namen der Standarddatei aus und geben optional einen Pfad zu einer benutzerdefinierten 404-Seite an. Wenn das Feature aktiviert wird, wird ein Container namens *$web* erstellt, wenn er noch nicht vorhanden ist.

Für Dateien im Container *$web* gilt:

- Sie werden über anonyme Zugriffsanforderungen bereitgestellt.
- Sie sind nur über Objektlesevorgänge verfügbar.
- Erfordert eine Beachtung der Groß-/Kleinschreibung.
- Sie sind mit folgendem Muster im öffentlichen Web verfügbar:
    - `https://<ACCOUNT_NAME>.<ZONE_NAME>.web.core.windows.net/<FILE_NAME>`
- Sie sind mit folgendem Muster über einen Blob Storage-Endpunkt verfügbar:
    - `https://<ACCOUNT_NAME>.blob.core.windows.net/$web/<FILE_NAME>`

Sie verwenden den Blob Storage-Endpunkt zum Hochladen von Dateien. Die Datei, die z.B. in diesen Speicherort hochgeladen wurde:

```bash
https://contoso.blob.core.windows.net/$web/image.png
```

steht im Browser an folgendem Speicherort zur Verfügung:

```bash
https://contoso.z4.web.core.windows.net/image.png
```

Der ausgewählte Standarddateiname wird im Stamm und in sämtlichen Unterverzeichnissen verwendet, wenn kein Dateiname angegeben wird. Wenn der Server 404 zurückgibt und Sie keinen Pfad zum Fehlerdokument angeben, wird eine 404-Standardseite an den Benutzer zurückgegeben.

## <a name="cdn-and-ssl-support"></a>CDN- und SSL-Unterstützung

Informationen dazu, wie Sie Ihre statischen Websitedateien über HTTPS verfügbar machen, finden Sie unter [Verwenden von Azure-CDN zum Zugreifen auf Blobs mit benutzerdefinierten Domänen über HTTPS](storage-https-custom-domain-cdn.md). Im Rahmen dieses Prozesses müssen Sie *das CDN auf den Webendpunkt verweisen*, nicht auf den Blobendpunkt. Möglicherweise müssen Sie einige Minuten warten, bevor Ihre Inhalte angezeigt werden, da die CDN-Konfiguration nicht sofort ausgeführt wird.


## <a name="custom-domain-names"></a>Benutzerdefinierte Domänennamen

Sie können [einen benutzerdefinierten Domänennamen für Ihr Azure Storage-Konto konfigurieren](storage-custom-domain-name.md), um Ihre statische Website über eine benutzerdefinierte Domäne verfügbar zu machen. Einen detaillierten Einblick in das Hosten Ihrer Domäne in Azure finden Sie unter [Hosten Ihrer Domäne in Azure DNS](../../dns/dns-delegate-domain-azure-dns.md).

## <a name="pricing"></a>Preise
Für das Hosten von statischen Websites werden keine zusätzlichen Kosten berechnet. Weitere Informationen zu Preisen für Azure Blob Storage finden Sie auf der Seite [Preise für Azure Blob Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="quickstart"></a>Schnellstart

### <a name="azure-portal"></a>Azure-Portal
Öffnen Sie zuerst das Azure-Portal unter https://portal.azure.com, und führen Sie in Ihrem GPv2-Speicherkonto die folgenden Schritte aus:

1. Klicken Sie auf **Einstellungen**.
2. Klicken Sie auf **Statische Website**.
3. Geben Sie einen *Namen für das Indexdokument* ein. (Ein gängiger Wert ist *index.html*.)
4. Geben Sie optional für eine benutzerdefinierte 404-Seite einen *Pfad zum Fehlerdokument* ein. (Ein gängiger Wert ist *404.html*.)

![](media/storage-blob-static-website/storage-blob-static-website-portal-config.PNG)

Laden Sie als Nächstes über das Azure-Portal oder mit dem [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) Ihre Ressourcen in den Container *$web* hoch, um vollständige Verzeichnisse hochzuladen. Stellen Sie sicher, dass Sie eine Datei aufnehmen, die dem *Namen für das Indexdokument* entspricht, den Sie beim Aktivieren des Features ausgewählt haben.

Navigieren Sie abschließend zu Ihrem Webendpunkt, um Ihre Website zu testen.

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Installieren Sie die Storage-Erweiterung (Vorschauversion):

```azurecli-interactive
az extension add --name storage-preview
```
Wenn Sie mehrere Abonnements verwenden, legen Sie die CLI auf das Abonnement des GPv2-Speicherkontos fest, das Sie aktivieren möchten:

```azurecli-interactive
az account set --subscription <SUBSCRIPTION_ID>
```
Aktivieren Sie das Feature. Stellen Sie sicher, dass Sie alle Platzhalterwerte durch Ihre eigenen Werte ersetzen, einschließlich der Klammern:

```azurecli-interactive
az storage blob service-properties update --account-name <ACCOUNT_NAME> --static-website --404-document <ERROR_DOCUMENT_NAME> --index-document <INDEX_DOCUMENT_NAME>
```
Fragen Sie die Webendpunkt-URL ab:

```azurecli-interactive
az storage account show -n <ACCOUNT_NAME> -g <RESOURCE_GROUP> --query "primaryEndpoints.web" --output tsv
```

Laden Sie Objekte aus einem Quellverzeichnis in den Container *$web* hoch. Achten Sie darauf, dass Sie den Verweis auf den Container *$web* im Befehl mit Escapezeichen versehen. Wenn Sie beispielsweise die Azure-Befehlszeilenschnittstelle aus der CloudShell heraus im Azure-Portal verwenden, versehen Sie den Container *$web* mit Escapezeichen wie gezeigt:

```azurecli-interactive
az storage blob upload-batch -s <SOURCE_PATH> -d \$web --account-name <ACCOUNT_NAME>
```

## <a name="deployment"></a>Bereitstellung

Beispielsweise die folgenden Methoden können für die Bereitstellung von Inhalten in einem Speichercontainer verwendet werden:

- [AzCopy](../common/storage-use-azcopy.md)
- [Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/)
- [Azure Pipelines](https://code.visualstudio.com/tutorials/static-website/deploy-VSTS)
- [Visual Studio Code-Erweiterung](https://code.visualstudio.com/tutorials/static-website/getting-started)

Stellen Sie in allen Fällen sicher, dass Sie Dateien in den Container *$web* kopieren.

## <a name="metrics"></a>Metriken

Um Metriken auf den Seiten Ihrer statischen Website zu aktivieren, klicken Sie auf **Einstellungen** > **Überwachung** > **Metriken**.

Metrikdaten werden durch Einbinden verschiedener Metrik-APIs generiert. Das Portal zeigt nur die API-Elemente an, die innerhalb eines bestimmten Zeitrahmens verwendet werden. Dies ermöglicht eine Konzentration auf die Elemente, die Daten zurückgeben. Um sicherzustellen, dass Sie die erforderlichen API-Elemente auswählen können, müssen Sie zuerst den Zeitrahmen erweitern.

Klicken Sie auf die Schaltfläche für den Zeitrahmen, und wählen Sie **Letzte 24 Stunden** aus. Klicken Sie dann auf **Übernehmen**.

![Metriken von statischen Websites in Azure Storage: Zeitbereich](./media/storage-blob-static-website/storage-blob-static-website-metrics-time-range.png)

Wählen Sie als Nächstes **Blob** aus der Dropdownliste *Namespace* aus.

![Metriken von statischen Websites in Azure Storage: Namespace](./media/storage-blob-static-website/storage-blob-static-website-metrics-namespace.png)

Wählen Sie dann die Metrik **Ausgehend** aus.

![Metriken von statischen Websites in Azure Storage: Metrik](./media/storage-blob-static-website/storage-blob-static-website-metrics-metric.png)

Wählen Sie **Summe** aus dem Selektor *Aggregation* aus.

![Metriken von statischen Websites in Azure Storage: Aggregation](./media/storage-blob-static-website/storage-blob-static-website-metrics-aggregation.png)

Klicken Sie anschließend auf die Schaltfläche **Filter hinzufügen**, und wählen Sie **API-Name** aus dem Selektor *Eigenschaft* aus.

![Metriken von statischen Websites in Azure Storage: API-Name](./media/storage-blob-static-website/storage-blob-static-website-metrics-api-name.png)

Aktivieren Sie schließlich das Kontrollkästchen neben **GetWebContent** im Selektor *Werte*, um den Metrikbericht zu füllen.

![Metriken von statischen Websites in Azure Storage: GetWebContent](./media/storage-blob-static-website/storage-blob-static-website-metrics-getwebcontent.png)

Nach der Aktivierung werden Datenverkehrsstatistiken zu Dateien im Container *$web* im Dashboard für Metriken gemeldet.

## <a name="faq"></a>Häufig gestellte Fragen

**Ist das Feature für statische Websites für alle Arten von Speicherkonten verfügbar?**  
Nein. Das Hosten von statischen Websites ist nur für Standardspeicherkonten vom Typ GPv2 verfügbar.

**Werden Storage-VNET- und Firewallregeln für den neuen Webendpunkt unterstützt?**  
Ja. Für den neuen Webendpunkt werden die VNET- und Firewallregeln beachtet, die für das Speicherkonto konfiguriert werden.

**Muss für den Webendpunkt die Groß-/Kleinschreibung beachtet werden?**  
Ja, für den Webendpunkt muss ebenso wie für den Blobendpunkt zwischen Groß-/Kleinschreibung unterschieden werden. 

## <a name="next-steps"></a>Nächste Schritte
* [Verwenden von Azure-CDN zum Zugreifen auf Blobs mit benutzerdefinierten Domänen über HTTPS](storage-https-custom-domain-cdn.md)
* [Konfigurieren eines benutzerdefinierten Domänennamens für Ihren Blob Storage-Endpunkt](storage-custom-domain-name.md)
* [Azure-Funktionen](/azure/azure-functions/functions-overview)
* [Azure App Service](/azure/app-service/overview)
* [Build a serverless web app in Azure](https://docs.microsoft.com/azure/functions/tutorial-static-website-serverless-api-with-database) (Erstellen einer serverlosen Web-App in Azure)
* [Tutorial: Hosten Ihrer Domäne in Azure DNS](../../dns/dns-delegate-domain-azure-dns.md)
