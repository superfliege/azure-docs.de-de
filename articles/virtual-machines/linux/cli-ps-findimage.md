---
title: "Auswählen von Linux-VM-Images mit der Azure CLI | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über die Verwendung der Azure CLI, um den Herausgeber, das Angebot, die SKU und die Version für Marketplace-VM-Images zu ermitteln."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 7a858e38-4f17-4e8e-a28a-c7f801101721
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/28/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c65ebbc8a61c13b96364dadde45bd4bca828e337
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="how-to-find-linux-vm-images-in-the-azure-marketplace-with-the-azure-cli"></a>Vorgehensweise zum Suchen nach Linux-VM-Images im Azure Marketplace mit der Azure CLI
Dieses Thema beschreibt, wie Sie mit Azure CLI 2.0 nach VM-Images im Azure Marketplace suchen. Verwenden Sie diese Informationen, um ein Marketplace-Image anzugeben, wenn Sie einen virtuellen Computer programmgesteuert mit der Befehlszeilenschnittstelle, mit Resource Manager-Vorlagen oder anderen Tools erstellen.

Stellen Sie sicher, dass Sie die aktuelle Version von [Azure CLI 2.0](/cli/azure/install-az-cli2) installiert haben und bei einem Azure-Konto (`az login`) angemeldet sind.

[!INCLUDE [virtual-machines-common-image-terms](../../../includes/virtual-machines-common-image-terms.md)]

## <a name="list-popular-images"></a>Liste von beliebten Images

Führen Sie den Befehl [az vm image list](/cli/azure/vm/image#az_vm_image_list) ohne die Option `--all` aus, um eine Liste beliebter VM-Images im Azure Marketplace anzuzeigen. Führen Sie beispielsweise den folgenden Befehl aus, um eine zwischengespeicherte Liste beliebter Images im Tabellenformat anzuzeigen:

```azurecli
az vm image list --output table
```

Die Ausgabe enthält den Image-URN (Wert in der Spalte *Urn*). Beim Erstellen eines virtuellen Computers mit einem der beliebten Marketplace-Images können Sie alternativ das *UrnAlias*-Element angeben, eine Kurzform wie etwa *UbuntuLTS*.

```
You are viewing an offline list of images, use --all to retrieve an up-to-date list
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
...
```

## <a name="find-specific-images"></a>Suchen nach bestimmten Images

Wenn Sie ein bestimmtes VM-Image im Marketplace suchen möchten, verwenden Sie den Befehl `az vm image list` mit der Option `--all`. Die Ausführung dieser Befehlsversion dauert einige Zeit und kann eine umfangreiche Ausgabe zurückgeben. Daher filtern Sie in der Regel die Liste nach `--publisher` oder einem anderen Parameter. 

Mit dem folgenden Code werden beispielsweise alle Debian-Angebote angezeigt (zur Erinnerung: Ohne den Switch `--all` wird nur der lokale Cache von allgemeinen Images durchsucht):

```azurecli
az vm image list --offer Debian --all --output table 

```

Hier sehen Sie einen Teil der Ausgabe: 
```
Offer    Publisher    Sku                Urn                                              Version
-------  -----------  -----------------  -----------------------------------------------  --------------
Debian   credativ     7                  credativ:Debian:7:7.0.201602010                  7.0.201602010
Debian   credativ     7                  credativ:Debian:7:7.0.201603020                  7.0.201603020
Debian   credativ     7                  credativ:Debian:7:7.0.201604050                  7.0.201604050
Debian   credativ     7                  credativ:Debian:7:7.0.201604200                  7.0.201604200
Debian   credativ     7                  credativ:Debian:7:7.0.201606280                  7.0.201606280
Debian   credativ     7                  credativ:Debian:7:7.0.201609120                  7.0.201609120
Debian   credativ     7                  credativ:Debian:7:7.0.201611020                  7.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201602010                  8.0.201602010
Debian   credativ     8                  credativ:Debian:8:8.0.201603020                  8.0.201603020
Debian   credativ     8                  credativ:Debian:8:8.0.201604050                  8.0.201604050
Debian   credativ     8                  credativ:Debian:8:8.0.201604200                  8.0.201604200
Debian   credativ     8                  credativ:Debian:8:8.0.201606280                  8.0.201606280
Debian   credativ     8                  credativ:Debian:8:8.0.201609120                  8.0.201609120
Debian   credativ     8                  credativ:Debian:8:8.0.201611020                  8.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201701180                  8.0.201701180
Debian   credativ     8                  credativ:Debian:8:8.0.201703150                  8.0.201703150
Debian   credativ     8                  credativ:Debian:8:8.0.201704110                  8.0.201704110
Debian   credativ     8                  credativ:Debian:8:8.0.201704180                  8.0.201704180
Debian   credativ     8                  credativ:Debian:8:8.0.201706190                  8.0.201706190
Debian   credativ     8                  credativ:Debian:8:8.0.201706210                  8.0.201706210
Debian   credativ     8                  credativ:Debian:8:8.0.201708040                  8.0.201708040
...
```

Wenden Sie ähnliche Filter mit den Optionen `--location`, `--publisher` und `--sku` an. Sie können sogar Teilübereinstimmungen für einen Filter durchführen, z.B. bei der Suche nach `--offer Deb`, um alle Debian-Images zu finden.

Wenn Sie keinen bestimmten Standort mit der Option `--location` angeben, werden die Werte für den Standardstandort zurückgegeben. (Legen Sie einen anderen Standardstandort fest, indem Sie `az configure --defaults location=<location>` ausführen.)

Beispielsweise listet der folgende Befehl alle Debian 8-SKUs am Standort „Europa, Westen“ auf:

```azurecli
az vm image list --location westeurope --offer Deb --publisher credativ --sku 8 --all --output table
```

Hier sehen Sie einen Teil der Ausgabe:

```
Offer    Publisher    Sku                Urn                                              Version
-------  -----------  -----------------  -----------------------------------------------  -------------
Debian   credativ     8                  credativ:Debian:8:8.0.201602010                  8.0.201602010
Debian   credativ     8                  credativ:Debian:8:8.0.201603020                  8.0.201603020
Debian   credativ     8                  credativ:Debian:8:8.0.201604050                  8.0.201604050
Debian   credativ     8                  credativ:Debian:8:8.0.201604200                  8.0.201604200
Debian   credativ     8                  credativ:Debian:8:8.0.201606280                  8.0.201606280
Debian   credativ     8                  credativ:Debian:8:8.0.201609120                  8.0.201609120
Debian   credativ     8                  credativ:Debian:8:8.0.201611020                  8.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201701180                  8.0.201701180
Debian   credativ     8                  credativ:Debian:8:8.0.201703150                  8.0.201703150
Debian   credativ     8                  credativ:Debian:8:8.0.201704110                  8.0.201704110
Debian   credativ     8                  credativ:Debian:8:8.0.201704180                  8.0.201704180
Debian   credativ     8                  credativ:Debian:8:8.0.201706190                  8.0.201706190
Debian   credativ     8                  credativ:Debian:8:8.0.201706210                  8.0.201706210
...
```

## <a name="navigate-the-images"></a>Navigieren zu den Images 
Eine weitere Möglichkeit für die Suche nach einem Image an einem Standort besteht darin, die Befehle [az vm image list-publishers](/cli/azure/vm/image#az_vm_image_list_publishers), [az vm image list-offers](/cli/azure/vm/image#az_vm_image_list_offers) und [az vm image list-skus](/cli/azure/vm/image#az_vm_image_list_skus) nacheinander auszuführen. Mit diesen Befehlen ermitteln Sie folgende Werte:

1. Auflistung der Herausgeber von Images
2. Auflistung der Angebote eines bestimmten Anbieters
3. Auflistung der SKUs eines bestimmten Angebots

Anschließend können Sie für eine ausgewählte SKU eine bereitzustellende Version auswählen.

Beispielsweise listet der folgende Befehl die Herausgeber von Images für den Standort „USA, Westen“ auf:

```azurecli
az vm image list-publishers --location westus --output table
```

Hier sehen Sie einen Teil der Ausgabe:

```
Location    Name
----------  ----------------------------------------------------
westus      1e
westus      4psa
westus      7isolutions
westus      a10networks
westus      abiquo
westus      accellion
westus      Acronis
westus      Acronis.Backup
westus      actian_matrix
westus      actifio
westus      activeeon
westus      adatao
...
```
Verwenden Sie diese Informationen, um nach Angeboten eines bestimmten Herausgebers zu suchen. Wenn beispielsweise *Canonical* als Herausgeber von Images für den Standort „USA, Westen“ festgelegt ist, suchen Sie nach dessen Angeboten, indem Sie `azure vm image list-offers` ausführen. Übergeben Sie den Standort und den Herausgeber, wie im folgenden Beispiel gezeigt wird:

```azurecli
az vm image list-offers --location westus --publisher Canonical --output table
```

Ausgabe:

```
Location    Name
----------  -------------------------
westus      Ubuntu15.04Snappy
westus      Ubuntu15.04SnappyDocker
westus      UbunturollingSnappy
westus      UbuntuServer
westus      Ubuntu_Core
westus      Ubuntu_Snappy_Core
westus      Ubuntu_Snappy_Core_Docker
```
Sie sehen, dass in der Region „USA, Westen“ Canonical das *UbuntuServer*-Angebot in Azure herausgibt. Aber welche SKUs? Um diese Werte zu erhalten, führen Sie `azure vm image list-skus` aus, und legen Sie den Standort, Herausgeber und das von Ihnen entdeckte Angebot fest:

```azurecli
az vm image list-skus --location westus --publisher Canonical --offer UbuntuServer --output table
```

Ausgabe:

```
Location    Name
----------  -----------------
westus      12.04.3-LTS
westus      12.04.4-LTS
westus      12.04.5-DAILY-LTS
westus      12.04.5-LTS
westus      12.10
westus      14.04.0-LTS
westus      14.04.1-LTS
westus      14.04.2-LTS
westus      14.04.3-LTS
westus      14.04.4-LTS
westus      14.04.5-DAILY-LTS
westus      14.04.5-LTS
westus      16.04-beta
westus      16.04-DAILY-LTS
westus      16.04-LTS
westus      16.04.0-LTS
westus      16.10
westus      16.10-DAILY
westus      17.04
westus      17.04-DAILY
westus      17.10-DAILY
```

Verwenden Sie abschließend den Befehl `az vm image list`, um nach einer bestimmten gewünschten Version der SKU zu suchen, z.B. *16.04-LTS*:

```azurecli
az vm image list --location westus --publisher Canonical --offer UbuntuServer --sku 16.04-LTS --all --output table
```

Ausgabe:

```
Offer         Publisher    Sku        Urn                                               Version
------------  -----------  ---------  ------------------------------------------------  ---------------
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201611220  16.04.201611220
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201611300  16.04.201611300
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612050  16.04.201612050
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612140  16.04.201612140
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612210  16.04.201612210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201701130  16.04.201701130
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702020  16.04.201702020
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702200  16.04.201702200
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702210  16.04.201702210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702240  16.04.201702240
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703020  16.04.201703020
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703030  16.04.201703030
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703070  16.04.201703070
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703270  16.04.201703270
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703280  16.04.201703280
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703300  16.04.201703300
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201705080  16.04.201705080
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201705160  16.04.201705160
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201706100  16.04.201706100
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201706191  16.04.201706191
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201707210  16.04.201707210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201707270  16.04.201707270
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708030  16.04.201708030
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708110  16.04.201708110
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708151  16.04.201708151
```

Jetzt können Sie genau das Image auswählen, das Sie verwenden möchten. Notieren Sie sich hierzu den Wert des URN. Übergeben Sie diesen Wert mit dem Parameter `--image`, wenn Sie einen virtuellen Computer mit dem Befehl [az vm create](/cli/azure/vm#az_vm_create) erstellen. Beachten Sie, dass Sie optional die Versionsnummer im URN durch „latest“ ersetzen können. Diese Version ist immer die neueste Version des Images. 

Wenn Sie einen virtuellen Computer mithilfe einer Resource Manager-Vorlage bereitstellen, legen Sie die Imageparameter einzeln in den `imageReference`-Eigenschaften fest. Informationen finden Sie in der [Vorlagenreferenz](/azure/templates/microsoft.compute/virtualmachines).

[!INCLUDE [virtual-machines-common-marketplace-plan](../../../includes/virtual-machines-common-marketplace-plan.md)]

### <a name="view-plan-properties"></a>Anzeigen von Planeigenschaften
Um die Kaufplaninformationen eines Images anzuzeigen, führen Sie den Befehl [az vm image show](/cli/azure/image#az_image_show) aus. Wenn die `plan`-Eigenschaft in der Ausgabe nicht `null` ist, müssen Sie vor der programmgesteuerten Bereitstellung Bedingungen des Images akzeptieren.

Das Image „Canonical Ubuntu Server 16.04 LTS“ verfügt beispielsweise nicht über zusätzliche Bedingungen, da `null` für `plan` angegeben ist:

```azurecli
az vm image show --location westus --publisher Canonical --offer UbuntuServer --sku 16.04-LTS --version 16.04.201801260
```

Ausgabe:

```
{
  "dataDiskImages": [],
  "id": "/Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/Canonical/ArtifactTypes/VMImage/Offers/UbuntuServer/Skus/16.04-LTS/Versions/16.04.201801260",
  "location": "westus",
  "name": "16.04.201801260",
  "osDiskImage": {
    "operatingSystem": "Linux"
  },
  "plan": null,
  "tags": null
}
```

Wenn Sie einen ähnlichen Befehl für das von Bitnami zertifizierte RabbitMQ-Image ausführen, werden die folgenden `plan`-Eigenschaften angezeigt: `name`, `product` und `publisher`. (Einige Images besitzen auch eine `promotion code`-Eigenschaft.) Sehen Sie sich zum Bereitstellen dieses Images die folgenden Abschnitte an, um die Bedingungen zu akzeptieren und die programmgesteuerte Bereitstellung zu ermöglichen.

```azurecli
az vm image show --location westus --publisher bitnami --offer rabbitmq --sku rabbitmq --version 3.7.1801130730
```
Ausgabe:

```
{
  "dataDiskImages": [],
  "id": "/Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/bitnami/ArtifactTypes/VMImage/Offers/rabbitmq/Skus/rabbitmq/Versions/3.7.1801130730",
  "location": "westus",
  "name": "3.7.1801130730",
  "osDiskImage": {
    "operatingSystem": "Linux"
  },
  "plan": {
    "name": "rabbitmq",
    "product": "rabbitmq",
    "publisher": "bitnami"
  },
  "tags": null
}
```

### <a name="accept-the-terms"></a>Akzeptieren der Bedingungen
Verwenden Sie zum Anzeigen und Akzeptieren der Lizenzbedingungen den Befehl [az vm image accept-terms](/cli/azure/vm/image?#az_vm_image_accept_terms). Wenn Sie die Bedingungen akzeptieren, ermöglichen Sie die programmgesteuerte Bereitstellung in Ihrem Abonnement. Sie müssen für das Image die Bedingungen nur einmal pro Abonnement akzeptieren. Beispiel: 

```azurecli
az vm image accept-terms --urn bitnami:rabbitmq:rabbitmq:latest
``` 

Die Ausgabe enthält ein `licenseTextLink`-Element für die Lizenzbedingungen und gibt an, dass für `accepted` der Wert `true` angegeben ist:

```
{
  "accepted": true,
  "additionalProperties": {},
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/Microsoft.MarketplaceOrdering/offertypes/bitnami/offers/rabbitmq/plans/rabbitmq",
  "licenseTextLink": "https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_BITNAMI%253a24RABBITMQ%253a24RABBITMQ%253a24IGRT7HHPIFOBV3IQYJHEN2O2FGUVXXZ3WUYIMEIVF3KCUNJ7GTVXNNM23I567GBMNDWRFOY4WXJPN5PUYXNKB2QLAKCHP4IE5GO3B2I.txt",
  "name": "rabbitmq",
  "plan": "rabbitmq",
  "privacyPolicyLink": "https://bitnami.com/privacy",
  "product": "rabbitmq",
  "publisher": "bitnami",
  "retrieveDatetime": "2018-02-22T04:06:28.7641907Z",
  "signature": "WVIEA3LAZIK7ZL2YRV5JYQXONPV76NQJW3FKMKDZYCRGXZYVDGX6BVY45JO3BXVMNA2COBOEYG2NO76ONORU7ITTRHGZDYNJNKLNLWI",
  "type": "Microsoft.MarketplaceOrdering/offertypes"
}
```

### <a name="deploy-using-purchase-plan-parameters"></a>Bereitstellen mithilfe von Kaufplanparametern
Nach dem Akzeptieren der Bedingungen für das Image können Sie einen virtuellen Computer im Abonnement bereitstellen. Geben Sie zum Bereitstellen des Images mithilfe des Befehls `az vm create` Parameter für den Kaufplan sowie einen URN für das Image an. Zum Bereitstellen eines virtuellen Computers mit dem von Bitnami zertifizierten RabbitMQ-Image geben Sie beispielsweise Folgendes an:

```azurecli
az group create --name myResourceGroupVM --location westus

az vm create --resource-group myResourceGroupVM --name myVM --image bitnami:rabbitmq:rabbitmq:latest --plan-name rabbitmq --plan-product rabbitmq --plan-publisher bitnami

```



## <a name="next-steps"></a>Nächste Schritte
Um schnell mithilfe der Imageinformationen einen virtuellen Computer zu erstellen, lesen Sie die Informationen unter [Erstellen und Verwalten virtueller Linux-Computer mit der Azure-Befehlszeilenschnittstelle](tutorial-manage-vm.md).