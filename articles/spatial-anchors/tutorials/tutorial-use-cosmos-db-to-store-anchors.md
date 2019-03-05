---
title: 'Tutorial: Freigeben für Sitzungen und Geräte mit Azure Spatial Anchors und einem Azure Cosmos DB-Back-End | Microsoft-Dokumentation'
description: In diesem Tutorial wird beschrieben, wie Sie Azure Spatial Anchors-Bezeichner zwischen Geräten in Unity mit einem Back-End-Dienst und Azure Cosmos DB freigeben.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: f0cd42fc37727099ed95a1c6fc2d427b7862412e
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2019
ms.locfileid: "56753455"
---
# <a name="tutorial-sharing-across-sessions-and-devices-with-azure-spatial-anchors-and-an-azure-cosmos-db-back-end"></a>Tutorial: Freigeben für Sitzungen und Geräte mit Azure Spatial Anchors und einem Azure Cosmos DB-Back-End

In diesem Tutorial erfahren Sie, wie Sie [Azure Spatial Anchors](../overview.md) für folgende Zwecke nutzen:

1. Erstellen von Ankern („Anchors“) in einer Sitzung und anschließendes Wiederfinden in einer anderen Sitzung auf demselben oder einem anderen Gerät, beispielsweise an einem anderen Tag
2. Erstellen von Ankern, die von mehreren Geräten gleichzeitig an demselben Ort gefunden werden können

![Persistenz](./media/persistence.gif)

[Azure Spatial Anchors](../overview.md) ist ein plattformübergreifender Entwicklerdienst, mit dem Sie Mixed Reality-Umgebungen mit Objekten erstellen können, die ihre Position im Zeitverlauf geräteübergreifend beibehalten. Nach Abschluss des Vorgangs verfügen Sie über eine App, die auf zwei oder mehr Geräten bereitgestellt werden kann. Für Azure Spatial Anchors, die von einer Instanz erstellt werden, werden die Bezeichner über Cosmos DB mit anderen Ankern gemeinsam verwendet.

Sie lernen Folgendes:

> [!div class="checklist"]
> * Bereitstellen einer ASP.NET Core-Web-App in Azure, die zum Freigeben von Ankern verwendet werden kann (Speicherung in Cosmos DB)
> * Konfigurieren der AzureSpatialAnchorsLocalSharedDemo-Szene im Unity-Beispiel aus den Schnellstartanleitungen, um die Sharing Anchors-Web-App zu nutzen
> * Bereitstellen und Ausführen auf einem oder mehreren Geräten

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-share-sample-prereqs.md)]

Beachten Sie Folgendes: Sie verwenden in diesem Tutorial zwar Unity und Azure Cosmos DB, aber dies dient nur als Beispiel für die geräteübergreifende Freigabe von Azure Spatial Anchors-Bezeichnern. Sie können auch andere Sprachen und Back-End-Technologien verwenden, um dieses Ziel zu erreichen. Darüber hinaus verfügt die in diesem Tutorial verwendete ASP.NET Core-Web-App über eine Abhängigkeit vom .NET Core 2.2 SDK. Dies funktioniert gut für reguläre Azure-Web-Apps (für Windows), aber derzeit nicht für Azure-Web-Apps für Linux.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

[!INCLUDE [cosmos-db-create-dbaccount-table](../../../includes/cosmos-db-create-dbaccount-table.md)]

Notieren Sie sich den Inhalt von `Connection String`, da Sie ihn später noch benötigen.

## <a name="deploy-your-sharing-anchors-service"></a>Bereitstellen Ihres Diensts für die Freigabe von Ankern (Sharing Anchors)

Öffnen Sie Visual Studio und dann das Projekt im Ordner `Sharing\SharingServiceSample`.

### <a name="configure-the-service-so-that-it-uses-your-cosmos-db"></a>Konfigurieren des Diensts für die Verwendung von Cosmos DB

Öffnen Sie im **Projektmappen-Explorer** die Datei `SharingService\Startup.cs`.

Suchen Sie oben in der Datei nach der Zeile `#define INMEMORY_DEMO`, und kommentieren Sie sie aus. Speichern Sie die Datei.

Öffnen Sie im **Projektmappen-Explorer** die Datei `SharingService\appsettings.json`.

Suchen Sie nach der `StorageConnectionString`-Eigenschaft, und legen Sie den Wert auf den `Connection String`-Text fest, den Sie sich im Schritt [Erstellen eines Datenbankkontos](#create-a-database-account) notiert haben. Speichern Sie die Datei.

[!INCLUDE [Publish Azure](../../../includes/spatial-anchors-publish-azure.md)]

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Azure Cosmos DB verwendet, um Ankerbezeichner geräteübergreifend freizugeben. Weitere Informationen zur Azure Spatial Anchors-Bibliothek erhalten Sie, indem Sie unseren Leitfaden zum Erstellen und Suchen nach Ankern weiter durcharbeiten.

> [!div class="nextstepaction"]
> [Create and locate anchors using Azure Spatial Anchors](../create-locate-anchors-overview.md) (Erstellen und Suchen nach Ankern mit Azure Spatial Anchors)
