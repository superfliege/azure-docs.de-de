---
title: 'Tutorial: Sitzungs- und geräteübergreifendes Freigeben von Azure Spatial Anchors | Microsoft-Dokumentation'
description: In diesem Tutorial wird beschrieben, wie Sie Azure Spatial Anchors-Bezeichner zwischen Android/iOS-Geräten in Unity mit einem Back-End-Dienst freigeben.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: ff9868dd7347812eb6ef566288ec364bc89b6955
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58629306"
---
# <a name="tutorial-share-azure-spatial-anchors-across-sessions-and-devices"></a>Tutorial: Sitzungs- und geräteübergreifendes Freigeben von Azure Spatial Anchors

In diesem Tutorial erfahren Sie, wie Sie [Azure Spatial Anchors](../overview.md) verwenden, um im Rahmen einer Sitzung Anker zu erstellen und diese anschließend auf dem gleichen oder auf einem anderen Gerät zu finden. Die gleichen Anker können auch von mehreren Geräten am gleichen Ort und zur selben Zeit gefunden werden.

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

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine ASP.NET Core-Web-App in Azure bereitgestellt und anschließend eine Unity-App konfiguriert und bereitgestellt. Sie haben Spatial Anchors-Bezeichner mit der App erstellt und mithilfe der ASP.NET Core-Web-App für andere Geräte freigegeben.

Im nächsten Tutorial erfahren Sie, wie Sie Ihre ASP.NET Core-Web-App so optimieren, dass sie Azure Cosmos DB zur Speicherung der freigegebenen Spatial Anchors-Bezeichner nutzt. Azure Cosmos DB sorgt für die Persistenz Ihrer ASP.NET Core-Web-App. Dadurch kann Ihre App anhand des in Ihrer Web-App gespeicherten Ankerbezeichners einen Anker wiederfinden, den sie vor mehreren Tagen erstellt hat.

> [!div class="nextstepaction"]
> [Tutorial: Verwenden von Azure Cosmos DB zum Speichern von Textmarken](./tutorial-use-cosmos-db-to-store-anchors.md)
