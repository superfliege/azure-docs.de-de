---
title: Erste Schritte mit Azure DNS Private Zones mithilfe von Azure CLI 2.0 | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie eine private DNS-Zone und einen DNS-Eintrag in Azure DNS erstellen. Dies ist eine Schritt-für-Schritt-Anleitung zum Erstellen und Verwalten Ihrer ersten privaten DNS-Zone und Ihres ersten DNS-Eintrags mithilfe von Azure CLI 2.0.
services: dns
documentationcenter: na
author: KumuD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2018
ms.author: kumud
ms.openlocfilehash: d10f3201adb972468d8de7e66a940232ed19562d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="get-started-with-azure-dns-private-zones-using-azure-cli-20"></a>Erste Schritte mit Azure DNS Private Zones mithilfe von Azure CLI 2.0

> [!div class="op_single_selector"]
> * [PowerShell](private-dns-getstarted-powershell.md)
> * [Azure CLI 2.0](private-dns-getstarted-cli.md)

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie mithilfe der plattformübergreifenden Anwendung Azure CLI 2.0 (für Windows, Mac und Linux verfügbar) Ihre erste private DNS-Zone und Ihren ersten DNS-Eintrag erstellen. Sie können diese Schritte auch über Azure PowerShell ausführen.

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

Eine DNS-Zone wird zum Hosten der DNS-Einträge für eine bestimmte Domäne verwendet. Wenn Sie eine Domäne in Azure DNS hosten möchten, müssen Sie eine DNS-Zone für diesen Domänennamen erstellen. Jeder DNS-Eintrag für Ihre Domäne wird dann in dieser DNS-Zone erstellt. Um eine private DNS-Zone in Ihrem virtuellen Netzwerk zu veröffentlichen, geben Sie die Liste mit den virtuellen Netzwerken an, für die das Auflösen von Einträgen in der Zone zulässig ist.  Diese werden als „virtuelle Auflösungsnetzwerke“ bezeichnet.  Sie können auch ein virtuelles Netzwerk angeben, für das Azure DNS Hostnameneinträge verwaltet, wenn eine VM erstellt wird, sich ihre IP-Adresse ändert oder die VM zerstört wird.  Dies wird als „virtuelles Registrierungsnetzwerk“ bezeichnet.

In dieser Anleitung wird vorausgesetzt, dass Sie Azure CLI 2.0 bereits installiert und die Registrierung dafür durchgeführt sowie zusätzlich die erforderliche CLI-Erweiterung zur Unterstützung privater Zonen installiert haben. Hilfe erhalten Sie unter [Verwalten von DNS-Zonen mit der Azure CLI 2.0](dns-operations-dnszones-cli.md).

## <a name="to-installuse-azure-dns-private-zones-feature-public-preview"></a>Installieren und Verwenden des Features „Azure DNS Private Zones“ (Öffentliche Vorschauversion)
Das Feature „Azure DNS Private Zones“ ist als öffentliche Vorschauversion über eine Erweiterung der Azure CLI verfügbar. Installieren Sie die Azure CLI-Erweiterung „dns“ 

```
az extension add --name dns
``` 

## <a name="create-the-resource-group"></a>Ressourcengruppe erstellen

Vor dem Erstellen der DNS-Zone wird eine Ressourcengruppe für die DNS-Zone erstellt. Hier sehen Sie den Befehl:

```azurecli
az group create --name MyResourceGroup --location "West US"
```

## <a name="create-a-dns-private-zone"></a>Erstellen einer privaten DNS-Zone

Eine private DNS-Zone wird mit dem Befehl `az network dns zone create` erstellt. Hilfe zu diesem Befehl erhalten Sie mit `az network dns zone create --help`.

Im folgenden Beispiel wird eine private DNS-Zone mit dem Namen *contoso.local* in der Ressourcengruppe *MyResourceGroup* erstellt und für das virtuelle Netzwerk *MyAzureVnet* verfügbar gemacht (verlinkt), indem der Parameter „resolution-vnets“ verwendet wird. Verwenden Sie das Beispiel, um eine DNS-Zone zu erstellen, und ersetzen Sie dabei die Werte durch Ihre eigenen.

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.local --zone-type Private --resolution-vnets MyAzureVnet
```

Hinweis: Im obigen Beispiel gehört das virtuelle Netzwerk „MyAzureVnet“ zu derselben Ressourcengruppe und zu demselben Abonnement wie die private Zone. Wenn Sie ein virtuelles Netzwerk verlinken müssen, das einer anderen Ressourcengruppe oder einem anderen Abonnement angehört, müssen Sie die vollständige Azure Resource Manager-ID für den Parameter „--resolution-vnets“ angeben, anstatt nur den Namen des virtuellen Netzwerks. 

Verwenden Sie den Parameter *registration-vnets* anstelle von *resolution-vnets*, wenn Sie möchten, dass Azure in der Zone automatisch Hostnameneinträge erstellt.  Virtuelle Registrierungsnetzwerke sind automatisch für die Auflösung aktiviert.

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.local --zone-type Private --registration-vnets MyAzureVnet
```

## <a name="create-a-dns-record"></a>Erstellen eines DNS-Eintrags

Verwenden Sie zum Erstellen eines DNS-Eintrags den Befehl `az network dns record-set [record type] add-record`. Hilfe beispielsweise zu A-Einträgen finden Sie unter `azure network dns record-set A add-record --help`.

Im folgenden Beispiel wird ein Eintrag mit dem relativen Namen „ip1“ in der DNS-Zone „contoso.local“ unter der Ressourcengruppe „MyResourceGroup“ erstellt. Der vollqualifizierte Name des Ressourceneintragssatzes lautet „ip1.contoso.local“. Der Eintragstyp lautet „A“ mit der IP-Adresse „10.0.0.1“.

```azurecli
az network dns record-set a add-record -g MyResourceGroup -z contoso.local -n ip1 -a 10.0.0.1
```

Informationen zu anderen Eintragstypen, zu Eintragssätzen mit mehr als einem Eintrag, zu alternativen TTL-Werten und zum Ändern vorhandener Einträge finden Sie unter [Verwalten von DNS-Einträgen in Azure DNS mit der Azure CLI 2.0](dns-operations-recordsets-cli.md).

## <a name="view-records"></a>Anzeigen von Datensätzen

Listen Sie die DNS-Einträge in Ihrer Zone mit diesem Befehl auf:

```azurecli
az network dns record-set list -g MyResourceGroup -z contoso.com
```

## <a name="get-a-dns-private-zone"></a>Abrufen einer privaten DNS-Zone

Verwenden Sie `az network dns zone show`, um eine private DNS-Zone abzurufen. Entsprechende Hilfeinformationen finden Sie unter `az network dns zone show --help`.

Im folgenden Beispiel werden die DNS-Zone *contoso.local* und die zugehörigen Daten aus der Ressourcengruppe *MyResourceGroup* zurückgegeben. 

```azurecli
az network dns zone show --resource-group MyResourceGroup --name contoso.local
```

Das folgende Beispiel ist die Antwort.

```json
{
  "etag": "00000002-0000-0000-3d4d-64aa3689d201",
  "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/MyResourceGroup/providers/Microsoft.Network/dnszones/contoso.local",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.local",
  "nameServers": null,
  "numberOfRecordSets": 1,
  "registrationVirtualNetworks": [],
  "resolutionVirtualNetworks": [
    {
      "additionalProperties": {},
      "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/MyAzureVnet",
      "resourceGroup": "MyResourceGroup"
    }
  ]
  "resourceGroup": "MyResourceGroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones",
  "zoneType": "Private"
}
```

Beachten Sie, dass von `az network dns zone show` keine DNS-Einträge zurückgegeben werden. Verwenden Sie zum Auflisten von DNS-Einträgen `az network dns record-set list`.


## <a name="list-dns-zones"></a>Auflisten von DNS-Zonen

Verwenden Sie zum Aufzählen von DNS-Zonen `az network dns zone list`. Entsprechende Hilfeinformationen finden Sie unter `az network dns zone list --help`.

Wenn Sie die Ressourcengruppe angeben, werden nur die Zonen innerhalb der Ressourcengruppe aufgelistet:

```azurecli
az network dns zone list --resource-group MyResourceGroup
```

Wenn Sie die Ressourcengruppe weglassen, werden alle Zonen im Abonnement aufgelistet:

```azurecli
az network dns zone list 
```

## <a name="update-a-dns-zone"></a>Aktualisieren einer DNS-Zone

Änderungen an einer DNS-Zonenressource können mithilfe von `az network dns zone update`vorgenommen werden. Entsprechende Hilfeinformationen finden Sie unter `az network dns zone update --help`.

Dieser Befehl aktualisiert keinen der DNS-Ressourceneintragssätze in der Zone (siehe [Verwalten von DNS-Ressourceneintragssätzen](dns-operations-recordsets-cli.md)). Es wird nur verwendet, um Eigenschaften der Zonenressource selbst zu aktualisieren. Für private Zonen können Sie die virtuellen Registrierungs- bzw. Auflösungsnetzwerke aktualisieren, die mit einer Zone verlinkt sind. 

Im folgenden Beispiel wird veranschaulicht, wie Sie das mit einer privaten DNS-Zone verlinkte virtuelle Auflösungsnetzwerk aktualisieren. Das vorhandene verlinkte virtuelle Auflösungsnetzwerk wird durch das angegebene neue virtuelle Netzwerk ersetzt.

```azurecli
az network dns zone update --resource-group MyResourceGroup --name contoso.local --zone-type Private --resolution-vnets MyNewAzureVnet
```

## <a name="delete-a-dns-zone"></a>Löschen einer DNS-Zone

DNS-Zonen können mithilfe von `az network dns zone delete`gelöscht werden. Entsprechende Hilfeinformationen finden Sie unter `az network dns zone delete --help`.

> [!NOTE]
> Beim Löschen einer DNS-Zone werden auch alle DNS-Einträge in der Zone gelöscht. Dieser Vorgang kann nicht rückgängig gemacht werden. Wenn die DNS-Zone verwendet wird, tritt in Diensten, die die Zone verwenden, ein Fehler auf.
>
>Informationen dazu, wie Sie Zonen vor versehentlichem Löschen schützen, finden Sie unter [How to protect DNS zones and records](dns-protect-zones-recordsets.md) (Schützen von DNS-Zonen und -Einträgen).

Dieser Befehl fordert Sie zur Bestätigung auf. Der optionale Schalter `--yes` unterdrückt diese Aufforderung.

Das folgende Beispiel zeigt, wie die Zone *contoso.local* aus der Ressourcengruppe *MyResourceGroup* gelöscht wird.

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.local
```

## <a name="delete-all-resources"></a>Löschen aller Ressourcen
 
Führen Sie die folgenden Schritte aus, um alle in diesem Artikel erstellten Ressourcen zu löschen:

```azurecli
az group delete --name MyResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure DNS finden Sie unter [Azure DNS – Übersicht](dns-overview.md).

Weitere Informationen zum Verwalten von DNS-Zonen in Azure DNS finden Sie unter [Verwalten von DNS-Zonen in Azure DNS mit der Azure CLI 2.0](dns-operations-dnszones-cli.md).

Weitere Informationen zum Verwalten von DNS-Einträgen in Azure DNS finden Sie unter [Verwalten von DNS-Einträgen in Azure DNS mit der Azure CLI 2.0](dns-operations-recordsets-cli.md).
