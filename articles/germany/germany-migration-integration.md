---
title: Migration von Integration-Ressourcen von Azure Deutschland zu Azure weltweit
description: Dieser Artikel bietet Unterstützung bei der Migration von Integration-Ressourcen von Azure Deutschland zu Azure weltweit.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 8/15/2018
ms.topic: article
ms.custom: bfmigrate
---

# <a name="migration-of-integration-resources-from-azure-germany-to-global-azure"></a>Migration von Integration-Ressourcen von Azure Deutschland zu Azure weltweit

Dieser Artikel unterstützt Sie bei der Migration von Azure Integration-Ressourcen von Azure Deutschland zu Azure weltweit.

## <a name="service-bus"></a>Service Bus

Service Bus-Dienste verfügen nicht über Datenexport- und -importfunktionen. Um Service Bus von Azure Deutschland zu Azure weltweit zu migrieren, können Sie die Service Bus-Ressourcen [ als Resource Manager-Vorlage](../azure-resource-manager/resource-manager-export-template-powershell.md) exportieren. Übernehmen Sie dann die exportierte Vorlage für Azure weltweit, und erstellen Sie die Ressourcen erneut.

> [!NOTE]
> Dabei werden die Daten (z.B. Nachrichten) nicht kopiert, sondern nur die Metadaten erneut erstellt.


> [!IMPORTANT]
> Ändern Sie den Speicherort, Geheimnisse des Schlüsseltresors, Zertifikate und andere GUIDs, damit diese konsistent mit der neuen Region sind.

### <a name="metadata-service-bus"></a>Metadaten Service Bus

- Namespaces
- Warteschlangen
- Themen
- Abonnements
- Regeln
- Autorisierungsregeln (siehe weiter unten)

### <a name="keys"></a>Schlüssel

Die oben genannten Schritte für den Export bzw. die erneute Erstellung kopieren nicht die den Autorisierungsregeln zugeordneten SAS-Schlüssel. Wenn Sie die SAS-Schlüssel beibehalten müssen, verwenden Sie das Cmdlet `New-AzureRmServiceBuskey` mit dem optionalen Parameter `-Keyvalue`, um den Schlüssel als Zeichenfolge zu akzeptieren. Das aktualisierte Cmdlet finden Sie unter [PowerShell-Katalog Release 6.4.0 (Juli 2018)](https://www.powershellgallery.com/packages/AzureRM/6.4.0) oder auf [GitHub](https://github.com/Azure/azure-powershell/releases/tag/v6.4.0-July2018).

### <a name="usage-example"></a>Verwendungsbeispiel

```powershell
New-AzureRmServiceBuskey -ResourceGroupName <resourcegroupname> -Namespace <namespace> -Name <name of Authorization rule> -RegenerateKey <PrimaryKey/SecondaryKey> -KeyValue <string - key value>
```

```powershell
New-AzureRmServiceBuskey -ResourceGroupName <resourcegroupname> -Namespace <namespace> -Queue <queuename> -Name <name of Authorization rule> -RegenerateKey <PrimaryKey/SecondaryKey> -KeyValue <string - key value>
```

```powershell
New-AzureRmServiceBuskey -ResourceGroupName <resourcegroupname> -Namespace <namespace> -Topic <topicname> -Name <name of Authorization rule> -RegenerateKey <PrimaryKey/SecondaryKey> -KeyValue <string - key value>
```

> [!NOTE]
> Sie müssen Ihre Anwendungen aktualisieren, um eine neue Verbindungszeichenfolge zu verwenden, auch wenn Sie die Schlüssel beibehalten, da die DNS-Hostnamen für Azure Deutschland und Azure weltweit unterschiedlich sind.

### <a name="sample-connection-strings"></a>Beispielverbindungszeichenfolgen

**Azure Deutschland**

```cmd
Endpoint=sb://myBFProdnamespaceName.**servicebus.cloudapi.de**/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=XXXXXXXXXXXXx=
```

**Azure weltweit**

```cmd
Endpoint=sb://myProdnamespaceName.**servicebus.windows.net**/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=XXXXXXXXXXXXx=
```

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu Service Bus anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/service-bus-messaging/#step-by-step-tutorials) auf.
- Machen Sie sich mit den Vorgehensweisen zum [Exportieren einer Azure Resource Manager-Vorlage](../azure-resource-manager/resource-manager-export-template.md) vertraut, oder lesen Sie die Übersicht zu [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

### <a name="references"></a>Referenzen

- [Service Bus: Übersicht](../service-bus-messaging/service-bus-messaging-overview.md)
- [Exportieren einer Resource Manager-Vorlage mithilfe von PowerShell](../azure-resource-manager/resource-manager-export-template-powershell.md#export-resource-group-as-template)







## <a name="logic-apps"></a>Logic Apps

Der Logic Apps-Dienst ist in Azure Deutschland nicht verfügbar. Azure Scheduler (verfügbar) ist allerdings veraltet. Verwenden Sie stattdessen Azure Logic Apps zum Erstellen von Planungsaufträgen.

### <a name="next-steps"></a>Nächste Schritte

- Machen Sie sich mit den Features vertraut, die Azure Logic Apps bietet, indem Sie die [schrittweisen Tutorials](https://docs.microsoft.com/azure/logic-apps/#step-by-step-tutorials) nutzen.

### <a name="reference"></a>Verweis

- [Azure Logic Apps: Übersicht](../logic-apps/logic-apps-overview.md) 