---
title: "OMS-Azure-VM-Erweiterung für Linux | Microsoft-Dokumentation"
description: Stellen Sie den OMS-Agent mithilfe einer VM-Erweiterung auf einem virtuellen Linux-Computer bereit.
services: virtual-machines-linux
documentationcenter: 
author: danielsollondon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7bbf210-7d71-4a37-ba47-9c74567a9ea6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/15/2018
ms.author: danis
ms.openlocfilehash: fea3e992c70d286695691d837c746522f6a5ebb3
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2018
---
# <a name="oms-virtual-machine-extension-for-linux"></a>OMS-Azure-VM-Erweiterung für Linux

## <a name="overview"></a>Übersicht

Log Analytics bietet Überwachungs- und Warnungsfunktionen sowie Funktionen zum Beheben von Warnungen für cloudbasierte und lokale Ressourcen. Die OMS-Agent-VM-Erweiterung für Linux wird von Microsoft veröffentlicht und unterstützt. Die Erweiterung installiert den OMS-Agent auf virtuellen Azure-Computern und registriert virtuelle Computer in einem vorhandenen Log Analytics-Arbeitsbereich. Dieses Dokument enthält ausführliche Informationen zu den unterstützten Plattformen, Konfigurationen und Bereitstellungsoptionen für die OMS-VM-Erweiterung für Linux.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="operating-system"></a>Betriebssystem

Die OMS-Agent-Erweiterung kann für folgende Linux-Distributionen ausgeführt werden:

| Distribution | Version |
|---|---|
| CentOS Linux | 5, 6 und 7 |
| Oracle Linux | 5, 6 und 7 |
| Red Hat Enterprise Linux-Server | 5, 6 und 7 |
| Debian GNU/Linux | 6, 7 und 8 |
| Ubuntu | 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS |
| SUSE Linux Enterprise Server | 11 und 12 |

Die folgende Tabelle enthält eine Zuordnung der Version der OMS-VM-Erweiterung und des OMS-Agent-Pakets für jede Version. Ein Link zu den Anmerkungen zur jeweiligen OMS-Agent-Paketversion ist enthalten.  

| Version der OMS-VM-Erweiterung unter Linux | OMS-Agent-Paketversion | 
|--------------------------------|--------------------------|
| 1.4.59.1 | [1.4.3-174](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.3-174)|
| 1.4.58.7 | [14.2-125](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.2-125)|
| 1.4.56.5 | [1.4.2-124](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.2-124)|
| 1.4.55.4 | [1.4.1-123](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.1-123)|
| 1.4.45.3 | [1.4.1-45](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.1-45)|

### <a name="azure-security-center"></a>Azure Security Center

Azure Security Center stellt den OMS-Agent automatisch bereit und verbindet ihn mit einem Standardarbeitsbereich von Log Analytics, der von ASC in Ihrem Azure-Abonnement erstellt wird. Wenn Sie Azure Security Center verwenden, führen Sie nicht die Schritte in diesem Dokument aus. Andernfalls überschreiben Sie den konfigurierten Arbeitsbereich und unterbrechen die Verbindung mit Azure Security Center.

### <a name="internet-connectivity"></a>Internetkonnektivität

Um die OMS-Agent-Erweiterung für Linux verwenden zu können, muss der virtuelle Zielcomputer mit dem Internet verbunden sein. 

## <a name="extension-schema"></a>Erweiterungsschema

Der folgende JSON-Code zeigt das Schema für die OMS Agent-Erweiterung. Für die Erweiterung werden die Arbeitsbereichs-ID und der Arbeitsbereichsschlüssel aus dem Log Analytics-Zielarbeitsbereich benötigt. Diese Werte sind [im Log Analytics-Arbeitsbereich](../../log-analytics/log-analytics-quick-collect-linux-computer.md#obtain-workspace-id-and-key) im Azure-Portal zu finden. Da der Arbeitsbereichsschlüssel als vertrauliche Information behandelt werden muss, sollte er in einer geschützten Einstellungskonfiguration gespeichert werden. Die geschützten Einstellungsdaten der Azure-VM-Erweiterung werden verschlüsselt und nur auf dem virtuellen Zielcomputer entschlüsselt. Bitte beachten Sie, dass bei **workspaceId** und **workspaceKey** die Groß-/Kleinschreibung beachtet wird.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

### <a name="property-values"></a>Eigenschaftswerte

| NAME | Wert/Beispiel |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Herausgeber | Microsoft.EnterpriseCloud.Monitoring |
| type | OmsAgentForLinux |
| typeHandlerVersion | 1.4 |
| workspaceId (z.B.) | 6f680a37-00c6-41c7-a93f-1437e3462574 |
| workspaceKey (z.B.) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ== |


## <a name="template-deployment"></a>Bereitstellung von Vorlagen

Azure-VM-Erweiterungen können mithilfe von Azure Resource Manager-Vorlagen bereitgestellt werden. Vorlagen sind ideal, wenn Sie virtuelle Computer bereitstellen, die nach der Bereitstellung konfiguriert werden müssen (beispielsweise, um sie in Log Analytics zu integrieren). Eine Resource Manager-Beispielvorlage mit der OMS-Agent-VM-Erweiterung finden Sie im [Azure-Schnellstartkatalog](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm). 

Die JSON-Konfiguration für eine VM-Erweiterung kann innerhalb der VM-Ressource geschachtelt oder im Stamm bzw. auf der obersten Ebene einer Resource Manager-JSON-Vorlage platziert werden. Die Platzierung der JSON-Konfiguration wirkt sich auf den Wert von Name und Typ der Ressource aus. Weitere Informationen finden Sie unter [Set name and type for child resources](../../azure-resource-manager/resource-manager-templates-resources.md#child-resources) (Festlegen von Name und Typ für untergeordnete Ressourcen). 

Im folgenden Beispiel wird davon ausgegangen, dass die VM-Erweiterung in der VM-Ressource geschachtelt ist. Beim Schachteln der Ressource für die Erweiterung wird der JSON-Code im `"resources": []`-Objekt des virtuellen Computers platziert.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

Beim Platzieren des JSON-Codes für die Erweiterung im Stamm der Vorlage enthält der Name der Ressource einen Verweis auf die übergeordnete VM, und der Typ spiegelt die geschachtelte Konfiguration wider.  

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "<parentVmResource>/OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

## <a name="azure-cli-deployment"></a>Bereitstellung mithilfe der Azure-Befehlszeilenschnittstelle

Sie können die OMS-Agent-VM-Erweiterung mithilfe der Azure-Befehlszeilenschnittstelle auf einem vorhandenen virtuellen Computer bereitstellen. Ersetzen Sie *workspaceId* und *workspaceKey* durch die ID und den Schlüssel aus Ihrem Log Analytics-Arbeitsbereich. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.4 --protected-settings '{"workspaceKey": "omskey"}' \
  --settings '{"workspaceId": "omsid"}'
```

## <a name="troubleshoot-and-support"></a>Problembehandlung und Support

### <a name="troubleshoot"></a>Problembehandlung

Daten zum Status von Erweiterungsbereitstellungen können über das Azure-Portal und mithilfe der Azure-Befehlszeilenschnittstelle abgerufen werden. Führen Sie über die Azure-Befehlszeilenschnittstelle den folgenden Befehl aus, um den Bereitstellungsstatus von Erweiterungen für einen bestimmten virtuellen Computer anzuzeigen.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Die Ausgabe der Erweiterungsausführung wird in der folgenden Datei protokolliert:

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a>Fehlercodes und ihre Bedeutung

| Fehlercode | Bedeutung | Mögliche Aktion |
| :---: | --- | --- |
| 10 | VM ist bereits mit einem Log Analytics-Arbeitsbereich verbunden | Zum Verbinden der VM mit dem im Erweiterungsschema angegebenen Arbeitsbereich legen Sie „stopOnMultipleConnections“ in den öffentlichen Einstellungen auf FALSE fest, oder entfernen Sie diese Eigenschaft. Diese VM wird für jeden Arbeitsbereich, mit dem Sie verbunden ist, einmal in Rechnung gestellt. |
| 11 | Ungültige Konfiguration der Erweiterung bereitgestellt | Folgen Sie den vorherigen Beispielen, um alle für die Bereitstellung erforderlichen Eigenschaftswerte festzulegen. |
| 12 | Der dpkg-Paket-Manager ist gesperrt. | Stellen Sie sicher, dass alle dpkg-Updatevorgänge auf dem Computer abgeschlossen sind, und versuchen Sie es erneut. |
| 20 | Aktivierung zu früh aufgerufen | [Aktualisieren Sie den Azure Linux-Agent](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent) auf die neueste verfügbare Version. |
| 51 | Diese Erweiterung wird vom Betriebssystem der VM nicht unterstützt. | |
| 55 | Verbindung mit dem Microsoft Operations Management Suite-Dienst nicht möglich | Stellen Sie sicher, dass das System entweder Internetzugriff hat oder dass ein gültiger HTTP-Proxy bereitgestellt wurde. Überprüfen Sie darüber hinaus die Richtigkeit der Arbeitsbereichs-ID. |

Weitere Informationen zur Problembehandlung finden Sie im [Handbuch zur Problembehandlung für den OMS-Agent für Linux](../../log-analytics/log-analytics-azure-vmext-troubleshoot.md).

### <a name="support"></a>Support

Sollten Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie sich über das [MSDN Azure-Forum oder über das Stack Overflow-Forum](https://azure.microsoft.com/en-us/support/forums/) mit Azure-Experten in Verbindung setzen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/en-us/support/options/) auf, und wählen Sie „Support erhalten“ aus. Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](https://azure.microsoft.com/en-us/support/faq/).
