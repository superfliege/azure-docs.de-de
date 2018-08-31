---
title: Migration von IoT-Ressourcen von Azure Deutschland zu Azure weltweit
description: Dieser Artikel bietet Unterstützung bei der Migration von IoT-Ressourcen von Azure Deutschland zu Azure weltweit.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 8/15/2018
ms.topic: article
ms.custom: bfmigrate
---

# <a name="migration-of-internet-of-things-iot-resources-from-azure-germany-to-global-azure"></a>Migration von IoT-Ressourcen (Internet der Dinge) von Azure Deutschland zu Azure weltweit

Dieser Artikel unterstützt Sie bei der Migration von Azure IoT-Ressourcen von Azure Deutschland zu Azure weltweit.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB

Mit dem Azure Cosmos DB-Datenmigrationstool können Sie problemlos Daten zu Azure Cosmos DB migrieren. Das Azure Cosmos DB-Datenmigrationstool ist eine Open-Source-Lösung, mit der Daten aus verschiedenen Quellen in Azure Cosmos DB importiert werden können.

Das Tool steht als GUI-Tool oder als Befehlszeilentool zur Verfügung. Der Quellcode ist im GitHub-Repository für das [Azure Cosmos DB-Datenmigrationstool](https://github.com/azure/azure-documentdb-datamigrationtool) verfügbar, und eine kompilierte Version steht im [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=46436) zur Verfügung.

Die folgenden Schritte werden empfohlen:

- Führen Sie eine Überprüfung der Anforderungen an die Anwendungsbetriebszeit und der Kontokonfigurationen aus, um den richtigen Aktionsplan zu empfehlen.
- Führen Sie die Schritte zum Klonen der Kontokonfigurationen aus Azure Deutschland in die neue Region aus, indem Sie das Tool ausführen.
- Wenn ein Wartungsfenster möglich ist, führen Sie die Schritte zum Kopieren von Daten aus der Quelle in das Ziel aus, indem Sie das Tool ausführen.
- Wenn kein Wartungsfenster möglich ist, führen Sie die Schritte zum Kopieren von Daten aus der Quelle in das Ziel aus, indem Sie das Tool und den von uns empfohlenen Prozess ausführen.
  - Nehmen Sie Änderungen an den Lese-/Schreibvorgängen in der Anwendung mit einem konfigurationsgesteuerten Ansatz vor.
  - Führen Sie die erste Synchronisierung aus.
  - Richten Sie inkrementelle Synchronisierung/Berücksichtigung des Änderungsfeeds ein.
  - Verweisen Sie die Lesevorgänge auf das neue Konto, und überprüfen Sie die Anwendung.
  - Beenden Sie Schreibvorgänge in das alte Konto, überprüfen Sie, ob der Änderungsfeed berücksichtigt wird, und verweisen Sie dann Schreibvorgänge auf die neuen Konten.
  - Beenden Sie das Tool, und löschen Sie das alte Konto.
- Führen Sie die Schritte zum Ausführen des Tools aus, um zu überprüfen, ob die Daten in den alten und neuen Konten konsistent sind.


### <a name="references"></a>Referenzen

- [Azure Cosmos DB](../cosmos-db/introduction.md)
- [Importieren von Daten in Azure Cosmos DB](../cosmos-db/import-data.md)










## <a name="functions"></a>Functions

Die Migration von Functions zwischen Azure Deutschland und Azure weltweit wird zurzeit nicht unterstützt. Die empfohlene Vorgehensweise besteht darin, die Resource Manager-Vorlage zu exportieren, den Speicherort zu ändern und eine erneute Bereitstellung in der Zielregion vorzunehmen.

> [!IMPORTANT]
> Ändern Sie den Speicherort, Geheimnisse des Schlüsseltresors, Zertifikate und andere GUIDs, damit diese konsistent mit der neuen Region sind.

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu Functions anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/azure-functions/#step-by-step-tutorials) auf.
- Machen Sie sich mit den Vorgehensweisen zum [Exportieren einer Azure Resource Manager-Vorlage](../azure-resource-manager/resource-manager-export-template.md) vertraut, oder lesen Sie die Übersicht zu [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

### <a name="references"></a>Referenzen

- [Übersicht zu Azure Functions](../azure-functions/functions-overview.md)
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








## <a name="iot-hub"></a>IoT Hub

Eine nahtlose Migration von IoT Hub-Instanzen von Azure Deutschland zu Azure weltweit ist nicht möglich. Sie können die folgenden Schritte ausführen, um IoT Hub-Instanzen von Azure Deutschland zu Azure weltweit zu migrieren.

> [!NOTE]
> Diese Migration kann dazu führen, dass Ausfallzeiten und Datenverluste für Ihre IoT-Anwendung auftreten. Alle Telemetriemeldungen, C2D-Befehle und auftragsbezogenen Informationen (Zeitpläne und Verlauf) werden nicht zu Azure weltweit migriert. Sie müssen Ihre Geräte und Back-End-Anwendungen neu konfigurieren, damit beim Starten die neuen Verbindungszeichenfolgen verwendet werden.

### <a name="step-1---recreate-iot-hub"></a>Schritt 1: Erneutes Erstellen des IoT Hubs

IoT Hub unterstützt nativ keine Klonvorgänge. Sie können jedoch das Feature Azure Resource Manager verwenden, um [eine Ressourcengruppe als Vorlage](../azure-resource-manager/resource-manager-export-template-powershell.md) zu exportieren, um Ihre IoT Hub-Metadaten (einschließlich konfigurierter Routen und anderer IoT-Hub-Einstellungen) zu exportieren und sie in Azure weltweit erneut bereitzustellen. Möglicherweise finden Sie es auch einfacher, den IoT Hub im Portal erneut zu erstellen, indem Sie sich die Details im exportierten JSON ansehen.

### <a name="step-2---migrate-device-identities"></a>Schritt 2: Migrieren von Geräteidentitäten

- Verwenden Sie im Quellmandanten in Azure Deutschland die Resource Manager-API [ExportDevices](../iot-hub/iot-hub-bulk-identity-mgmt.md), um alle Geräteidentitäten, Gerätezwillinge und Modulzwillinge (einschließlich der Schlüssel) in einen Speichercontainer zu exportieren. Sie können einen Speichercontainer in Azure Deutschland oder Azure weltweit verwenden. Stellen Sie nur sicher, dass der generierte SAS-URI über ausreichende Berechtigungen verfügt. 
- Führen Sie die Azure Resource Manager-API [ImportDevices](../iot-hub/iot-hub-bulk-identity-mgmt.md) aus, um alle Geräteidentitäten aus dem Speichercontainer in den geklonten IoT Hub in Azure weltweit zu importieren.
- Konfigurieren Sie Ihre Geräte und Back-End-Anwendungen neu, damit beim Starten die neuen Verbindungszeichenfolgen verwendet werden. Der Hostname ändert sich aus `*.azure-devices.de` in `*.azure-devices.com`.  

> [!NOTE]
> Die Stamm-CA unterscheidet sich für Azure Deutschland und Azure weltweit, sodass dies bei der Neukonfiguration Ihrer Geräte und Back-End-Anwendungen, die mit der IoT Hub-Instanz interagieren, berücksichtigt werden muss.

### <a name="next-steps"></a>Nächste Schritte

- [Massenexport der IoT Hub-Identität](../iot-hub/iot-hub-bulk-identity-mgmt.md#export-devices)
- [Massenimport der IoT Hub-Identität](../iot-hub/iot-hub-bulk-identity-mgmt.md#import-devices)

### <a name="references"></a>Referenzen

- [Azure IoT Hub: Übersicht](../iot-hub/about-iot-hub.md)
