---
title: Aktivieren der sicheren TLS-Verbindung für den Azure Storage-Client | Microsoft-Dokumentation
description: Informationen zum Aktivieren von TLS 1.2 im Client von Azure Storage.
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: fhryo-msft
ms.assetid: ''
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 06/25/2018
ms.author: fryu
ms.openlocfilehash: 5c21df2b3bdeee6ac7c3956fe1cafa4f947dd6dd
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37034864"
---
# <a name="enable-secure-tls-for-azure-storage-client"></a>Aktivieren der sicheren TLS-Verbindung für den Azure Storage-Client

Wenn Sie Ihre Dienste bei Verwendung von Azure Storage basierend auf den neuesten Konformitäts- und Sicherheitsanforderungen überprüfen möchten, werden SSL 1.0, 2.0, 3.0 und TLS 1.0 als nicht konforme Kommunikationsprotokolle erkannt.

SSL 1.0, 2.0 und 3.0 haben sich als anfällig erwiesen. Sie werden per RFC untersagt. TLS 1.0 wird unsicher durch Verwendung der unsicheren Blockchiffre (DES CBC und RC2 CBC) und Streamchiffre (RC4). Der PCI-Rat hat zudem die Migration zu höheren TLS-Versionen empfohlen. Weitere Details finden Sie unter [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0).

In Azure Storage wurde die Verwendung von SSL 3.0 seit 2015 beendet, TLS 1.2 wird für öffentliche HTTPS-Endpunkte verwendet, TLS 1.0 und TLS 1.1 werden jedoch aus Gründen der Abwärtskompatibilität weiterhin unterstützt.

Um sichere und konforme Verbindungen mit Azure Storage sicherzustellen, müssen Sie TLS 1.2 auf Clientseite aktivieren, bevor Sie Anforderungen zur Verwendung des Azure Storage-Diensts senden.

## <a name="enable-tls-12-in-net-client"></a>Aktivieren von TLS 1.2 im .NET-Client

Damit der Client die Aushandlung über TLS 1.2 durchführt, müssen das Betriebssystem und die .NET Framework-Version beide TLS 1.2 unterstützen. Weitere Details finden Sie unter [Unterstützung für TLS 1.2](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls#support-for-tls-12).

Im folgenden Beispiel wird gezeigt, wie Sie TLS 1.2 in Ihrem .NET-Client aktivieren.

```csharp

    static void EnableTls12()
    {
        // Enable TLS 1.2 before connecting to Azure Storage
        System.Net.ServicePointManager.SecurityProtocol = System.Net.SecurityProtocolType.Tls12;

        // Connect to Azure Storage
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName={yourstorageaccount};AccountKey={yourstorageaccountkey};EndpointSuffix=core.windows.net");
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        CloudBlobContainer container = blobClient.GetContainerReference("foo");
        container.CreateIfNotExists();
    }

```

## <a name="enable-tls-12-in-powershell-client"></a>Aktivieren von TLS 1.2 im PowerShell-Client

Im folgenden Beispiel wird gezeigt, wie Sie TLS 1.2 in Ihrem PowerShell-Client aktivieren.

```powershell

# Enable TLS 1.2 before connecting to Azure Storage
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12;

$resourceGroup = "{YourResourceGropuName}"
$storageAccountName = "{YourStorageAccountNme}"
$prefix = "foo"

# Connect to Azure Storage
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup -Name $storageAccountName
$ctx = $storageAccount.Context
$listOfContainers = Get-AzureStorageContainer -Context $ctx -Prefix $prefix
$listOfContainers

```

## <a name="verify-tls-12-connection"></a>Überprüfen der TLS 1.2-Verbindung

Sie können Fiddler verwenden, um zu überprüfen, ob TLS 1.2 tatsächlich verwendet wird. Öffnen Sie Fiddler, um den Netzwerkdatenverkehr auf dem Client zu erfassen, und führen Sie dann das Beispiel oben aus. Anschließend können Sie die TLS-Version in der Verbindung sehen, die mit dem Beispiel hergestellt wird.

Der folgende Screenshot ist ein Beispiel für die Überprüfung.

![Screenshot zur Überprüfung der TLS-Version in Fiddler](./media/storage-security-tls/storage-security-tls-verify-in-fiddler.png)

## <a name="see-also"></a>Weitere Informationen

* [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0)
