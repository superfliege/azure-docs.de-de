---
title: 'Schnellstart: Erstellen einer Azure DNS-Zone und eines -Eintrags mithilfe der Azure CLI'
description: 'Schnellstart: Erfahren Sie, wie Sie eine DNS-Zone und einen DNS-Eintrag in Azure DNS erstellen. Dies ist eine schrittweise Anleitung zum Erstellen und Verwalten Ihrer ersten DNS-Zone und Ihres ersten DNS-Eintrags mithilfe der Azure CLI.'
services: dns
author: vhorne
ms.service: dns
ms.topic: quickstart
ms.date: 12/4/2018
ms.author: victorh
ms.openlocfilehash: 914f6ff67fb3f178a1cc8ad6aa698527cdbf40b4
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52889768"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-azure-cli"></a>Schnellstart: Erstellen einer Azure DNS-Zone und eines -Eintrags mithilfe der Azure CLI

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie mithilfe der für Windows, Mac und Linux verfügbaren Azure CLI Ihre erste DNS-Zone und Ihren ersten DNS-Eintrag erstellen. Sie können diese Schritte auch im [Azure-Portal](dns-getstarted-portal.md) oder mithilfe von [Azure PowerShell](dns-getstarted-powershell.md) ausführen.

Eine DNS-Zone wird zum Hosten der DNS-Einträge für eine bestimmte Domäne verwendet. Wenn Sie eine Domäne in Azure DNS hosten möchten, müssen Sie eine DNS-Zone für diesen Domänennamen erstellen. Jeder DNS-Eintrag für Ihre Domäne wird dann in dieser DNS-Zone erstellt. Und schließlich müssen Sie die Namenserver für die Domäne konfigurieren, um Ihre DNS-Zone im Internet zu veröffentlichen. Jeder dieser Schritte wird im Folgenden beschrieben.

Azure DNS unterstützt jetzt auch private DNS-Zonen (zurzeit als öffentliche Vorschauversion verfügbar). Weitere Informationen zu privaten DNS-Zonen finden Sie unter [Using Azure DNS for private domains](private-dns-overview.md) (Verwenden von Azure DNS für private Domänen). Ein Beispiel für die Erstellung einer privaten DNS-Zone finden Sie unter [Erstellen einer privaten Azure DNS-Zone mit der Azure-Befehlszeilenschnittstelle](./private-dns-getstarted-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-the-resource-group"></a>Ressourcengruppe erstellen

Bevor Sie die DNS-Zone erstellen, erstellen Sie eine Ressourcengruppe für die DNS-Zone:

```azurecli
az group create --name MyResourceGroup --location "East US"
```

## <a name="create-a-dns-zone"></a>Erstellen einer DNS-Zone

Eine DNS-Zone wird mit dem `az network dns zone create` -Befehl erstellt. Hilfe zu diesem Befehl erhalten Sie mit `az network dns zone create -h`.

Im folgenden Beispiel wird in der Ressourcengruppe namens *MyResourceGroup* eine DNS-Zone namens *contoso.com* erstellt. Verwenden Sie das Beispiel, um eine DNS-Zone zu erstellen, und ersetzen Sie dabei die Werte durch Ihre eigenen.

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.com
```

## <a name="create-a-dns-record"></a>Erstellen eines DNS-Eintrags

Verwenden Sie zum Erstellen eines DNS-Eintrags den Befehl `az network dns record-set [record type] add-record`. Hilfe zu A-Einträgen finden Sie unter `azure network dns record-set A add-record -h`.

Im folgenden Beispiel wird ein Eintrag mit dem relativen Namen „www“ unter der DNS-Zone „contoso.com“ in der Ressourcengruppe „MyResourceGroup“ erstellt. Der vollqualifizierte Name des Ressourceneintragssatzes ist „www.contoso.com“. Der Eintragstyp lautet „A“ mit der IP-Adresse „1.2.3.4“ und einer Standardgültigkeitsdauer (TTL) von 3.600 Sekunden (1 Stunde).

```azurecli
az network dns record-set a add-record -g MyResourceGroup -z contoso.com -n www -a 1.2.3.4
```

## <a name="view-records"></a>Anzeigen von Datensätzen

Listen Sie die DNS-Einträge in Ihrer Zone mit diesem Befehl auf:

```azurecli
az network dns record-set list -g MyResourceGroup -z contoso.com
```

## <a name="update-name-servers"></a>Aktualisieren der Namenserver

Wenn Sie sicher sind, dass Ihre DNS-Zone und die Einträge richtig eingerichtet wurden, müssen Sie den Namen Ihrer Domäne für die Verwendung der Azure DNS-Namenserver konfigurieren. So können andere Benutzer im Internet Ihre DNS-Einträge finden.

Die Namenserver für Ihre Zone werden durch den Befehl `az network dns zone show` angegeben. Um die Namen der Namenserver anzuzeigen, verwenden Sie eine JSON-Ausgabe, wie im folgenden Beispiel gezeigt.

```azurecli
az network dns zone show -g MyResourceGroup -n contoso.com -o json

{
  "etag": "00000003-0000-0000-b40d-0996b97ed101",
  "id": "/subscriptions/a385a691-bd93-41b0-8084-8213ebc5bff7/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-01.azure-dns.com.",
    "ns2-01.azure-dns.net.",
    "ns3-01.azure-dns.org.",
    "ns4-01.azure-dns.info."
  ],
  "numberOfRecordSets": 3,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Diese Namenserver müssen in der Domänennamen-Registrierungsstelle konfiguriert werden (das ist die Stelle, bei der Sie den Domänennamen erworben haben). Die Registrierungsstelle bietet eine Option an, um die Namenserver für die Domäne einzurichten. Weitere Informationen finden Sie unter [Tutorial: Hosten Ihrer Domäne in Azure DNS](dns-delegate-domain-azure-dns.md#delegate-the-domain).

## <a name="delete-all-resources"></a>Löschen aller Ressourcen
 
Wenn sie nicht mehr benötigt werden, können Sie alle in diesem Schnellstart erstellten Ressourcen löschen, indem Sie die Ressourcengruppe löschen:

```azurecli
az group delete --name MyResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun Ihre erste DNS-Zone und Ihren ersten -Eintrag mit der Azure CLI erstellt haben, können Sie Einträge für eine Web-App in einer benutzerdefinierten Domäne erstellen.

> [!div class="nextstepaction"]
> [Erstellen von DNS-Einträgen für eine Web-App in einer benutzerdefinierten Domäne](./dns-web-sites-custom-domain.md)
