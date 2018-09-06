---
title: Migration von Web-Ressourcen von Azure Deutschland zu Azure weltweit
description: Dieser Artikel bietet Unterstützung bei der Migration von Web-Ressourcen von Azure Deutschland zu Azure weltweit.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 8/15/2018
ms.topic: article
ms.custom: bfmigrate
ms.openlocfilehash: 8c5bae86636774b23da5507936e3dd0002538f05
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43346256"
---
# <a name="migration-of-web-resources-from-azure-germany-to-global-azure"></a>Migration von Web-Ressourcen von Azure Deutschland zu Azure weltweit

Dieser Artikel unterstützt Sie bei der Migration von Azure Web-Ressourcen von Azure Deutschland zu Azure weltweit.

## <a name="app-service---web-apps"></a>App Service – Web-Apps

Die Migration von App Services von Azure Deutschland zu Azure weltweit wird zurzeit nicht unterstützt. Die empfohlene Vorgehensweise besteht im Exportieren als Resource Manager-Vorlage und der erneuten Bereitstellung nach dem Ändern der Standorteigenschaft in die neue Zielregion.

> [!IMPORTANT]
> Ändern Sie den Speicherort, Geheimnisse des Schlüsseltresors, Zertifikate und andere GUIDs, damit diese konsistent mit der neuen Region sind.

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu App Services anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/app-service/#step-by-step-tutorials) auf.
- Machen Sie sich mit den Vorgehensweisen zum [Exportieren einer Azure Resource Manager-Vorlage](../azure-resource-manager/resource-manager-export-template.md) vertraut, oder lesen Sie die Übersicht zu [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

### <a name="references"></a>Referenzen

- [App Service: Übersicht](../app-service/app-service-web-overview.md)
- [Exportieren einer Resource Manager-Vorlage mithilfe von PowerShell](../azure-resource-manager/resource-manager-export-template-powershell.md#export-resource-group-as-template)
- [Azure-Standorte: Übersicht](https://azure.microsoft.com/global-infrastructure/locations/)
- [Erneutes Bereitstellen einer Vorlage](../azure-resource-manager/resource-group-template-deploy.md)












## <a name="notification-hubs"></a>Notification Hubs

Um Einstellungen von einem Notification Hub zu einem anderen zu migrieren, können Sie alle Registrierungstoken zusammen mit Tags exportieren und importieren. Das geht so:

- [Exportieren Sie die vorhandenen Hubregistrierungen](https://msdn.microsoft.com/library/azure/dn790624.aspx) in einen Azure Blob Storage-Container.
- Erstellen Sie einen neuen Notification Hub in der Zielumgebung.
- [Importieren Sie Ihre Registrierungstoken](https://msdn.microsoft.com/library/azure/dn790624.aspx) aus Azure Blob Storage in Ihren neuen Hub.

### <a name="next-steps"></a>Nächste Schritte

Frischen Sie Ihre Kenntnisse zu Notification Hubs anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/notification-hubs/#step-by-step-tutorials) auf.

### <a name="references"></a>Referenzen

- [Übersicht über Notification Hubs](../notification-hubs/notification-hubs-push-notification-overview.md)