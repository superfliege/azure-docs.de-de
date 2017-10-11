---
title: "Mehrere IP-Adressen für virtuelle Computer in Azure - Vorlage | Microsoft Docs"
description: "Erfahren Sie, wie eine virtuelle Maschine über ein Azure-Ressourcen-Manager-Vorlage mehrere IP-Adressen zuweisen."
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/08/2016
ms.author: jdial
ms.openlocfilehash: d4b189fb23dda1167c4f6b17b618c718d32dd98f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-an-azure-resource-manager-template"></a>Weisen Sie mehrere IP-Adressen für virtuelle Computer mit einer Azure-Ressourcen-Manager-Vorlage

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

In diesem Artikel wird erläutert, wie zum Erstellen eines virtuellen Computers (VM) über das Azure-Ressourcen-Manager-Bereitstellungsmodell mit einer Ressourcen-Manager-Vorlage. Auf der gleichen NIC können mehrere öffentliche und private IP-Adressen zugewiesen werden, bei der Bereitstellung einer virtuellen Maschine über dem klassischen Bereitstellungsmodell. Weitere Informationen zu Azure-Bereitstellungsmodellen finden Sie die [verstehen Bereitstellungsmodelle](../resource-manager-deployment-model.md) Artikel.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name="template-description"></a>Beschreibung der Vorlage

Bereitstellen einer Vorlage ermöglicht es Ihnen, schnell und konsistent mit anderen Konfigurationswerten Azure-Ressourcen erstellen. Lesen der [Ressourcen-Manager-Vorlage Exemplarische Vorgehensweise](../azure-resource-manager/resource-manager-template-walkthrough.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Artikel, wenn Sie nicht mit Vorlagen für Azure Resource Manager vertraut sind. Die [bereitstellen ein virtuellen Computers mit mehreren IP-Adressen](https://azure.microsoft.com/resources/templates/101-vm-multiple-ipconfig) Vorlage wird in diesem Artikel verwendet.

<a name="resources"></a>Bereitstellen der Vorlage erstellt die folgenden Ressourcen:

|Ressource|Name|Beschreibung|
|---|---|---|
|Die Netzwerkschnittstelle|*myNic1*|Die drei IP-Konfigurationen, die im Szenario Abschnitt dieses Artikels beschriebenen sind erstellt und auf diese NIC zugeordnet|
|Öffentliche IP-Adressressource|erstellt von 2: *MyPublicIP* und *myPublicIP2*|Diese Ressourcen sind statische öffentliche IP-Adressen und zugewiesen sind die *"ipconfig"-1-* und *"ipconfig"-2-* IP-Konfigurationen, die im Szenario beschrieben.|
|VM|*myVM1*|Eine standardmäßige DS3 VM.|
|Virtuelles Netzwerk|*myVNet1*|Ein virtuelles Netzwerk mit einem Subnetz mit dem Namen *MySubnet*.|
|Speicherkonto|Für die Bereitstellung eindeutig|Ein Speicherkonto.|

<a name="parameters"></a>Wenn Sie die Vorlage bereitstellen zu können, müssen Sie Werte für die folgenden Parameter angeben:

|Name|Beschreibung|
|---|---|
|adminUsername|Benutzername des Administrators. Der Benutzername muss einhalten, [Benutzername für Azure-Anforderungen](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json).|
|AdminPassword|Das Kennwort einhalten muss, Administratorkennwort [Azure kennwortanforderungen](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
|dnsLabelPrefix|DNS-Name PublicIPAddressName1. Der DNS-Namen verweisen auf eine öffentliche IP-Adressen mit dem virtuellen Computer zugewiesen. Der Name muss innerhalb der Azure-Region (Standort) eindeutig sein, die Erstellung des virtuellen Computers in.|
|dnsLabelPrefix1|DNS-Name PublicIPAddressName2. Der DNS-Namen verweisen auf eine öffentliche IP-Adressen mit dem virtuellen Computer zugewiesen. Der Name muss innerhalb der Azure-Region (Standort) eindeutig sein, die Erstellung des virtuellen Computers in.|
|OSVersion|Die Windows-/Linux-Version für den virtuellen Computer. Das Betriebssystem ist ein vollständig gepatchten Image der angegebenen Windows-/Linux-Version ausgewählt.|
|imagePublisher|Der Windows-/Linux-Image-Herausgeber für den ausgewählten virtuellen Computer.|
|imageOffer|Die Windows-/Linux-Image für den ausgewählten virtuellen Computer.|

Jede der Ressourcen, die von der Vorlage bereitgestellt wird mit mehrere Standardeinstellungen konfiguriert. Sie können diese Einstellungen über eine der folgenden Methoden anzeigen:

- **Zeigen Sie die Vorlage auf GitHub:** , wenn Sie mit Vorlagen vertraut sind, sehen Sie die Einstellungen in der [Vorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json).
- **Zeigen Sie die Einstellungen nach der Bereitstellung:** , wenn Sie nicht mit Vorlagen vertraut sind, können die Vorlage mithilfe der Schritte in den folgenden Abschnitten bereitstellen und zeigen Sie dann die Einstellungen nach der Bereitstellung.

Sie können Azure-Portal, PowerShell oder der Azure-Befehlszeilenschnittstelle (CLI) verwenden, um die Vorlage bereitzustellen. Alle Methoden liefern das gleiche Ergebnis. Um die Vorlage bereitzustellen, führen Sie die Schritte in einem der folgenden Abschnitte:

## <a name="deploy-using-the-azure-portal"></a>Die Bereitstellung über das Azure-portal

Um die Vorlage mit der Azure-Portal bereitzustellen, müssen führen Sie die folgenden Schritte aus:

1. Ändern Sie die Vorlage aus, bei Bedarf. Die Vorlage bereitstellt, die Ressourcen und Einstellungen aufgeführt, der [Ressourcen](#resources) Abschnitt dieses Artikels. Weitere Informationen zu Vorlagen und wie diese erstellen, lesen die [Authoring Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-network%2ftoc.json)Artikel.
2. Bereitstellen Sie die Vorlage mit einer der folgenden Methoden:
    - **Wählen Sie die Vorlage im Portal:** die Schritte in der [Bereitstellen von Ressourcen aus der benutzerdefinierten Vorlage](../azure-resource-manager/resource-group-template-deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-resources-from-custom-template) Artikel. Wählen Sie die bereits vorhandene Vorlage, die mit dem Namen *101-Vm-Multiple-Ipconfig*.
    - **Direkt:** klicken Sie auf die folgende Schaltfläche, um die Vorlage direkt in das Portal zu öffnen:<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-vm-multiple-ipconfig%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

Unabhängig von der Methode Sie sich entscheiden, müssen Sie Werte für die [Parameter](#parameters) zuvor in diesem Artikel aufgeführt. Nachdem der virtuelle Computer bereitgestellt wird, Herstellen einer Verbindung mit dem virtuellen Computer, und fügen Sie der privaten IP-Adressen für das Betriebssystem, die Sie bereitgestellt haben, mithilfe der Schritte in der [Hinzufügen von IP-Adressen zu einem VM-Betriebssystem](#os-config) Abschnitt dieses Artikels. Fügen Sie die öffentlichen IP-Adressen nicht für das Betriebssystem an.

## <a name="deploy-using-powershell"></a>Mithilfe von PowerShell bereitstellen

Um die Vorlage mithilfe von PowerShell bereitstellen zu können, müssen führen Sie die folgenden Schritte aus:

1. Bereitstellen die Vorlage mithilfe der Schritte in der [Bereitstellen einer Vorlage mit PowerShell](../azure-resource-manager/resource-group-template-deploy-cli.md) Artikel. Dieser Artikel beschreibt mehrere Optionen für die Bereitstellung einer Vorlage. Falls gewünscht, um die Bereitstellung über die `-TemplateUri parameter`, ist der URI für diese Vorlage *https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json*. Falls gewünscht, um die Bereitstellung über die `-TemplateFile` Parameter, kopieren Sie den Inhalt von der [Vorlagendatei](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json) über GitHub in eine neue Datei auf Ihrem Computer. Ändern Sie den Vorlageninhalt bei Bedarf. Die Vorlage bereitstellt, die Ressourcen und Einstellungen aufgeführt, der [Ressourcen](#resources) Abschnitt dieses Artikels. Weitere Informationen zu Vorlagen und wie diese erstellen, lesen die [Authoring Azure Resource Manager-Vorlagen ](../azure-resource-manager/resource-group-authoring-templates.md)Artikel.

    Unabhängig von der Option, die Sie auswählen, um die Vorlage mit bereitzustellen, müssen Sie angeben, Werte für die Parameter aufgeführt, die der [Parameter](#parameters) Abschnitt dieses Artikels. Falls gewünscht, Parameter, die mithilfe einer Parameterdatei bereitstellen, kopieren Sie den Inhalt von der [Parameterdatei](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.parameters.json) über GitHub in eine neue Datei auf Ihrem Computer. Ändern Sie die Werte in der Datei. Verwenden Sie die Datei, die Sie erstellt haben, als der Wert für die `-TemplateParameterFile` Parameter.

    Um gültige Werte für die "osversion" ImagePublisher und ImageOffer Parameter zu ermitteln, führen Sie die Schritte in der [navigieren und die Option Windows-VM-Images Artikel](../virtual-machines/windows/cli-ps-findimage.md) Artikel.

    >[!TIP]
    >Wenn Sie nicht sicher sind, ob eine Dnslabelprefix verfügbar ist, geben Sie die `Test-AzureRmDnsAvailability -DomainNameLabel <name-you-want-to-use> -Location <location>` Befehl aus, um herauszufinden. Wenn es verfügbar ist, wird der Befehl zurück `True`.

2. Nachdem der virtuelle Computer bereitgestellt wird, Herstellen einer Verbindung mit dem virtuellen Computer, und fügen Sie der privaten IP-Adressen für das Betriebssystem, die Sie bereitgestellt haben, mithilfe der Schritte in der [Hinzufügen von IP-Adressen zu einem VM-Betriebssystem](#os-config) Abschnitt dieses Artikels. Fügen Sie die öffentlichen IP-Adressen nicht für das Betriebssystem an.

## <a name="deploy-using-the-azure-cli"></a>Bereitstellen Sie mithilfe der Azure-CLI

Um die Vorlage mit der Azure-CLI-1.0 bereitzustellen, müssen führen Sie die folgenden Schritte aus:

1. Bereitstellen die Vorlage mithilfe der Schritte in der [Bereitstellen einer Vorlage mit der Azure-CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) Artikel. Dieser Artikel beschreibt mehrere Optionen für die Bereitstellung der Vorlage. Falls gewünscht, um die Bereitstellung über die `--template-uri` (-f), ist der URI für diese Vorlage *https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json*. Falls gewünscht, um die Bereitstellung über die `--template-file` (-f) Parameter, kopieren Sie den Inhalt von der [Vorlagendatei](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json) über GitHub in eine neue Datei auf Ihrem Computer. Ändern Sie den Vorlageninhalt bei Bedarf. Die Vorlage bereitstellt, die Ressourcen und Einstellungen aufgeführt, der [Ressourcen](#resources) Abschnitt dieses Artikels. Weitere Informationen zu Vorlagen und wie diese erstellen, lesen die [Authoring Azure Resource Manager-Vorlagen ](../azure-resource-manager/resource-group-authoring-templates.md)Artikel.

    Unabhängig von der Option, die Sie auswählen, um die Vorlage mit bereitzustellen, müssen Sie angeben, Werte für die Parameter aufgeführt, die der [Parameter](#parameters) Abschnitt dieses Artikels. Falls gewünscht, Parameter, die mithilfe einer Parameterdatei bereitstellen, kopieren Sie den Inhalt von der [Parameterdatei](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.parameters.json) über GitHub in eine neue Datei auf Ihrem Computer. Ändern Sie die Werte in der Datei. Verwenden Sie die Datei, die Sie erstellt haben, als der Wert für die `--parameters-file` (– e) Parameter.

    Um gültige Werte für die "osversion" ImagePublisher und ImageOffer Parameter zu ermitteln, führen Sie die Schritte in der [navigieren und die Option Windows-VM-Images Artikel](../virtual-machines/windows/cli-ps-findimage.md) Artikel.

2. Nachdem der virtuelle Computer bereitgestellt wird, Herstellen einer Verbindung mit dem virtuellen Computer, und fügen Sie der privaten IP-Adressen für das Betriebssystem, die Sie bereitgestellt haben, mithilfe der Schritte in der [Hinzufügen von IP-Adressen zu einem VM-Betriebssystem](#os-config) Abschnitt dieses Artikels. Fügen Sie die öffentlichen IP-Adressen nicht für das Betriebssystem an.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
