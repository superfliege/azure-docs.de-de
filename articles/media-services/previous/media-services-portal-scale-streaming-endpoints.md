---
title: Skalieren von Streamingendpunkten mithilfe des Azure-Portals | Microsoft Docs
description: Dieses Tutorial führt Sie durch die Schritte zur Skalierung von Streamingendpunkten mithilfe des Azure-Portals.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: 1008b3a3-2fa1-4146-85bd-2cf43cd1e00e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.author: juliako
ms.openlocfilehash: f2a14f2622d78a4222a8518172eb1ce8ed9e6637
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33785639"
---
# <a name="scale-streaming-endpoints-with-the-azure-portal"></a>Skalieren von Streamingendpunkten mithilfe des Azure-Portals
## <a name="overview"></a>Übersicht

> [!NOTE]
> Um dieses Tutorial abzuschließen, benötigen Sie ein Azure-Konto. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

**Premium**-Streamingendpunkte eignen sich für erweiterte Workloads und bieten eine dedizierte und skalierbare Bandbreitenkapazität. Kunden mit einem **Premium**-Streamingendpunkt erhalten standardmäßig eine einzelne Streamingeinheit (Streaming Unit, SU). Der Streamingendpunkt kann durch Hinzufügen von SUs skaliert werden. Jede SU bietet zusätzliche Bandbreitenkapazität für die Anwendung. Weitere Informationen zu Streamingendpunkt-Typen und der CDN-Konfiguration finden Sie im Thema [Übersicht über Streamingendpunkte](media-services-streaming-endpoints-overview.md).
 
In diesem Thema wird gezeigt, wie ein Streamingendpunkt skaliert wird.

Informationen zu den Preisen finden Sie unter [Mediendienste – Preisübersicht](http://go.microsoft.com/fwlink/?LinkId=275107).

## <a name="scale-streaming-endpoints"></a>Skalieren von Streamingendpunkten

Gehen Sie wie folgt vor, um die Anzahl von Streamingeinheiten zu ändern:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihr Azure Media Services-Konto aus.
2. Klicken Sie im Fenster **Einstellungen** auf **Streamingendpunkte**.
3. Klicken Sie auf den Streamingendpunkt, den Sie skalieren möchten. 

    > [!NOTE] 
    > Sie können nur **Premium**-Streamingendpunkte skalieren.

4. Verschieben Sie den Schieberegler, um die Anzahl der Streamingeinheiten anzugeben.

    ![Streamingendpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die Media Services-Lernpfade.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

