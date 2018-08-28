---
title: Hosten von statischen Websites in Azure Storage (Vorschauversion) | Microsoft-Dokumentation
description: Azure Storage ermöglicht jetzt das Hosten von statischen Websites (Vorschauversion), und es wird eine kostengünstige, skalierbare Lösung zum Hosten von modernen Webanwendungen bereitgestellt.
services: storage
author: MichaelHauss
ms.service: storage
ms.topic: article
ms.date: 08/17/18
ms.author: mihauss
ms.component: blobs
ms.openlocfilehash: 65a1cd85baf18ac1f0d193e7e6d6c3139919fb59
ms.sourcegitcommit: a62cbb539c056fe9fcd5108d0b63487bd149d5c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2018
ms.locfileid: "42617396"
---
# <a name="static-website-hosting-in-azure-storage-preview"></a>Hosten von statischen Websites in Azure Storage (Vorschauversion)
Azure Storage ermöglicht jetzt das Hosten von statischen Websites (Vorschauversion), und Sie können moderne Webanwendungen kostengünstig und skalierbar in Azure bereitstellen. Auf einer statischen Website enthalten Webseiten statischen Inhalt und JavaScript oder anderen clientseitigen Code. Im Gegensatz dazu sind dynamische Websites von serverseitigem Code abhängig und können mit [Azure-Web-Apps](/azure/app-service/app-service-web-overview) gehostet werden.

Da für Bereitstellungen immer häufiger elastische, kostengünstige Modelle verwendet werden, ist es wichtig, dass Webinhalte ohne Durchführung einer Serververwaltung bereitgestellt werden können. Die Einführung des Hostens von statischen Websites in Azure Storage macht dies möglich, indem über umfassende Back-End-Funktionen mit serverlosen Architekturen [Azure Functions](/azure/azure-functions/functions-overview) und andere PaaS-Dienste genutzt werden können.

## <a name="how-does-it-work"></a>Wie funktioniert dies?
Wenn Sie die Nutzung von statischen Websites unter Ihrem Speicherkonto aktivieren, wird ein neuer Webdienst-Endpunkt der Art `<account-name>.<zone-name>.web.core.windows.net` erstellt.

Der Webdienst-Endpunkt ermöglicht jederzeit den anonymen Lesezugriff, gibt als Antwort auf Dienstfehler formatierte HTML-Seiten zurück und lässt nur Objektlesevorgänge zu. Der Webdienst-Endpunkt gibt das Indexdokument im angeforderten Verzeichnis für das Stammverzeichnis und alle Unterverzeichnisse zurück. Wenn der Speicherdienst den Fehler 404 zurückgibt, gibt der Webendpunkt ein benutzerdefiniertes Fehlerdokument zurück, falls Sie dies konfiguriert haben.

Der Inhalt für Ihre statische Website wird in einem speziellen Container mit dem Namen „$web“ gehostet. Im Rahmen des Aktivierungsprozesses wird „$web“ für Sie erstellt, falls er nicht bereits vorhanden ist. Auf Inhalte in „$web“ kann über das Stammverzeichnis des Kontos zugegriffen werden, indem der Webendpunkt verwendet wird. `https://contoso.z4.web.core.windows.net/` gibt beispielsweise das Indexdokument zurück, das Sie für Ihre Website konfiguriert haben, wenn ein Dokument dieses Namens im Stammverzeichnis von „$web“ enthalten ist.

Verwenden Sie den Blob Storage-Endpunkt, wenn Sie Inhalte auf Ihre Website hochladen. Verwenden Sie die folgende URL, um ein Blob mit dem Namen „image.jpg“ hochzuladen, auf das im Stammverzeichnis des Kontos zugegriffen werden kann: `https://contoso.blob.core.windows.net/$web/image.jpg`. Sie können das hochgeladene Bild in einem Webbrowser auf dem entsprechenden Webendpunkt anzeigen: `https://contoso.z4.web.core.windows.net/image.jpg`.


## <a name="custom-domain-names"></a>Benutzerdefinierte Domänennamen
Sie können eine benutzerdefinierte Domäne verwenden, um Ihre Webinhalte zu hosten. Befolgen Sie hierzu die Anleitung unter [Konfigurieren eines benutzerdefinierten Domänennamens für Ihren Blob Storage-Endpunkt](storage-custom-domain-name.md). Informationen zum Zugreifen auf Ihre Website, die unter einem benutzerdefinierten Domänennamen gehostet wird, per HTTPS finden Sie unter [Verwenden von Azure-CDN zum Zugreifen auf Blobs mit benutzerdefinierten Domänen über HTTPS](storage-https-custom-domain-cdn.md). Verweisen Sie Ihr CDN auf den Webendpunkt (nicht auf den Blobendpunkt), und denken Sie daran, dass die CDN-Konfiguration nicht sofort erfolgt, sodass Sie möglicherweise einige Minuten warten müssen, bis Ihr Inhalt sichtbar wird.

## <a name="pricing-and-billing"></a>Preise und Abrechnung
Für das Hosten von statischen Websites werden keine zusätzlichen Kosten berechnet. Weitere Informationen zu Preisen für Azure Blob Storage finden Sie auf der Seite [Preise für Azure Blob Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="quickstart"></a>Schnellstart
### <a name="azure-portal"></a>Azure-Portal
Wenn noch nicht geschehen, erstellen Sie ein [GPv2-Speicherkonto](../common/storage-quickstart-create-account.md). Um das Hosting Ihrer Webanwendung zu starten, können Sie die Funktion über das Azure Portal konfigurieren und in der linken Navigationsleiste unter „Einstellungen“ auf „Statische Website (Vorschau)“ klicken. Klicken Sie auf „Aktiviert“, und geben Sie den Namen des Indexdokuments und (optional) den Pfad zum benutzerdefinierten Fehlerdokument ein.

![](media/storage-blob-static-website/storage-blob-static-website-portal-config.PNG)

Laden Sie Ihre Webressourcen in den Container „$web“ hoch, der im Rahmen der Aktivierung von statischen Websites erstellt wurde. Sie können dies direkt im Azure-Portal durchführen oder den [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) nutzen, um vollständige Verzeichnisstrukturen hochzuladen. Achten Sie darauf, ein Indexdokument mit dem konfigurierten Namen einzubinden. In diesem Beispiel hat das Dokument den Namen „index.html“.

> [!NOTE]
> Für den Dokumentnamen wird die Groß-/Kleinschreibung beachtet, deshalb muss er mit dem Namen der Datei im Speicher genau übereinstimmen.

Navigieren Sie abschließend zu Ihrem Webendpunkt, um Ihre Website zu testen.

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Installieren Sie die Storage-Erweiterung (Vorschauversion):

```azurecli-interactive
az extension add --name storage-preview
```
Aktivieren Sie das Feature:

```azurecli-interactive
az storage blob service-properties update --account-name <account-name> --static-website --404-document <error-doc-name> --index-document <index-doc-name>
```
Fragen Sie die Webendpunkt-URL ab:

```azurecli-interactive
az storage account show -n <account-name> -g <resource-group> --query "primaryEndpoints.web" --output tsv
```

Laden Sie Objekte in den $web-Container hoch:

```azurecli-interactive
az storage blob upload-batch -s deploy -d $web --account-name <account-name>
```

## <a name="faq"></a>Häufig gestellte Fragen
**Sind statische Websites für alle Arten von Speicherkonten verfügbar?**  
Nein. Das Hosten von statischen Websites ist nur für Standardspeicherkonten vom Typ GPv2 verfügbar.

**Werden Storage-VNET- und Firewallregeln für den neuen Webendpunkt unterstützt?**  
Ja. Für den neuen Webendpunkt werden die VNET- und Firewallregeln beachtet, die für das Speicherkonto konfiguriert werden.

**Muss für den Webendpunkt zwischen Groß-/Kleinschreibung unterschieden werden?**  
Ja, für den Webendpunkt muss ebenso wie für den Blobendpunkt zwischen Groß-/ Kleinschreibung unterschieden werden. 

## <a name="next-steps"></a>Nächste Schritte
* [Verwenden von Azure-CDN zum Zugreifen auf Blobs mit benutzerdefinierten Domänen über HTTPS](storage-https-custom-domain-cdn.md)
* [Konfigurieren eines benutzerdefinierten Domänennamens für Ihren Blob Storage-Endpunkt](storage-custom-domain-name.md)
* [Azure-Funktionen](/azure/azure-functions/functions-overview)
* [Azure-Web-Apps](/azure/app-service/app-service-web-overview)
* [Build a serverless web app in Azure](https://aka.ms/static-serverless-webapp) (Erstellen einer serverlosen Web-App in Azure)
