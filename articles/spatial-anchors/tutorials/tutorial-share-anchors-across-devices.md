---
title: 'Tutorial: Freigeben für Sitzungen und Geräte mit Azure Spatial Anchors | Microsoft-Dokumentation'
description: In diesem Tutorial wird beschrieben, wie Sie Azure Spatial Anchors-Bezeichner zwischen Android/iOS-Geräten in Unity mit einem Back-End-Dienst freigeben.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 7d9fe58b7db60513eed81aae628ebd7ca754a53a
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57901303"
---
# <a name="tutorial-sharing-across-sessions-and-devices-with-azure-spatial-anchors"></a>Tutorial: Freigeben für Sitzungen und Geräte mit Azure Spatial Anchors

In diesem Tutorial erfahren Sie, wie Sie [Azure Spatial Anchors](../overview.md) für folgende Zwecke nutzen:

1. Erstellen von Ankern („Anchors“) in einer Sitzung und anschließendes Wiederfinden in einer anderen Sitzung auf demselben oder einem anderen Gerät, beispielsweise an einem anderen Tag
2. Erstellen von Ankern, die von mehreren Geräten gleichzeitig an demselben Ort gefunden werden können

![Persistenz](./media/persistence.gif)

Azure Spatial Anchors ist ein plattformübergreifender Entwicklerdienst, mit dem Sie Mixed Reality-Umgebungen mit Objekten erstellen können, die ihre Position im Zeitverlauf geräteübergreifend beibehalten. Nach Abschluss des Vorgangs verfügen Sie über eine App, die auf zwei oder mehr Geräten bereitgestellt werden kann. Von einer Instanz erstellte Azure Spatial Anchors-Bezeichner können für die anderen Instanzen freigegeben werden.

Sie lernen Folgendes:

> [!div class="checklist"]
> * Bereitstellen einer ASP.NET Core-Web-App in Azure, die zum Freigeben von Ankern verwendet werden kann (Speicherung im Arbeitsspeicher für einen bestimmten Zeitraum)
> * Konfigurieren der AzureSpatialAnchorsLocalSharedDemo-Szene im Unity-Beispiel aus den Schnellstartanleitungen, um die Sharing Anchors-Web-App zu nutzen
> * Bereitstellen und Ausführen auf einem oder mehreren Geräten

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-share-sample-prereqs.md)]

Beachten Sie Folgendes: Sie verwenden in diesem Tutorial zwar Unity und eine ASP.NET Core-Web-App, aber dies dient nur als Beispiel für die geräteübergreifende Freigabe von Azure Spatial Anchors-Bezeichnern. Sie können auch andere Sprachen und Back-End-Technologien verwenden, um dieses Ziel zu erreichen. Darüber hinaus verfügt die in diesem Tutorial verwendete ASP.NET Core-Web-App über eine Abhängigkeit vom .NET Core 2.2 SDK. Dies funktioniert gut für reguläre Azure-Web-Apps (für Windows), aber derzeit nicht für Azure-Web-Apps für Linux.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project-in-unity"></a>Öffnen des Beispielprojekts in Unity

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

## <a name="deploy-your-sharing-anchors-service"></a>Bereitstellen Ihres Diensts für die Freigabe von Ankern (Sharing Anchors)

Öffnen Sie Visual Studio und dann das Projekt im Ordner `Sharing\SharingServiceSample`.

[!INCLUDE [Publish Azure](../../../includes/spatial-anchors-publish-azure.md)]

## <a name="open-the-sample-project-in-unity"></a>Öffnen des Beispielprojekts in Unity

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine ASP.NET Core-Web-App in Azure bereitgestellt und anschließend eine Unity-App konfiguriert und bereitgestellt. Sie haben Spatial Anchors-Bezeichner mit der App erstellt und mithilfe der ASP.NET Core-Web-App für andere Geräte freigegeben.

Wenn Sie erfahren möchten, wie Sie Ihre ASP.NET Core-Web-App optimieren, sodass sie Azure Cosmos DB zur Speicherung der freigegebenen Spatial Anchors-Bezeichner nutzt, fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Tutorial: Verwenden von Azure Cosmos DB zum Speichern von Textmarken](./tutorial-use-cosmos-db-to-store-anchors.md)
